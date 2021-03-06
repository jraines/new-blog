---
title: A Retrospective on Two Years as a Remote Startup Developer
date: 2014-01-18
tags: remote
---

#A Retrospective on Two Years in a Startup as a Remote Developer

Tomorrow morning, I’ll board a flight from LAX to JFK with a wardrobe woefully ill-equipped, in both style and thermal insulation, for New York City, and this will mark the end of 2 years working with, and later leading, a startup engineering team from the comfort of my home.

I want to share my thoughts on this experience. As always, your mileage may vary.

##Onboarding

The beginning may be the hardest phase for a remote employee. A theme of remote work is that communication is hard and needs special attention, because context clues are lacking or present in forms we’re not naturally accustomed to picking up on. It can be intimidating and difficult to learn how the company works, what the culture is like, what is expected of you, and in what timeframe.

I was hired along with a CTO, who was also CTO at my previous company, so I was counting on him to help me ramp up: I could get face time since he was in my local area, and ask any question without worrying about undermining my credibility with the current team. Less than a month in, he left the company. I panicked, and tried to resign, but in a show of confidence my CEO convinced me to stay.

Over the next two months I would get to know my new team: three consultant developers (in Philadelphia, India, and Iraq), a product manager, the CEO acting as product owner (both in New York), and myself (in California).

At this time we took most of our technical direction and process structure from the most senior of our consultants, who in turn had cribbed some of the good ideas from the Thoughtbot Playbook, and he helped me step up my git practices and TDD skill, which are both extra important in a distributed team (the full argument for that is probably a separate essay, and one that’s already been written by someone else). We had one week product iterations with a retrospective call & planning call (9:30 AM Eastern on Mondays & Wednesdays).

I suspect that some aspects of culture — some strong and some weak points— will emerge on any remote team based solely on the timezone distribution of the team members. I suffered a bit from the 6:30 AM calls (which would later be mercifully moved back a bit), not just because I’m a night owl, but because starting the day with a long conference call made it hard for me to get into a coding flow for some time after the call. I’d usually go eat breakfast, then come back to the computer in prime “chat room time” — everyone on, following up on issues raised in the call or in pull requests from the night before. So on two out of five weekdays, it would be after lunch before I got into much coding of my own. On the other hand, having some overlap with two team members near the end of their day was helpful, because their work for the day was done and in pull requests, and the U.S. team could give feedback at the start of our day and issues could usually be addressed, and the pull requests merged, before they logged off.

Three months in, we hired a senior front-end developer/designer and embarked on a major new feature. From my point of view, and to the best of my recollection, this went well — better than some later large features. However, the developer decided to leave the company afterwards, and this was a setback for the company.

Often when you read about distributed teams, one of the major advantages espoused is that it’s easier to hire. This is undoubtedly true: you have a larger pool of talent to draw from, and remote positions are attractive. I wonder if it’s slightly harder to retain, though — I am curious about other teams’ experiences in the first few months after hiring, because I think with remote jobs it’s easy for the candidate to have one foot out the door for a bit longer, and also for the company to consider them in a “trial phase” for longer. It’s easier to quit, or to let someone go, if a couple months have passed and the candidate doesn’t feel fully integrated, or the company is still unsure about the fit, and you don’t see each other face to face every day.

Onboarding, while always key, should be even more of a strategic priority for distributed teams.

##Getting Things Done

The next few months were spent revamping some existing web app functionality and adding some new features to support some marketing promotions. I don’t have much to say specifically about this time period, so I want to list some general obvservations which were becoming clear.

###Automate important notifications

This is part of taking care of the boring things first. To minimize the number of messages you must manually send, set automated notifications. Be judicious. Here are some good candidates:

- Deploys
- Completed work is ready for testing
- A build passed (or failed)
- A commit has gone into master
- The site is down

Send to email and/or group chat to taste. Don’t put too much noise into the chat room(s) where non-technical team members hang out. For email, Gmail filters can go along way towards efficiently monitoring these.

