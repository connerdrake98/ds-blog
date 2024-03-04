---
layout: post
title:  "Discover Profitable Stock Strategies in Pyton"
date: 2024-02-24
description: This post will show you exactly how to use the yfinance library in Python to test any stock strategy you could imagine. Just follow the simple outlined steps, and you will be testing your strategies in no time. Who knows, maybe you could even find one that outperforms the market!
image: "/assets/img/2024-02-24-title-image.png"
display_image: false
---
<p class="intro"><span class="dropcap">H</span>ave you ever tried to test stock strategies manually by looking at charts and indicators, writing down numbers, and then having to redo the entire process to test different inputs? Well, there is an easier way that will allow you to rapidly test tons of strategies in seconds.</p>

### Introduction

There are TONS of tools and APIs that claim to have free stock data or allow you to test stock strategies easily. The truth? Most of these APIs are either paid or out of support. 

This is why I scoured the internet to find a solution that was both free and easy to use for testing stock strategies. <a href='https://pypi.org/project/yfinance/#smarter-scraping'>Here it is.</a>

### 1 - Install the yfinance package

In order to start getting stock data to work with, you will need to use the yfinance python package.

First, install the yfinance package using <a href='https://pypi.org/project/pip/'>pip</a> (or your preferred python package installer). Make sure to install it into the virtual environment that is assigned to the project you will be writing code in.

{%- highlight python -%}
$ pip install --upgrade "yfinance[repair]"
{%- endhighlight -%}

<a href='https://pip.pypa.io/en/stable/development/architecture/upgrade-options/'>--upgrade</a> upgrades any already installed packages to their latest versions, and [repair] installs the price repair dependency, which fixes some errors with fetching data from Yahoo's API such as missing data, missing volume, accidentally 100xing the price for some entries, etc.

It is also recommended to install pandas, since the objects returned by yfinance are <a href='https://pandas.pydata.org/pandas-docs/version/2.1/reference/api/pandas.DataFrame.html'>pandas DataFrames.</a>

{%- highlight python -%}
$ pip install pandas
{%- endhighlight -%}

### 2 - Import the Necessary Packages in Your Code

After installing the required pacakages, open your favorite <a href='https://hackr.io/blog/best-python-ide'>Python IDE</a>, select the environment that has yfinance installed, and import them using:

{%- highlight python -%}
import yfinance as yf
import pandas as pd # requests return pandas DataFrames, so it is useful to import pandas as well
{%- endhighlight -%}

### 3 - Make Your First yfinance Request For Historical Data

Create a yfinance ticker object, which you can use to access ticker data.

{%- highlight python -%}
aapl = yf.Ticker('AAPL')
{%- endhighlight -%}

Once you have created the object, you can call .history(period="1mo") to get the last one month of daily ticker data.

<h3>Code For Getting Historical Data</h3>

{%- highlight python -%}
aapl_history = aapl.history(period="1mo")
print(aapl_history['Close'])
{%- endhighlight -%}

This code outputs some tabular price data. We are selecting the 'Close' column since this is the price measured at the end of each trading day (each day when the markets are open).

<figure>
	<img src="https://connerdrake98.github.io/ds-blog/assets/img/aapl-tabular-data-2024-03.png" alt=""> 
	<figcaption>AAPl tabular stock price data output</figcaption>
</figure>

We can verify the accuracy of our call for data by comparing the most recent entry in the table returned by the program and looking at the most recent price of AAPL stock with a Google search.

<figure>
	<img src="https://connerdrake98.github.io/ds-blog/assets/img/aapl-stock-search-2024-03-03.png" alt=""> 
	<figcaption>A Google search for 'AAPL Stock' showing a matching most recent price of 179.66.</figcaption>
</figure>

A word of warning - if you make a lot of requests in quick succession in your code, Yahoo Finance may start to limit the amount of requests you make or even block you from making additional requests.

A simple and easy way to space out your requests is to use time.sleep(n), with n being the number of seconds to sleep (I recommend 2 seconds between requests). You will not need to install the time library prior to import because it comes installed with Python already. See the example below for how you could pause the program in between requests.

<h3>Getting Data From Multiple Stocks</h3>

{%- highlight python -%}
import time

tickers = ['AAPL', 'MSFT', 'IBM']

hist_data_frames = []

for i in range(0, len(tickers)):
  ticker_obj = yf.Ticker(tickers[i])
  ticker_hist = ticker_obj.history(period="1mo")
  print(tickers[i])
  print(ticker_his)
  hist_data_frames.append(ticker_his)

  time.sleep(2)

{%- endhighlight -%}

This code may appear to get the "monthly" historical data for AAPL stock, but it actually gets the last 1 month of historical daily data. The yfinance API only works with daily stock metrics as to not overwhelm Yahoo Finance with requests. 

When we print what aapl.history(period="1mo") returns, we get:

