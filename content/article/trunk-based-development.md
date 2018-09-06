---
title: "Trunk Based Development"
date: "2018-09-06"
type: "article"
---

When it comes to software development and release workflows, there are a number of strategies that can be followed. A common way of thinking about this
is to look at the size of the team working on it, or the type of project. For instance, a small, scrappy team of where everyone wears multiple hats would
work in a significantly different manner than a bigger team. Similarly, development of a client library would typically be different from that of a self-hosted
executable. It is a misconception that the development strategy should be governed by these factors; because it doesn't factor some of these problems:

- Huge pull requests are hard to code review
- Hard to review code lets bugs escape
- Continuous re-basing and conflict resolution is hard

The major cause of the above problems is __long-lived branches__. To rephrase Frank Compagner, _“Long-lived branches create distance between developers and we do not want that”_.

<span class="newthought">Trunk based development offers</span> a pragmatic solution to these problems. While the core idea is fairly easy to grasp,
it's important to understand the details to truly appreciate its long-term benefits, and learn the techniques to employ in development workflows.

Let’s contrast trunk based development with the most commonly used strategy - **Gitflow**. <label for="sn-git-flow" class="margin-toggle sidenote-number"></label></span><input type="checkbox" id="sn-git-flow" class="margin-toggle"/><span class="sidenote">Gitflow is a Git workflow design that was first published and made popular by Vincent Driessen at nvie. Gitflow defines a strict branching model designed around the project release. This provides a robust framework for managing larger projects.</span> If the name doesn’t ring a bell, you’re probably using some of your own flavour of the Gitflow strategy.


![Gitflow](/assets/trunk-based-development-git-flow.svg)


One of the relatively better flavours of the gitflow strategy is to checkout a feature branch which acts as a base throughout the development process of the feature. But no commits are made directly to this feature branch. Rather, further short-lived branches are checked out and merged back to the feature branch via pull requests. Code reviews are relatively easy due to smaller changes. It works well when one feature is being developed at a time. When multiple feature branches are checked out, conflict resolution problems start popping up.

Trunk based development really shines as an alternative, mainly because it encourages you to tweak different parts of the workflow, and
it is more of a software discipline rather than a strategy per se.

In brief, trunk based development is where all developers commit to one shared branch (called _mainline_ or _trunk_) under source-control, resisting the urge to create long-lived branches.

_"But we’re working on a feature which might take a long time to be rolled out!”_ 

Consider these practices:

  - **Atomic commits**. Each commit be one logical unit of change and must not fail a build
  - **Feature flags**. Though the commits are checked into the trunk and deployed in production, they needn’t be turned on until the feature is complete. Feature flags are powerful and helps you roll-out features in a controlled way.

Since the commit has to be merged into the trunk as it comes in, the review must be done.
The review backlog should be never allowed to pile up. To get your team up to speed, it's essential to establish a culture such that reviewing code is
of equal priority to writing code.

Is this worth it? In short, Yes. Trunk based development, when followed consistently, enables the following:

- Continuous integration. Incremental commits ensures the code changes are always integrated well.
- Flexible feature roll-out. You could roll-out features in a controlled way say to a specific set of users.
- Active code base which encourages better collaboration and clarity amongst the team.

---

Post kickstart of trunk based development in your development workflow, we suggest reading up on [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com/) which has a lots of further pointers and explanations.
