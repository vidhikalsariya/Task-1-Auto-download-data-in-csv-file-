# Task-1-Auto-download-data-in-csv-file-
This Python script downloads intraday/daily OHLCV stock data using yfinance, processes it, resamples it to any timeframe (1m → 5m → 15m → 1h → 1d → 1wk), and saves the final result into a CSV file.  It is useful for:  ✔ Algo traders ✔ Backtesting ✔ Technical analysis ✔ Machine learning datasets ✔ Candle charting

# Features:

✔ Stock data download using yfinance
Example:
SBIN.NS (NSE), AAPL, TSLA, RELIANCE.NS
✔ Auto-detect Datetime Column
Handles both:
Datetime
Date
✔ Converts data into clean rows:
date, time, open, high, low, close, volume
✔ Supports 13 timeframes:
Input	Description
1m	1 Minute
2m	2 Minutes
5m	5 Minutes
15m	15 Minutes
30m	30 Minutes
60m	1 Hour
1h	1 Hour
1d	Daily
5d	5-Day
1wk	Weekly
1mo	Monthly
3mo	Quarterly
✔ Saves final data to CSV

Format:
SBIN.NS_5m.csv

# How the Script Works (Step-by-Step)

1️⃣ User Inputs Required
You enter:
Stock Symbol: SBIN.NS
Start Date: 2025-01-01
End Date: 2025-01-10
Interval: 5m

2️⃣ Script Downloads Data
Using:
df = yf.download(symbol, start, end, interval)
If no data → shows error.

3️⃣ Cleans and Fixes Yahoo’s Data
YFinance sometimes returns:
MultiIndex columns
Date instead of Datetime
Missing time column
Script fixes everything:
✔ Flattens columns
✔ Creates separate date and time
✔ Drops missing rows

4️⃣ Combines Date + Time for Resampling
Creates datetime index:
df['datetime'] = pd.to_datetime(df['date'] + ' ' + df['time'])
df.set_index('datetime')

5️⃣ Resamples Data into Selected Timeframe
Example: Convert 1-minute candles → 5-minute candles
df.resample('5T').agg({
  'open': 'first',
  'high': 'max',
  'low': 'min',
  'close': 'last',
  'volume': 'sum'
})

6️⃣ Converts Datetime Back to Date & Time
For clean CSV output:
date = 2025-01-01
time = 09:30:00

7️⃣ Saves Final Data to CSV
File name:
SBIN.NS_5m.csv
CSV Contains:
date	time	open	high	low	close	volume
🧪 Sample Output (First 5 Rows)
        date        time     open     high      low    close     volume
0   2025-01-01   09:30:00   670.2    672.1    669.5    671.8    120000
1   2025-01-01   09:35:00   671.8    673.0    670.2    672.6     85000

#  How to Use the Script
1️⃣ Install Dependencies
pip install yfinance pandas

2️⃣ Run the Script
python app.py

3️⃣ Enter Required Inputs
Example:
Enter Stock Symbol: SBIN.NS
Enter Start Date: 2025-01-01
Enter End Date: 2025-01-10
Enter Timeframe: 5m

4️⃣ Check Output
Your CSV file will be created in the same folder:
SBIN.NS_5m.csv
