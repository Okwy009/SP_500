# S_P_500
## Overview
The S&P 500 is a major benchmark index tracking 500 large companies listed in the United States, widely used as an indicator of overall market performance. This project analyzes stock data for these companies, providing insights that can guide investment and portfolio management decisions.

This project provides an in-depth analysis of S&P 500 companies, focusing on price trends, trading volumes, sector/industry performance, and volatility. Using Python (Pandas) for data preprocessing and Power BI for dashboard creation, the project aims to deliver actionable insights into stock performance and market trends.

The data for this project was sourced from Kaggle and carefully merged to ensure comprehensive historical coverage of S&P 500 stock data. The selected datasets include essential variables such as:
- **Stock prices:** Open, High, Low, and Close prices for each stock.
- **Volume traded:** Total volume of shares traded.
- **Sector Classification:** Industry categorization for each stock.
- **Date:** Timestamp for each observation.

## Questions To answer
In this project, I will perform data manipulation, analysis, and create visualization and dashboard Using Python and Power BI to answer these key questions.

1. How have the adjusted close prices of selected stocks (AAPL, MSFT, and GOOGL) trended over time?
2. How do different sectors and industries perform in terms of returns and risk (volatility) over time?
3. - How does trading volume vary across different sectors or symbols?
    - What does this variation indicate about stock liquidity and investor interest?

## Tools I Used
From Data Sourcing to creating Visualization for this project, I utilized the following tools:
- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data.
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals.
- **Power BI:** To create an interactive dashboard, visualizing stock performance and comparing sector trends.
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

## Data Preparation, Merging and Cleanup
- Data Preparation
```python
# Loading Libraries 
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt


# Loading datasets
file_path = r'C:\Users\hp\.cache\kagglehub\datasets\andrewmvd\sp-500-stocks\versions\963\sp500_stocks.csv'

df_sp500 = pd.read_csv(file_path)
df_companies = pd.read_csv(r'C:\Users\hp\OneDrive\Desktop\Data_Analyst\S&P_500\sp500_data\sp500_companies.csv')
```

- Merging Datasets
```python
# Merging Datasets
merged_df = pd.merge(df_cleaned, df_companies, on='Symbol', how='inner')
```

- Data Cleanup
```python
# Cleaning df_sp500
# Convert Date to datetime format
df_sp500['Date'] = pd.to_datetime(df_sp500['Date'])

df_sp500['Symbol'] = df_sp500['Symbol'].astype('category')

df_cleaned = df_sp500.dropna()

# Cleaning the merged datasets
# Remove Duplicates
merged_df.drop_duplicates(inplace=True)
# Ensure numerical columns are of the correct type
numerical_type_cols = ['Currentprice', 'Marketcap', 'Volume', 'Weight']
merged_df[numerical_type_cols] = merged_df[numerical_type_cols].apply(pd.to_numeric, errors='coerce')

# Handling the missing values  

#  Fill missing Ebitda values with the sector median
merged_df['Ebitda'] = merged_df.groupby('Sector')['Ebitda'].transform(lambda x: x.fillna(x.median()))

# Impute Revenuegrowth with sector median
merged_df['Revenuegrowth'] = merged_df.groupby('Sector')['Revenuegrowth'].transform(lambda x: x.fillna(x.median()))

# Fill missing states with a placeholder
merged_df['State'] = merged_df['State'].fillna("Unknown")
```

## The Analysis
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

### 1. How have the adjusted close prices of selected stocks (AAPL, MSFT, and GOOGL) trended over time?

To evaluate performance trends for AAPL, MSFT, and GOOGL, I analyzed their adjusted close prices over time, visualizing long-term trajectories to highlight growth, volatility, and resilience under varying market conditions. This analysis compares each stock's relative performance and identifies key growth patterns.

Additionally, I assessed average price trends across the top ten industries, offering insights into sectoral growth drivers, industry responses to economic changes, and potential areas for investment or diversification. Together, these stock-specific and industry-wide insights provide a well-rounded view of market dynamics, supporting strategic portfolio management decisions.

