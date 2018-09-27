# Contributing

At Deep Thought, we publish deeply researched but concise articles on topics such
as design patterns, development workflows, software architecture, maintainability and technical debt.
Each article is aimed at explaining _one idea_ related to these topics from its first principles. We believe
this is the best possible format for our readers to assimilate these ideas and put them into practice -- eventually
increasing the quality of software that they build.

The guiding principle behind Deep Thought is quality over quantity, and understanding time-tested ideas fundamentally.

Deep Thought is driven by its community. We're open to publish articles from contributors as long as they follow the principles explained above.
To maintain the quality, all articles go through a review process via GitHub [pull requests](https://github.com/deepthoughtblog/blog/pulls).
There are designated editors for these articles who have a final say on acceptance -- and the process is totally transparent via these pull request reviews.

## Creating a new article

Each article is a Markdown file present in the path `content/article`. We use [Hugo](https://gohugo.io/) as our static site framework. The file name of
the article is taken as the article's slug in the URL. For example, `content/article/new-post.md` would add a new article with slug `new-post`.

### Article format

```markdown
---
title: "Name of the article"
date: "YYYY-MM-DD"
type: "article"
---

< The article content goes here... >
```

You can take a look at [this article](content/article/what-indexes-to-create-on-table.md) for reference.


## How to publish

- If you already have a rough draft of the article, please create a pull request. An editor of Deep Thought would add necessary labels and take it forward for review.
- Every article needs at-least one approval from editors to be published.
- If you would like to write about a topic but need more insights from the community, you can create an issue with the article's topic and ask for feedback.

## Publishing schedule

We publish articles every alternate Monday of the month. The schedule is maintained in [GitHub milestones](https://github.com/deepthoughtblog/blog/milestones).
When an article is approved, it will be published under the upcoming milestone.

## Addendum

- DeepThought is a community initiative and are run by individuals on their spare time. Please be patient.
- Please be open to criticism/insights on the articles and co-operate with the participants of the article review when changes are requested.

We are looking forward to your contribution!
