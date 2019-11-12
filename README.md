# S&P500 - Stock Return Analysis 
Webpage deployed [here](https://sp500priceprediction.herokuapp.com/#page1). Explore!

We looked the current S&P 500, looked at the past 5 years of stock returns in a number of ways. Built a  Deep Learning Model to classify stocks into outperformers, market performers and underperformers. Tested it with a Monte Carlo simulation.  While the time period is rather short, the model appears to provide excess returns.  

Python | Pandas | Numpy | SQLite | scikit-learn | TensorFlow

Team Members: Brad Daniels, Olufnke Olaleye, and Estella Yu.

![webpage](https://github.com/BradDaniels732/Final-Project/blob/master/static/imgs/stocks.gif)

We downloaded the current list of 500 stocks from Wikipedia, five years of quarterly income statements and balance sheets for each company from Edgar-Online, and monthly closing prices from AlphaVantage.  From this data, we were able to calculate the following

__Fundamental Factors__
1. Price / Trailing four quarter earnings per share
2. Price / Book Value
3. Enterprise Value / Revenue
4. Enterprise Value / Earnings before Interest and Taxes

__Quality Factor__
1. Net Debt / Total Capital

__Size Factor__
1. Market Capitalization

__Momentum Factors__
1. Trailing One-Month Price Return
2. Trailing Three-Month Price Return
3. Trailing Twelve-Month Price Return

__Project Summary__
The final project was done in two phases, with the first phase looking at the return history to the fundamental, quality and size factors, across the whole market and by economic sector.  On the website, that analysis is contained in the first row of tiles.  The second phase of the project focused on machine learning.  We tried an ARIMA model to try and forecast future stock prices, a deep learning model to predict outperformers, market performers and underperformers, and a cluster analysis to see if there were any unexpected groupings of stocks.

My job, in the first phase, was to extract all the data, transform and perform all stock-level calculations and load it into an SQLite database.  I also calculated the wealth indices for all factors across all sectors, resulting in the statistics that are seen in the visuals in the first row of tiles on the website.  In the second phase, I added the momentum factors, developed the deep learning model and ran it through the Monte Carlo simulation.  The results of that are in the PowerPoint slideshow in the tile on the second row, second tile.  Olufunke Olaleye was responsible for the cluster analysis that can also be seen towards the end of the aforementioned PowerPoint.  She also assisted with the creation of the website.  Estella Yu was responsible for the bulk of the website design and innovative graphics.  She also did the ARIMA analysis, which can be found in the tile on the second row, first tile.

__Technical Details__
1. For all the fundamental and quality factors, the financial data was lagged by two months, to make sure that the financial data was known at the time of the analysis.  If I had the date of each quarter's press release or 10Q/10K filing, I would have used those dates to lag the data. 

2. All work on P/E, P/B, EV/Revenue and EV/EBIT were actually done with the inverse of the numbers, thus E/P, B/P, Revenue/EV and EBIT/EV.  This was done to prevent the discontinuity in the calculations when earnings, book value or EBIT transition from positive to negative.

3. For the Deep Learning model, for each month, I split the data into train/test groups by ticker.  The model was trained on the prior 24 months data, then tested on the next month's total return.  This was done to eliminate any look-ahead bias.  Thus, for a given month, say June 2019, the model was trained on data from July 31, 2017 through June 30, 2019 on the "train" set of tickers, then tested on the total return of the classified "test" set of ticker from June 30th through July 31st, 2019.

4. I would have much preferred to use data for the analysis, going back 20 years or so, and to be able to include companies that no longer exist, whether they were taken over by other public companies, taken private or gone bankrupt.  But we were limited to using free data that we could access.  

5. Financial Stocks do not have "EBIT", or "Earnings before Interest and Taxes".  For all other companies, interest payments are a "cost of capital" as opposed to a part of their normal business operations.  Thus, EBIT generally represents the amount of money a company has after running their normal operations, but before paying interest expense and taxes.  Financials, on the other hand, are in the business of borrowing, holding and lending money.  Since interest payments are an integral part of their business, we can't separate out the interest payments that are for "normal business operations" versus "cost of capital".  Thus there is no "EBIT" for financials.  In the graphs, anything related to EBIT and Financials has been set to zero.
