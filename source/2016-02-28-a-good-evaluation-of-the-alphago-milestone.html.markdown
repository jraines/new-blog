---
title: A good evaluation of the AlphaGo milestone
date: 2016-02-28 23:49 UTC
tags:
---

I believe the following is a reasonable take on what the AlphaGo program & win "means" for AI, regardless of whether it wins or loses vs. Lee Sedol:

> ...progress so far has largely been toward demonstrating general approaches for building narrow systems rather than general approaches for building general systems. Progress toward the former does not entail substantial progress toward the latter. The latter, which requires transfer learning among other elements, has yet to have its Atari/AlphaGo moment, but is an important area to keep an eye on going forward, and may be especially relevant for economic/safety purposes. This suggests that an important element of rigorously modeling AI progress may be formalizing the idea of different levels of generality of operating AI systems (as opposed to the generality of the methods that produce them, though that is also important).

[Source](http://www.milesbrundage.com/blog-posts/alphago-and-ai-progress)

This opinion is based on my understanding of AlphaGo, which uses two neural networks, a value network and a policy network, trained on a corpus of Go games between experts, to narrow the search space to be explored by Monte Carlo tree search.