{%- highlight python -%}
-                                Open        High  ...  Dividends  Stock Splits
Date                                               ...                         
2024-02-02 00:00:00-05:00  179.630787  187.091269  ...       0.00           0.0
2024-02-05 00:00:00-05:00  187.910213  189.008818  ...       0.00           0.0
2024-02-06 00:00:00-05:00  186.621869  189.068743  ...       0.00           0.0
2024-02-07 00:00:00-05:00  190.397053  190.806534  ...       0.00           0.0
2024-02-08 00:00:00-05:00  189.148646  189.298448  ...       0.00           0.0
2024-02-09 00:00:00-05:00  188.649994  189.990005  ...       0.24           0.0
2024-02-12 00:00:00-05:00  188.419998  188.669998  ...       0.00           0.0
2024-02-13 00:00:00-05:00  185.770004  186.210007  ...       0.00           0.0
2024-02-14 00:00:00-05:00  185.320007  185.529999  ...       0.00           0.0
2024-02-15 00:00:00-05:00  183.550003  184.490005  ...       0.00           0.0
2024-02-16 00:00:00-05:00  183.419998  184.850006  ...       0.00           0.0
2024-02-20 00:00:00-05:00  181.789993  182.429993  ...       0.00           0.0
2024-02-21 00:00:00-05:00  181.940002  182.889999  ...       0.00           0.0
2024-02-22 00:00:00-05:00  183.479996  184.960007  ...       0.00           0.0
2024-02-23 00:00:00-05:00  185.009995  185.039993  ...       0.00           0.0
2024-02-26 00:00:00-05:00  182.240005  182.759995  ...       0.00           0.0
2024-02-27 00:00:00-05:00  181.100006  183.919998  ...       0.00           0.0
2024-02-28 00:00:00-05:00  182.509995  183.119995  ...       0.00           0.0
2024-02-29 00:00:00-05:00  181.270004  182.570007  ...       0.00           0.0
2024-03-01 00:00:00-05:00  179.550003  180.529999  ...       0.00           0.0

[20 rows x 7 columns]
{%- endhighlight -%}

As you can see, the object created by calling [ticker-object].history(period="[n months]mo") is a pandas DataFrame.

### 4 - Features of the Price Data DataFrame

Calling [ticker-object].history() returns a pandas DataFrame with many Columns containing data you can use to build and test a stock strategy. Here is a guide to each one:

<h6>Date:</h6> 
The Date feature indicates the date in which the stock price data corresponds to. It is in the format YYYY-MM-DD HH:MM:SS[+/- HH:MM UTC] with the last set of two-digit numbers represent the hours and minutes of offset from UTC time.

<h6>Open</h6>
The Open feature contains the price of the given stock at the beginning of the measured time interval. In this case, since yfinance provides access to daily stock data, the Open feature contains the price of the stock at the beginning of the day (at market open).

<h6>High</h6>
The Close feature contains the highest price the given stock reahced during the measured time interval. In this case, it is the highest price the stock reached during the date (day) of the corresponding entry.

<h6>Low</h6>
The Low feature contains the lowest price the given stock fell to during the measureed time interval. In this case, it is the lowest price the stock reached during the date (day) of the corresponding entry.

<h6>Close</h6>
The close feature contains the price of the given stock at the end of the measured time interval, or in this case, the price at the end of the corresponding day (at market close).

<h6>Volume</h6>
The <a href='https://www.investopedia.com/terms/v/volume.asp'>volume</a> feature contains the volume of shares of stock traded. This is the total amount of shares that exchanged ownership during that time period, or in this case, during the corresponding day indicated by the Date feature for the given stock.

<h6>Dividends</h6>
Indicates whether a <a href='https://www.investopedia.com/terms/d/dividend.asp'>dividend</a> was paid out (and how much was paid out per share). If the value is something other than 0, this indicates that a dividend equal to that value (per share) was paid out to owners of the stock. Stock prices often move before, during, and/or after divident payouts, so some stock trading strategies revolve around predicting these movements.

<h6>Stock Splits</h6>
Indicates whether a <a href='https://www.investopedia.com/terms/s/stocksplit.asp'>stock split</a> occurred, with 0 indicating no stock split and a number other than 0 indicating that a stock split occurred that multiplied the number of shares in circulation by the given number. 

<h6>Stock Split Example</h6>
For example, if the "Stock Splits" feature had a value of 3, this would indicate that the number of shares were multiplied by 3 (and would be three times cheaper, with each owner of stock now owning 3x as many shares). The yfinance app automatically backwards-adjusts prices to compensate for stock splits. So in this case, the price would not suddenly change to three times cheaper on the stock split date, but the price values before the stock-split date would be adjusted to one-third of their value to ensure smooth price data.
 
### 5 - The Portfolio Management Code (Making the Trades)

The goal of this function is to iterate through the days in the entries of a stock data DataFrame and make buying or selling decisions based on any provided criteria.

The programmer should be able to set:
<ol>
    <li>Number of months of data to test</li>
    <li>Choice of stock to test</li>
    <li>Initial portfolio size</li>
    <li>Signal length (the number of price points needed to calculate relevant indicators and make a trading decision)</li>
    <li>The buying/selling criteria (which will be a function that takes in the last n entries and makes a decision whether to buy or not)</li>
    <li>The stop loss and take profit percentages</li>