View my notebook with detailed steps here: [3_Price_Trend](project\3_Price_Trends.ipynb)

### Visualization
```python
# Plot Price Trends for filtered symbols
plt.figure(figsize=(12, 6))
for symbol in symbols:
    subset = stock_data[stock_data['Symbol'] == symbol]
    plt.plot(subset['Date'], subset['Adj Close'], label=symbol)

# Rotate x-axis labels for better visibility
plt.xticks(rotation=45, ha='right')

# Set title and labels
plt.title('Price Trends of Selected Stocks Over Time', fontsize=16)
plt.xlabel('Date', fontsize=14)
plt.ylabel('Adjusted Close Price', fontsize=14)

# Adjust legend position and size
plt.legend()

# Optimize layout to prevent overlap
plt.tight_layout()

# Add grid lines
plt.grid(True)
plt.grid(color='gray', linestyle='--', linewidth=0.7, alpha=0.8)

# Improve x-axis formatting
plt.gca().xaxis.set_major_locator(mdates.MonthLocator(interval=6))  # Display every 6th month
plt.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m'))

# Show the plot
plt.show()
```

### Results
![Price Trends of Selected Stocks Over Time](images\Price_Trends_of_Selected_Stocks_Over_Time.png) Price Trends of Selected Stocks Over Time
### Insights
1. **Long-Term Growth Trends:** All three stocks have shown significant long-term growth, with notable increases after specific periods. Microsoft (MSFT), for example, has an especially sharp growth trajectory beginning around the late 2010s, suggesting a strong performance in recent years.
2. **Comparison of Stock Performance:** Microsoft (MSFT) appears to have outperformed both Apple (AAPL) and Google (GOOGL) over the entire period in terms of stock price growth. Google and Apple follow a more steady, yet still strong, upward trend.
3. **Volatility Differences:** While all three stocks exhibit periods of volatility, some, like Microsoft, appear to have had more dramatic swings, particularly in recent years. This suggests that MSFT may have been more reactive to certain market changes or company-specific events.
4. **Relative Performance Post-2000s:** Starting in the early 2000s, each of these stocks began to grow more significantly. The growth trajectories align with major tech advancements and increased consumer reliance on technology, underscoring the growth of the tech sector as a whole.

### 2. How do different sectors and industries perform in terms of returns and risk (volatility) over time?
To figure out how different sectors and industries perform in terms of returns and risk (volatility) over time, I calculated and visualized sector returns and volatility over time, with each plot focusing on specific financial metrics for sectors across different dates.
View my notebook with detailed steps here: [4_Sector_Industry_Comparison](project\4_Sector_Industry_Comparison.ipynb)

### Visualization
```python
# Plot Sector Returns
sector_returns.plot(ax=ax[0], legend=False)  # Temporarily hide legend for this plot
ax[0].set_title('Average Daily Returns by Sector')
ax[0].set_ylabel('Average Return')
ax[0].grid()

# Add legend outside for Sector Returns
handles, labels = ax[0].get_legend_handles_labels()
fig.legend(handles, labels, loc='center left', bbox_to_anchor=(1.05, 0.5), title='Sectors')

# Plot Sector Volatility
sector_volatility.plot(ax=ax[1], colormap='tab20', legend=False)  # Temporarily hide legend for this plot
ax[1].set_title('Rolling Volatility by Sector')
ax[1].set_ylabel('30-Day Rolling Volatility')
ax[1].grid()
plt.xlabel('Date')

# Adjust layout and show
plt.tight_layout(rect=[0, 0, 0.85, 1])  # Leave space for the legend on the right
plt.show()
```
### Results
![Volatility V Returns by Sector](images\Volatility_V_Returns_by_Sector.png) Volatility V Returns by Sector

### Insights
1. **Market Volatility and Economic Shocks:**

