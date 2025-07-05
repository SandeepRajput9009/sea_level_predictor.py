# sea_level_predictor.py
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress


def draw_plot():
    # Read data
    df = pd.read_csv('epa-sea-level.csv')

    # Create scatter plot
    fig, ax = plt.subplots(figsize=(12, 6))
    ax.scatter(df['Year'], df['CSIRO Adjusted Sea Level'], label='Observed Data')

    # First line of best fit (1880 to 2050)
    res1 = linregress(df['Year'], df['CSIRO Adjusted Sea Level'])
    x_pred1 = pd.Series(range(1880, 2051))
    y_pred1 = res1.slope * x_pred1 + res1.intercept
    ax.plot(x_pred1, y_pred1, 'r', label='Best Fit: 1880-2050')

    # Second line of best fit (2000 to 2050)
    df_recent = df[df['Year'] >= 2000]
    res2 = linregress(df_recent['Year'], df_recent['CSIRO Adjusted Sea Level'])
    x_pred2 = pd.Series(range(2000, 2051))
    y_pred2 = res2.slope * x_pred2 + res2.intercept
    ax.plot(x_pred2, y_pred2, 'g', label='Best Fit: 2000-2050')

    # Chart formatting
    ax.set_title('Rise in Sea Level')
    ax.set_xlabel('Year')
    ax.set_ylabel('Sea Level (inches)')
    ax.legend()

    # Save and return
    fig.savefig('sea_level_plot.png')
    return fig
