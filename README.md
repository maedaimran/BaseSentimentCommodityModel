# BaseSentimentCommodityModel
This project analyzes sentiment trends in Reddit comments on oil-related news using PRAW and HuggingFace Transformers, and examines correlations with historical stock data from Yahoo Finance.
##Libraries and Tools
PRAW: For accessing and scraping data from Reddit.
HuggingFace Transformers: For sentiment analysis using the distilroberta-finetuned-financial-news-sentiment-analysis model.
yfinance: For fetching historical stock data.

##Steps
Install Necessary Libraries:

python
!pip install praw transformers yfinance
Initialize Sentiment Analysis Model:

python
from transformers import pipeline
sentiment_pipeline = pipeline("sentiment-analysis", model="mrm8488/distilroberta-finetuned-financial-news-sentiment-analysis")
Configure Reddit API with PRAW:

python
Copy code
import praw

reddit = praw.Reddit(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET",
    user_agent="YOUR_USER_AGENT",
)
Fetch Relevant Reddit Posts:

python
subreddit = reddit.subreddit("worldnews")
submissions = []
for submission in reddit.subreddit("worldnews").search("oil", sort="relevance", time_filter="month", limit=5):
    submissions.append(submission)
Extract and Analyze Comments:

python
submission = submissions[0]
submission.comments.replace_more(limit=0)
comments = submission.comments.list()
comment = comments[0].body
sentiment = sentiment_pipeline(comment)
Aggregate Sentiment Analysis Results:

python
positive_count = 0
negative_count = 0

for comment in comments:
    result = sentiment_pipeline(comment.body)
    if result[0]['label'] == 'POSITIVE':
        positive_count += 1
    elif result[0]['label'] == 'NEGATIVE':
        negative_count += 1

print("Positive count:", positive_count)
print("Negative count:", negative_count)
Fetch Historical Stock Data:

python
import yfinance as yf

ticker_symbol = "USO"
start_date = "2024-03-11"
end_date = "2024-03-12"

data = yf.download(ticker_symbol, start=start_date, end=end_date)
print(data)
##Results
Sentiment Analysis: Analyzing the comments from Reddit posts related to oil news showed neutral sentiments predominantly, with very few positive or negative sentiments detected.
Stock Data: The historical stock data for the United States Oil Fund (USO) was fetched for the specified date range to examine potential correlations with the sentiment analysis results.
Conclusion
This project successfully integrates various tools and libraries to perform sentiment analysis on social media data and correlate it with financial data. The results indicate a need for further analysis with a broader dataset to derive meaningful insights.

Future Work
Expand Data Collection: Include more Reddit posts and comments to build a comprehensive dataset.
Enhance Sentiment Analysis: Explore and compare different sentiment analysis models for better accuracy.
Correlation Analysis: Perform detailed statistical analysis to find correlations between sentiment trends and stock prices.
