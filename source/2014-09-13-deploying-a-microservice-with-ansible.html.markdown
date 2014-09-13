---
title: Deploying a Microservice With Ansible
date: 2014-09-13 16:28 UTC
tags: deployment, node, ansible
---

#Deploying a Microservice With Ansible

##The Problem

Some time ago, happily tending our app behind the tall walls of Heroku-town,
we faced a problem: long-running requests to a Heroku dyno block it
(or at least one process on the dyno) from processing any further requests, but
Heroku's router may still send requests to that dyno, resulting in timeouts.

Our app processes image uploads from both web and mobile clients, and does some
thumbnailing of the images, so direct to S3 uploads were not an option and
we needed a server to handle these uploads outside of Heroku.

##The Quick Fix

One of our engineers whipped up a Node.js app which would:

1. Accept a file upload
2. Mimic the thumbnailing processes performed by Paperclip on our Rails app
3. Send the files to S3
4. Call back to the Rails app with the image metadata, so we could create an
   Image record on the server

Everything worked nicely and we stopped seeing timeouts on our Rails app.

##The Problems With The Fix

Everyone on our team at the time had some basic knowledge of Node and
intermediate Linux admin skill. But since these image uploads were critical
to our user experience, intermediate was not enough.  Any bug with this service
was potentially critical, so any botched configurations -- whether they were
the bug or part of someone's attempt to fix a bug -- could have quite a negative
impact.  So config files became scary to touch, and we wanted to carefully
monitor the service for some time after any change to these

Deployment was also not fully automated, so if anyone forgot a step, it could
result in bugs.

##The Ideal Solution

After some high stress bugfixes and unacceptably long outages, we came up with
a list of requirements (a few of which had been satisfied by the original developer,
hence the config files I mentioned). The service should:

1. Be easy to deploy to either a running instance or a clean box.
2. Bring itself back up if it failed.
3. Manage its logs.
4. Allow for easy restoration of any config files modified during troubleshooting.