###Good Writing is Everything

Once you’ve automated some of your important communication channels, nearly everything written is an outgoing signal. Not just emails, but commit messages, comments on pull requests, comments on bug tickets, etc.

Economy and specificity are key. The more you write, the more opportunity for misunderstanding there is. If you find yourself writing more than a paragraph, pick up the phone.

Your next actions and requests for other people to take action should be specific. If you want someone to take an action based on what you are communicating, ask for it specifically.

###Push Development/Feature Branches

Often This is the best status update (among the engineering team at least — don’t overestimate how closely management is monitoring chat, even if they’re in the room).

Your commit messages should be well written, and you should think about keywords when writing them, because you will want to have a searchable, informative commit log.

My former colleague Len Smith has a [great blog post](http://www.barrison.com/manipulate-history-for-meanginful-commits-with-git/) about when to commit while doing TDD in your feature branches to allow yourself flexibility to experiment & revert, and then using an interactive rebase to create an informative final commit message.

###Roger Wilco

If you are told something, acknowledge it, even with an “OK” or “Hmm”. If you are asked something, give a clear answer. If you don’t understand, say so.

Nothing is quite so frustrating as being in the middle of the conversation with someone and they just suddenly become unresponsive. It takes less than a second to type “brb phone” or similar, so don’t leave people hanging.

Set up alerts on your name in group chat (or encourage use of the public direct message feature) so that you can be responsive without constant active monitoring of the chat.

###Findability

Many discussions and decisions will be generated about any given piece of work. There should be one place where that can be referred to (either directly or via links).

> “Work should have a URL” --Ryan Tomayko, CTO, GitHub

I believe this is one of those areas where a policy should be in place, and someone should be responsible for enforcing this property of the system.

###Flow

Engineers need to get in the zone, and email and instant messages are the enemy of the zone. People must be able to set their availability level, and it must be respected. Hours of heads down work should be OK (days heads down, not so much).

If you’re a tech lead or engineering manager, you have to allow yourself the opportunity to get into flow, too. Don’t shy away from setting boundaries, having office hours, or turning down/rescheduling meetings. And checking email first thing in the morning is still a productivity destroyer even if you are starting your day three hours behind the rest of the team. Some of my most badly derailed days were a result of checking email before I even got out of bed.

###Boundaries & Burnout

The ease of online asynchronous communications makes it tempting to be available all the time. This is how work creeps into your personal life and blurs the boundaries between working/not-working. I suffered badly from this and it was partially related to the timezone split and my own daily schedule.

When the East Coast people would start rolling off for dinner around 4pm my time, I’d go for a walk on the beach (chalk one in the “pro” column of the remote work scorecard) to get some fresh air and watch the sunset.

This helped me de-stress and let new ideas form unbidden, by taking my mind off the things I’d been consciously focusing on all day.

But once I got back, I’d often get right back to work, because this was now my precious “alone time” for development work that required uninterrupted focus and/or unbounded exploration. Then my girlfriend would come home and be ready to unwind from her day, and sometimes I’d resent the “intrusion” — especially if I was working on something that had to be done, which, in a startup, was quite often. Even if I didn’t think I showed it, or even if I didn’t actually resent it at all, it was a burden on her to feel like she had to tiptoe around me or be sure that I was really done (since sometimes my transition for work to not-work was just closing a few windows & tabs).

I’ve gotten better about this and usually unapologetically say to coworkers that I have to take a break, or be done for the evening, or just log off. I wish I’d adopted that mentality & practice sooner.

> Burnout is about resentment. It’s about knowing what matters to you so much that if you don’t get it that you’re resentful. —Marissa Mayer

A lot of good counterpoints have been made to that quote—burnout is certainly about more than resentment—but I believe there’s some truth in it. It’s easy to have a bit of “remote guilt” that will cause you to be extra available, or work extra hard, sacrificing small but important moments, in the name of demonstrating—whether to yourself or to others—that you deserve the privilege of working from home (not to mention that next raise). Even if that sense of obligation comes primarily from your own head, you can get resentful, and begin to feel burnt out.

###The Chat Room

Don’t develop a culture of saying “goodnight” in chat. If normal working hours are past, and you’re ready to be done for the day, just log off. If someone does mention that they’re logging off, that’s probably an invitation to hold them up if there’s something immediate that needs attention before they go, not an invitation to chit chat for another 10 or 15 minutes while they anxiously look for a polite way to say, “OK, logging off for real now”.

Encourage people to be as public as possible. One on one chats almost always contain information that would be valuable in the public chat. If people are engaging in a lot of back channel chats, there’s probably a culture problem. These can be stealthy and insidious on a distributed team, and I encourage you to read Shanley’s (EDIT: she removed it and I'm not sure if it's been reposted elsewhere) essay on the subject, from which these two excerpts are taken:

> Create opportunities for team members to bond in places besides group chat — regional meetups, video chats, etc. can humanize and evolve the culture. Insist on maintaining professional rapport even in online communications — it’s a job, not a frat house. Avoid a workaholic culture where teammates rely on work for social connections they should be getting outside of work
> …we form connections, even relationships using what comes down to a nub of humanity and the narrowest of windows into the soul: a screen name and a few lines of text. To make it work, we create elaborate webs of inside jokes, shared speech norms, memes, emoticons, image macros. Mythology of the company and mythology of each other. In order to function as tools of connection and belonging, these “in-group” mechanisms must lose granularity, variety and evolution in their need to mediate wide, loosely connected groups

###Don’t fight in the chat room.

It’s fine to disagree & argue, but the moment an argument seems intractable, personal, or emotional, you should immediately suggest a call to discuss it by voice or video. When things get emotional, you need the context clues that are absent from text alone.

###Outsourcing and “Trial Projects”

We’ve had a lot of success with consultants, with one glaring failure that is akin to the failure of effective onboarding. Consultants should be treated & managed as part of the team. In my limited experience, siloing them/putting them on a “parallel track” simply doesn’t work. In our case, the failed case was one where we engaged a team to build a very important feature for us, but none of our own team members were fully on the project. We managed it solely by reviewing pull requests, and this led to a “frog in the frying pan” situation. Each individual PR was good enough, but after a couple months we had to admit that we were screwed — any changes to the code would be expensive and difficult. A month or two later we did an intense, high-stakes rewrite that thankfully turned out well, but could’ve been avoided.

On a couple occasions when we made a new full-time hire (2 of 3 remote, the other partially remote), we made basically the same mistake. With good intentions, we wanted to give them a greenfield project so they could move quickly and display their skill and feel comfortable. What we should’ve done was have them pair program extensively on the new project and on legacy code. This would’ve reduced the risk of project failure and helped integrate them into the team (or at the worst, make a confident decision that they weren’t a fit).

##In Summary

- Pay special attention to how you onboard new remote hires, extra contact and attention is needed for at least a month.
- Do the boring things first: automated notifications, set up CI, establish clear spaces where decisions and discussion are visible & linkable. Keep your feedback loops tight.
- For that same reason: Encourage, and enforce if needed, certain standards of responsiveness and etiquette in chat. If you’re available, you should be responsive. If someone’s not available, respect that (barring emergencies, as always).
- Encourage people to feel free to disconnect when they need a break or reach a logical stopping point in their day. No one should feel obligated to stay around just because someone else is still in the room (especially given timezone differences).
- Don’t let chat be the only source of interaction, camaraderie, and company culture.
- Don’t wait until everyone is offline to do work that requires uninterrupted focus (but be professional enough to not have the Do Not Disturb sign up all day)
- Encourage good writing everywhere: branch names, commit messages, bug reports, feature requests, release notes, etc.
- When things get emotional, pick up the phone — don’t fight in the chat room or over email.

____________________
