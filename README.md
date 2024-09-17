# Introduction
This Project analysis the relation between sentiment of developers with different factors like part of day , issue types , types of developers and severity of issues.
## Dataset Origin

The dataset is derived from the 20-MAD (20-Year Massive Software Artifact Data) dataset, which integrates commit and issue data from Mozilla and Apache projects spanning over 20 years. Our dataset builds upon this foundation by adding sentiment analysis annotations to commit messages.

## Overview

The dataset includes:
- Commit messages from various open-source projects.
- Sentiment analysis of each commit message using a fine-tuned seBERT model.
- Metadata such as commit times, contributor categories, and issue types (bug/non-bug).

## Dataset Details

- **Projects Covered**: 114 Apache projects
- **Total Commits**: 641,628
- **Total Issues** : 297648
- **Sentiment Analysis**: Each commit message is annotated with the sentiment (positive, negative and neutral).
- **Metadata Included**:
  - Commit messages
  - Commit times
  - Contributor categories
  - Issue types (bug/non-bug)
  - "Part of the day" based on commit time



- **Generated Tables for Research Questions**:


| commits             |            |
|---------------------|-----------|
| product             | *String*  |
| issue_id            | *String*  |
| working_times       | *String*  |
| committer           | *String*  |
| hash                | *String*  |
| commit_time         | *String*  |
| message_sentiment   | *String*  |
| issuetype           | *String*  |
| priority            | *String*  |
| category            | *String*  |

&nbsp;

| issue comments      | *String*  |
|---------------------|-----------|
| text_sentiment      | *String*  |
| product             | *String*  |
| issue_id            | *String*  |
| comment_id          | *String*  |
| priority            | *String*  |
| issuetype           | *String*  |


## Sentiment Analysis Using seBERT


## Result Analysis


## Usage

The dataset is provided in parquet. Due to large file size we split the dataset into five different parts. You can combine those and use it for various research purposes, including but not limited to:
- Sentiment analysis in software development
- Analyzing the relationship between commit sentiment and project outcomes
- Studying contributor behavior and patterns in open-source projects