The tool we chose for this (and more, detailed below) was Ansible. [Here's the best
introductory guide I've seen](https://serversforhackers.com/editions/2014/08/26/getting-started-with-ansible/).

I'd like to highlight a few excerpts from our Ansible playbook, because while this is
not a full tutorial and far from a "best practices"
guide, it does cover a few things that might get left out of other beginner level guides.

##Ansible tips

###Installing updated packages

Install versions of nginx and node that were relased after, say, the closing
of the Western frontier:

*playbook.yml*

```yaml

    - name: Package prerequisites for node.js
      action: apt pkg=python-software-properties state=installed

    - name: Add the node.js PPA
      apt_repository: repo="ppa:chris-lea/node.js"

    - name: Add the nginx PPA
      apt_repository: repo="ppa:nginx/stable"
```

###Installing and configuring upstart

Install upstart, which allows you to easily start and stop processes, and check
their status, like so:

```bash
sudo image-manager status
sudo image-manager restart
sudo image-manager stop
sudo image-manager start
```

[The docs are thorough if a bit intimidating](http://upstart.ubuntu.com/cookbook)

*playbook.yml*

```yaml
    - name: install upstart
      apt: pkg=upstart state=latest

    - name: (PROD) copy upstart config file to /etc/init
      copy: src=config/image-manager.conf
            dest=/etc/init/image-manager.conf
```

The config file:

```
#!upstart
description "Image Manager Production App"

start on startup
stop on shutdown
console output

#Important: this means upstart will expect the process it is
#managing to call `fork` exactly twice.  For our app, I
#distinguished between this and "expect fork" by trial and error
#
#see http://upstart.ubuntu.com/cookbook/#expect-daemon

expect daemon

script
  export HOME="/home/ubuntu"
  cd $HOME/image-manager
  sudo NODE_ENV=production \
       PORT=5555 \
       /usr/bin/node /home/ubuntu/image-manager/app.js >> \
       /var/log/image-manager.log &
end script

#Upstart has to know about the process id of all the processes it monitors
#This is the only way I could find to get that

post-start script
  upstart_pid=$(status image-manager | awk '{print $NF}')
  sudo echo $upstart_pid > /var/run/image-manager.pid
end script
```

###Installing ntpd

This is the Network Time Protocol daemon, and installing it will
prevent clock drift and ensure that, say, S3 won't decide to stop
talking to your server during the second half of the BCS Championship game
because your server thinks it's living 16 minutes in the future.

*playbook.yml*

```yaml

    - name: ensure ntpd is at the latest version
      apt: pkg=ntp state=latest
      notify:
      - restart ntpd

    #...

    handlers:
    - name: restart ntpd
      service: name=ntp state=restarted
```

This uses an Ansible handler. It's basically the same as a task, but is just
called by other tasks like a callback.

###Setting up logrotate

Don't let your logs fill up your hard drive:

*playbook.yml*

```yaml

    - name: install logrotate
      apt: pkg=logrotate state=latest

    - name: copy logrotate config file
      copy: src=config/logrotate
            dest=/etc/logrotate.d/image-manager
```

[Guide to logrotate](http://www.rackspace.com/knowledge_center/article/understanding-logrotate-utility)

*/etc/logrotate.d/image-manager*

```bash
/var/log/*image*.log {
daily
compress
copytruncate
size 2M
rotate 4
}
```


###Setting up monit

Monit will periodically check the status of your services and bring them back
up as needed, optionally also sending you alerts or taking other actions if it
detects that a service is down.  In this sense, it has some overlap with upstart,
but I find it easier to use, it makes an actual http request instead of just
checking the pid file, and I like to stick to upstart just for enabling
start/stop commands, and starting services on server startup.

```yaml
    - name: copy monit config file
      copy: src=config/monitrc
            dest=/etc/monit/monitrc
            mode=0700

    #this might not be needed, I forgot why it's here
    - name: reload monit
      command: monit reload

    - name: monitor collage service
      monit: name=image-manager state=monitored
```

We set it up to check the service every minute, with a 10 second timeout.

The config file:

```bash
set logfile /var/log/monit.log
set daemon 60

#Even if you don't use the web interface for monit,
#this has to be set up:
set httpd port 2813 and
  use address localhost
  allow localhost

check process image-manager with pidfile "/var/run/image-manager.pid"
    start program = "/sbin/start image-manager"
    stop program  = "/sbin/stop image-manager"
    if failed port 5555 protocol HTTP
        request /
        with timeout 10 seconds
        then restart
```

###Uptime monitoring

We use pingdom.com for this.  We could set up monit to send an email when the
process goes down, but we also want notifications if the whole box is unreachable.

###Other notes

This isn't really part of the playbook, and probably goes without saying, but
if you don't explicitly specify the versions of your dependencies, you don't
have a repeatable deployment process.


There's a lot more that can be done with Ansible, including just for organizing
a simple playbook like ours.  It's a great tool that we will use more in the future.

##Deployment

To deploy we have a `hosts` file which specifies the machine(s) that the ansible playbook
can reference, which looks like this:

```
[aws]
ec2-123456.compute-1.amazonaws.com ansible_ssh_private_key_file=secret.pem
```

And to deploy we can run:

```
ansible-playbook -i hosts playbook.yml -u ubuntu --extra-vars "env=prod"
```

This will ensure all our config files match what we have in our playbook, pull
down the latest master branch from the service's Github repo, and restart the
image-manager service.

If we were to need to deploy to a new AWS box, we can run:

```
  vagrant provision web --provider=aws`
```

This is set up through Vagrant, and the config looks like this:

```ruby
   web.vm.provider :aws do |aws, override|
      aws.access_key_id = "secret"
      aws.secret_access_key = "secret"
      # ubuntu AMI
      aws.ami = "ami-1d8c9574"
      aws.instance_type = "m3.medium"
      aws.keypair_name = "secret"
      aws.security_groups = ["quicklaunch-1"]

      aws.tags = {
        'Name' => 'Img Manager'
      }

      web.vm.box = "dummy"
      web.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
      web.ssh.username = "ubuntu"
      web.ssh.private_key_path = "secret.pem"
    end
```

