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

- **Top 10 Products Based on highest commits**:


| Products | Commits | Committers | Issues | Bugs  | Non-Bugs |
|----------|---------|------------|--------|-------|----------|
| HBASE    | 41661   | 192        | 14687  | 22184 | 19477    |
| AMBARI   | 35575   | 205        | 21145  | 26858 | 8717     |
| SLING    | 31377   | 127        | 6747   | 9146  | 22231    |
| CAMEL    | 26848   | 206        | 10458  | 8956  | 17892    |
| SPARK    | 26119   | 122        | 15958  | 11963 | 14156    |
| SOLR     | 22598   | 131        | 6728   | 8813  | 13785    |
| HIVE     | 18093   | 160        | 12709  | 10767 | 7326     |
| HDFS     | 17493   | 175        | 8104   | 7646  | 9847     |
| LUCENE   | 17468   | 110        | 5615   | 6573  | 10895    |
| HADOOP   | 15805   | 224        | 8238   | 8485  | 7320     |


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

- **RQ1: Does Commitfrequency affect the developer sentiment on the commit message?**
    
  
   - *Sentiment distribution across different committer frequencies* 
      | Sentiment     | High  |   Low | Medium
      |--------|-----------|------|----|
      | Negative    |  32721 |   884 | 5432 | 
      | Positive    |  12184 |   196 | 1820 | 
      | Neutral   |  495019 |   12261 | 81111 | 

    

  - *Chi Squared Test Result*  
     - Chi-squared Statistic: 55.067899563179445
     - p-value : 3.144 × 10<sup>-11</sup>


  - *Expected frequencies for each sentiment category across different committer frequencies.*
     | Sentiment     | High  |   Low | Medium
     |--------|-----------|------|----|
     | Negative    |  32849.272769 |    811.673769 | 5376.053462 | 
     | Positive    |  11949.168054 |   295.252389|  1955.579557 | 
     | Neutral   |  495125.559178 |   12234.073842 | 81031.366981 | 


  - *Standardized residuals for Committer Category* 
     | Category    | Negative  |   Positive | Neutral
     |--------|-----------|------|----|
     | High   |  -0.707736| 2.148267|-0.151437 
     | Medium    | 0.763030|-3.065889| 0.279748 | 
     | Low   |   2.538663 |-5.776227| 0.243438 | 

  - *Mann-Whitney U test*
      | Category    | U-statistics  |   P-value | 
     |--------|-----------|------|
     | High   |  3.053 × 10<sup>10</sup> | P<<0.05| 
     | Medium    | 7.580 × 10<sup>8</sup>|1.261 × 10<sup>-163</sup>| 
     | Low   |   1.430 × 10<sup>7</sup> |4.9 × 10<sup>-31</sup>| | 

  - *Mann-Whitney U test between positive and negative sentiment scores for each committer category*
      | Category    | High  |   Medium | Low
     |--------|-----------|------|----|
     | High   |  -| 0.199|-0.081
     | Medium    | 0.119|-| 0.286 | 
     | Low   |   0.081 |0.286| - | 


  ![Alt text](rq1.png "Sentiment Frequencies of Commit messages by Committer’s Category")

   <div style="border: 1px solid #000; padding: 10px; margin: 10px;">
  Answer: The analysis reveals a significant association between committer frequency and commit sentiment. Low-category committers have significantly higher negative sentiment than other categories.
</div>


## Usage

The dataset is provided in parquet. Due to large file size we split the dataset into five different parts. You can combine those and use it for various research purposes, including but not limited to:
- Sentiment analysis in software development
- Analyzing the relationship between commit sentiment and project outcomes
- Studying contributor behavior and patterns in open-source projects

