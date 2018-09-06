---
title: "Trunk Based Development"
date: "2018-09-06"
type: "article"
---

When it comes to development and release workflows, there are a lot of strategies to follow. In the beginning, I had a misconception that the strategy should be designed depending on the team’s requirements and project’s type (either a client library or a self hosted executable). When I was with company X, we tried various strategies but the following came into the picture repeatedly.

- Huge PRs are hard to code review
- Hard to review let bugs escape
- Continuous rebasing and conflict resolution is hard

The major contributor to the above was long-lived branches. To rephrase Frank Compagner, “Long-lived branches create distance between developers and we do not want that”.

TL;DR version of the solution would be ‘Follow trunk based development'. But I urge you to read further because 1) You need to understand the long-term benefits 2) There are some techniques you need to employ in your workflow to adopt trunk based development.

Let’s contrast trunk based development with the most commonly used strategy - Gitflow. <label for="sn-git-glow" class="margin-toggle sidenote-number"></label></span><input type="checkbox" id="sn-git-flow" class="margin-toggle"/><span class="sidenote">Gitflow is a Git workflow design that was first published and made popular by Vincent Driessen at nvie. Gitflow defines a strict branching model designed around the project release. This provides a robust framework for managing larger projects.</span> If the name doesn’t ring a bell, you’re probably using some of your own flavour of the Gitflow strategy. Take a look at the image below.


![Gitflow](/assets/trunk-based-development-git-flow.svg)


One of the relatively better flavour of gitflow strategy we observed was to checkout a feature branch which acts as a base throughout the development process of the feature. But no commits are made directly to this feature branch. Rather, further short-lived branches were checked out and merged back to the feature branch via pull requests. Code reviews were relatively easy due to smaller changes. Worked well when one feature is being developed at a time. When multiple feature branches were checked out, conflict resolution problems started popping up.

We started looking out for better strategies to enable us move forward smoothly. Trunk based development was an interesting one. Mainly because 1) It encourages you to tweak different parts of the workflow 2) Its more of a software discipline rather than a strategy per se.

In brief, Trunk-Based Development is where all developers commit to one shared branch (called mainline or trunk) under source-control resisting the urge to create long-lived branches. Think about it for a while and you end up with a lot of questions and caveats for this approach. That’s alright. As I said, you need to make some changes to your development practices.

One common question is "We’re working on a feature which might take a long time to be rolled out.” You can fix this by following best practices which comes along with its own set of benefits.

  - Atomic commits. Each commit be one logical unit of change and must not fail a build
  - Feature flags. Though the commits are checked into the trunk and deployed in production, they needn’t be turned on until the feature is complete. Feature flags are powerful and helps you roll-out features in a controlled way.

Since the commit has to be merged into the trunk as it comes in, the review should be done. The review backlog should be never allowed to pile up. To get your team upto speed, establish a culture such that reviewing code is of equal priority to writing code.

Is this worth it? In short, Yes. Trunk based development when followed enables the following


- Continuous integration. Incremental commits ensures the code changes are always integrated well.
- Flexible feature rollout. You could rollout features in a controlled way say to a specific set of users.
- Active codebase which encourages better collaboration and clarity amongst the team.

Post kickstart of trunk based development in your development workflow, read up on https://trunkbaseddevelopment.com/ which has a lots of further pointers and explanations.

Till next time.
