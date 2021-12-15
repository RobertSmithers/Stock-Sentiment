# StockSentimentPredictor
By: Robert Smithers

A stock market predictor using techniques of pair trading and sentiment analysis

## Overview
This entire project has been documented throughout with my continued progress and comments. With that said, here is the brief overview:
1. **pair_stats.ipynb**: to start, I spent some time running many statistics on combinations of pairs in the NASDAQ. I will not go into depth for the overview, but many individual obstacles, thinking, and workarounds are presented in the file. The purpose of this file was to discover pairs of stocks that are highly correlated (at different offsets), with the idea that a proper ML model could use the correlations to better estimate a stock's price.
2. **sentiment_analysis.ipynb**: now that we have the pair(s) of stocks that we want to make predictions from/on, we need a mechanism to incorporate the sentiment_analysis portion of the vision. To start, Twitter would be super valuable for this... they only allow up to 7-days history with their API :( . Reddit is a bit more transparent, though, so luckily I could (and did) accomplish something very similar using Reddit and their many stock/wallstreetbets subreddits instead. **Note: Due to APIs linking to account, you will need your own secret keys and ids in your own reddit_secrets.json, and/or potentially twitter_auth.py if you want the 7-day history**
3. **model.ipynb**: Now to the funn(er) part! This is straightforward, so I will keep this brief. LSTM model is trained on [the last 7 days of a highly correlated stock, sentiment analysis of reddit posts for the past 7 days] and aims to predict the direction of movement of a specific stock. There are some nice visualizations at the end that does a quick little simulation on returns, so there's a lot of potential to play around with other stocks and pairs.

## Results
The specific numbers themselves are not too important, but for the sake of reporting, here are a couple metrics:
1. LSTM Model: 3.84% return, Market: 1.03% return
2. LSTM Model made correct buy/short decision 60% of the time
3. Train loss: 0.6, Test loss: 0.48

![3.84% Market Return](COHR-LSTM-rate-price.jpeg)

The second result seems pretty cool, but they're not as cool as I would've hoped. To keep matters simple, this can best be summarized by the fact that the data on our correlated stock is quite limited (~6 months), so training many epochs on this doesn't help too much. Unfortunately, stocks that have not been on the market for a long term were given an unfair advantage in correlation since those stocks would not have to correlate for as long with other stocks to achieve a higher correlation value.

Finally, the last thing worth mentioning is the possibility of some data snooping in our train/test cycles. If we think about the statistics for measuring correlation, I calculated the pearson correlation coefficient for every 7-day period between the predicted stock, stock A, and the correlated stock, stock B, with a 7 day offset (t-7 to t-1 of stock B compared to t, t+7 of A). Choosing the pair with the highest offsets may have given the model greater insight into what future prices may be by virtue of the fact that it already knows it is highly correlated. This makes the model's job a lot easier, but in the future, we do not know for sure that the correlation will remain that high.


## Conclusion
In all, I learned a lot, and it is much more difficult to cover all of the aspects of this project over text, but it was very insightful and serves as a fantastic placeholder for future research. I am excited to go even more in-depth with this in the future. One of the biggest takeaways for me is to really improve this model, perhaps it is best to take the more mainstream route of sentiment analysis and other factors opposed to pair trading. The pair trading strategy did not seem to hold much potential, though perhaps I did not implement it correctly. Regardless, the model served1 its purpose well and can be easily modified to train on various stocks. It would be worthwhile to see if the results hold up on stocks with a longer history, higher volume, and greater volatility!