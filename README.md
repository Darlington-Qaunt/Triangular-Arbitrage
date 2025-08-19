# Triangular-Arbitrage
 Development and Backtesting

1\. Research  Overview

As part of the development and testing phase of this research, the main goal was to build a working triangular arbitrage bot using real-time prices and historical data from Binance. This involved creating a script that could continuously monitor three trading pairs — SOL/USDT, SOL/ETH, and ETH/USDT — and look for opportunities where a profit could be made by cycling through these trades.  The bot was designed to simulate trading cycles, detect arbitrage opportunities, measure profitability, and log all results for post-analysis. This process included both live backtesting using scraped prices, which refers to the practice of automatically extracting price data from websites, often using automated scripts or bots, to monitor and analyze pricing information and data logging for offline visualization and reporting.

2\. **Setup and Tools**

The development environment was established using PyCharm IDE and Python 3.11+. Core packages used include:

* requests – To call Binance's public API.  
* csv – To save results in a CSV file.  
* datetime, time – To timestamp each transaction event.  
* pandas, matplotlib – For data analysis and visualization.

```python```
import requests
import time
import csv
from datetime import datetime
import pandas as pd
import matplotlib.pyplot as plt


3\. **Core Logic Design**  
The core bot functionality is based on simulating a 3-step arbitrage cycle using an initial 1000  USDT capital. The flow of a single transaction cycle is as follows:

* USDT → SOL: The bot checks how much SOL can be purchased with the initial capital.  
* SOL → ETH: Converts the SOL received into ETH using the SOL/ETH pair.  
* ETH → USDT: Finally, the ETH is sold back to USDT, completing the triangular loop.

At the end of the cycle, the bot compares the final USDT amount to the initial amount to determine:

* If a profit was made  
* The percentage gain  
* Whether an arbitrage opportunity existed at that timestamp

The logic is wrapped inside a function that runs in a loop every few seconds (default: 5 seconds). The results, including timestamp, profit, and other metrics, are appended to a CSV file for record-keeping. and later visualization.  
4\. **Testing with Live Price Data**  
The testing phase used  live data via Binance's public API to simulate realistic and dynamic market conditions. This was crucial in identifying the frequency and nature of arbitrage opportunities in real time.  
The bot was run for multiple sessions each with 5 seconds time laps  with varying durations and intervals. For each iteration:

* Live prices were fetched.  
* Simulated trades were computed.  
* Results were logged into arbitrage\_resultsbot.csv.

This approach mimicked the latency and unpredictability of actual markets and helped in evaluating the stability of the strategy in fluctuating conditions. 

5\. **Trading Fees and Practical Considerations**

While the bot  doesn’t execute real trades, it includes provisions to simulate fees (e.g., 0.1% per trade). Although Binance offers discounted fees for certain token holdings or VIP users, the default assumption in our simulation was 0.1% fee per leg, applied implicitly in the backtest analysis phase.  
This allows for more conservative estimations of profitability and helps avoid overfitting the results to ideal condition

6\. **Visualization and Results Interpretation**  
A second script, visualize\_arbitrage.py, was developed to analyze and plot the results of each session. Using the stored CSV file, we generated the following key plots:

* Profit Over Time \- Visual trend of profit/loss across time intervals.  
* Profit Histogram \- Distribution of profitable vs. unprofitable opportunities.  
* Cmulative Profit \- Total performance over the session.  
* Initial vs Final Capital \- Comparison of starting vs ending USDT.  
*  Opportunity Frequency \- Number of profitable trades vs. missed opportunities.


These visuals provided tangible proof of the strategy's behavior over time, as well as valuable insight into when and why opportunities arise.

**7\. Key Insights from Development and Testing**  

The testing phase revealed several important insights about triangular arbitrage in cryptocurrency markets.  
7.1. Arbitrage opportunities do exist, but they are often small and short-lived.  
During the experiments, the bot detected moments where cycling through SOL → ETH → USDT (or the reverse) would yield a profit. However, these profits were usually very small, often fractions of a percent. This aligns with the nature of modern digital markets: because exchanges are highly competitive and connected, mispricings are corrected quickly, often within seconds. Thus, while opportunities are present, they require near-instant execution to be exploited effectively

**7.2 Speed is absolutely critical**.  

One of the clearest lessons was that execution speed can determine success or failure. Even a delay of two or three seconds between detecting and attempting to trade can completely erase the profit. By the time an arbitrage opportunity is identified, other traders—or bots with faster systems—may already have taken advantage of it, pushing the prices back into alignment. This highlights the importance of minimizing latency through better infrastructure 