- Both charts reveal a sharp increase in volatility around late 2019 and early 2020, which aligns with the onset of the COVID-19 pandemic. This event caused significant market disruptions, with heightened fluctuations in both daily returns and volatility across all sectors.

- The average daily returns in this period show extreme negative and positive swings, indicating increased uncertainty and varying responses among sectors during the economic downturn and subsequent recovery.

2. **Sector Stability and Volatility Levels:**

- Certain sectors appear more volatile than others throughout the timeline. For example, Energy and Financial Services sectors tend to exhibit higher peaks in volatility, which may be due to their sensitivity to macroeconomic factors like oil prices and interest rates.

- Utilities and Consumer Defensive sectors generally show more stable returns and lower volatility, reflecting their traditional role as "defensive" sectors that tend to be more resilient during economic downturns.

3. **Investment Strategies Based on Volatility:**

- The rolling volatility chart indicates that investors may find opportunities in low-volatility sectors (like Utilities and Consumer Defensive) for safer, more predictable returns, especially during uncertain times.

- Conversely, high-volatility sectors (like Technology and Financial Services) may offer higher potential returns but come with increased risk, making them suitable for growth-oriented investors during bullish markets. 


## 3. Volume_Trends
- How does trading volume vary across different sectors or symbols?
- What does this variation indicate about stock liquidity and investor interest?

I first calculated the total trading volume for each sector on a daily basis, organizing the data into a structured table where each sector's daily volume is represented in separate columns. This organization supports a clear visualization of volume trends across sectors.

Additionally, I computed a 30-day moving average of the trading volume for individual stocks, specifically filtering for Apple Inc. (AAPL). This setup allows for a targeted analysis of AAPL's volume trends by comparing its daily trading volume to its 30-day moving average, offering insights into fluctuations in trading activity and investor interest over time.
View my notebook for detailed steps:
```python
plt.figure(figsize=(12, 8))

# Plot daily volume
plt.plot(stock_vol['Date'], stock_vol['Volume'], label='Daily Volume', color='blue', alpha=0.6)

# Plot 30-day moving average volume
plt.plot(stock_vol['Date'], stock_vol['Volume_MA'], label='30-Day MA Volume', color='orange', linewidth=2)

# Improve x-axis formatting
plt.gca().xaxis.set_major_locator(mdates.MonthLocator(interval=3))  # Display every 3rd month
plt.gca().xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m'))
plt.xticks(rotation=45, ha='right')  # Rotate x-axis labels for better readability

plt.title(f'Trading Volume for {symbol} Over Time')
plt.xlabel('Date')
plt.ylabel('Volume')
plt.legend()
plt.grid(True)
plt.tight_layout()  # Adjust layout to prevent clipping of labels
plt.show()
```

### Results
![Volume Trends](images\Trading_Volume_for_APPL_Over_Time.png)

### Insights
1. Overall Decline in Trading Volume:

- The chart shows a clear trend of decreasing trading volume over time. In the early years, AAPL experienced very high trading volumes, which have gradually declined. This could indicate a change in investor interest or trading patterns, possibly due to Apple's stock stabilizing or transitioning from a high-growth, high-volatility period to a more mature phase.

2. Volatility in Early Trading Activity:

- Early in the timeline, there is significant fluctuation in daily trading volumes, suggesting a period of heightened market activity. This could reflect Apple’s early growth stages, where news, product launches, and market speculation drove more frequent and larger swings in trading volume.

3. 30-Day Moving Average as a Trend Indicator:

- The 30-day moving average (orange line) provides a smoother trend of trading volume, making it easier to see the overall trajectory. This line shows a gradual decline but also highlights periodic surges, possibly coinciding with significant events for Apple, such as product announcements or earnings reports.

4. Seasonal or Event-Driven Volume Spikes:

- While there is a general decline, occasional spikes are visible, suggesting certain events temporarily increased trading activity. These spikes may align with major product releases, quarterly earnings reports, or broader market events impacting tech stocks, reflecting a surge in trading interest during such times.

## Conclusion
