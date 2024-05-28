
## Query of RQ3

<button onclick="copyToClipboard('#install-command')">Copy</button>

<pre id="RQ3">

--Step 1: Calculate Commit Frequency

  CREATE VIEW commit_gaps AS
SELECT 
    committer,
    commit_time,
    LAG(commit_time) OVER (PARTITION BY committer ORDER BY commit_time) AS prev_commit_time,
    EXTRACT(EPOCH FROM (commit_time::timestamp - LAG(commit_time::timestamp) OVER (PARTITION BY committer ORDER BY commit_time::timestamp))) / 86400 AS days_between_commits
FROM 
    commits;

	

--Step 2: Categorize Commits Based on Frequency and Time
CREATE VIEW categorized_commits AS
SELECT
    c.*,
    CASE
        WHEN cg.days_between_commits >= 7 THEN 'irregular'
        ELSE 'regular'
    END AS commit_frequency,
    CASE
        WHEN c.part_of_day_commit = 'night' THEN 'odd'
        ELSE 'regular'
    END AS commit_time_category
FROM 
    commits c
LEFT JOIN 
    commit_gaps cg
ON 
    c.committer = cg.committer AND c.commit_time = cg.commit_time;


--Step 3: Convert Sentiment Text to Numerical Values

CREATE VIEW sentiment_numerical AS
SELECT
    committer,
    commit_frequency,
    commit_time_category,
    CASE
        WHEN message_sentiment = 'positive' THEN 1
        WHEN message_sentiment = 'neutral' THEN 0
        WHEN message_sentiment = 'negative' THEN -1
        ELSE NULL
    END AS numerical_sentiment
FROM 
    categorized_commits;


	

SELECT
    committer,
    commit_frequency,
    commit_time_category,
    AVG(numerical_sentiment) AS avg_sentiment
FROM 
    sentiment_numerical
GROUP BY 
    committer, 
    commit_frequency, 
    commit_time_category
ORDER BY 
    committer;


--Step 4: Aggregate Sentiment Scores
SELECT
    committer,
    commit_frequency,
    commit_time_category,
    AVG(numerical_sentiment) AS avg_sentiment
FROM 
    sentiment_numerical
GROUP BY 
    committer, 
    commit_frequency, 
    commit_time_category
ORDER BY 
    committer;

</pre> 


## MWW test 

<button onclick="copyToClipboard('#install-command')">Copy</button>

<pre id="RQ3">

import pandas as pd
from scipy.stats import mannwhitneyu

# Load the data from the CSV file
data = pd.read_csv('path_to_your_csv_file.csv')

# Filter the data based on commit_frequency
regular_sentiment = data.loc[data['commit_frequency'] == 'regular', 'avg_sentiment']
irregular_sentiment = data.loc[data['commit_frequency'] == 'irregular', 'avg_sentiment']

# Perform the Mann-Whitney U test
stat, p_value = mannwhitneyu(regular_sentiment, irregular_sentiment)

# Output the results
print(f'Mann-Whitney U Test statistic: {stat}')
print(f'p-value: {p_value}')

# Interpret the result
alpha = 0.05
if p_value < alpha:
    print('Reject the null hypothesis: There is a significant difference between regular and irregular commit frequencies.')
else:
    print('Fail to reject the null hypothesis: There is no significant difference between regular and irregular commit frequencies.')
</pre> 