</ol>

To do this, we create a simple program that applies these settings to execute a strategy and report the trades taken and the resulting portfolio balance after a set amount of time.

In this simple example, we will use one ticker, so we will not need to make multiple requests when running our python file, but if you would like to adapt the code to test multiple tickers, remember to use the time library to run time.sleep([num. seconds to sleep]) between requests to avoid timeouts and restrictions.

<h3>Full Code</h3>

{%- highlight python -%}
import yfinance as yf
import pandas as pd # requests return pandas DataFrames, so it is useful to import pandas as well

'''---------------------------------------------------
--   CUSTOMIZE THIS PART (YOUR DESIRED STRATEGY)     --
----------------------------------------------------'''

history_length_in_months = 12
ticker_to_test = "MSFT"
portfolio_equity = 10000
stop_loss_percentage = 5
take_profit_percentage = 10
signal_length = 21 # test for crossover on 20sma
def buy_on_sma_20_crossover(df_slice):
    sma_20 = df_slice['Close'][:-1].mean()  # Use the first 20 prices
    second_most_recent_close = df_slice['Close'].iloc[-2] # is < sma_20 in crossover
    most_recent_close = df_slice['Close'].iloc[-1] # is > sma_20 in crossover

    # return whether there is a crossover
    return second_most_recent_close < sma_20 < most_recent_close

'''---------------------------------------------------
--    LEAVE THIS PART (IT TESTS YOUR STRATEGY)       --
----------------------------------------------------'''

ticker_obj = yf.Ticker(ticker_to_test)
ticker_history = ticker_obj.history(period=f"{history_length_in_months}mo")
ticker_history.reset_index(inplace=True)

in_trade = False
pnl = 0
entry_price = 0
trade_results = []

saved_starting_equity = portfolio_equity

for index, row in ticker_history.iterrows():
    if index >= signal_length - 1:
        if in_trade:
            current_price = row['Close']
            price_change_percentage = ((current_price - entry_price) / entry_price) * 100
            pnl = price_change_percentage

            if pnl <= -stop_loss_percentage or pnl >= take_profit_percentage:
                portfolio_equity = portfolio_equity * ((100 + pnl) / 100)
                print(f"   Exit signal at index {index} on date {row['Date']}. Close price: {row['Close']:.2f}, PnL: {pnl:.2f}%. Equity: {portfolio_equity:.2f}")
                # Assuming entire portfolio is used in trade at 1x leverage
                in_trade = False
                trade_results.append('Profit' if pnl > 0 else 'Loss')
        else:
            # Get a slice of the dataframe whose size matches the indicator length
            df_slice = ticker_history.iloc[index - signal_length + 1: index + 1]
            # Check for buy signal
            if buy_on_sma_20_crossover(df_slice):
                in_trade = True  # Enter trade
                entry_price = row['Close']  # Mark the entry price for the trade
                print(f"Buy signal at index {index} on date {row['Date']}. Close price: {row['Close']:.2f}. Equity: {portfolio_equity:.2f}")

print(f"\n* Starting Equity: ${saved_starting_equity}")
print(f"* Final Equity: ${portfolio_equity:.2f}")
print(f"* Total Return (Percentage): {((portfolio_equity / saved_starting_equity) * 100 - 100): .2f}%")
print(f"* Trades Taken: {len(trade_results)}")

{%- endhighlight -%}

This program iterates through the price entries of the chosen stock and initiates a trade if the criteria you set in the previous section of the program is met. It then keeps track of the change in price until the price hits your stop loss or take profit, then closes the trade and updates your portfolio balance. It continues to do this for all of the price data provided whose length is dependent on your provided number of months of data to iterate through as selected in the previous step.

Here are the trades taken and the results for 12 months of Microsoft's (MSFT) stock data:

<figure>
	<img src="https://connerdrake98.github.io/ds-blog/assets/img/msft-sma-20-trading-results.png" alt=""> 
	<figcaption>The SMA-20 strategy for trading MSFT yields a profit of 27.74% over the last 12 months over 4 trades.</figcaption>
</figure>

As you can see from this daily chart of MSFT's stock price with an added SMA-20, the trades were correctly executed when the price of MSFT stock crosses above the 20-Simple-Moving-Average.

<figure>
	<img src="https://connerdrake98.github.io/ds-blog/assets/img/msft-sma-20-strategy-chart.png" alt=""> 
	<figcaption>The trades in the SMA-20 strategy for trading MSFT as shown on a TradingView chart</figcaption>
</figure>

There is also some example code available in the <a href='https://github.com/ranaroussi/yfinance'>readme of the yfinance github repo</a>, but note that the featured code for caching and request limiting uses out-of-date and out-of-support packages and will not work if you just copy and paste it.

### 5 - Conclusion

The yfinance Python library is by far the best free resource for testing stock strategies. If you use the code featured in this blog post, you can get up and running in testing your own strategies in as little as five minutes.

What will you do with yfinance? Find the next market-beating trading strategy? Beat the returns in your managed 401k? Who knows what profitable strategies you could find. I would love to see what kinds of results you can get!