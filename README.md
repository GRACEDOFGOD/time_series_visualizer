# time_series_visualizer
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

def draw_line_plot():
    # Load the data
    df = pd.read_csv('fcc-forum-pageviews.csv', index_col='date', parse_dates=True)

    # Clean the data
    df = df[(df['value'] <= df['value'].quantile(0.975)) & (df['value'] >= df['value'].quantile(0.025))]

    # Draw a line plot
    plt.figure(figsize=(10, 5))
    plt.plot(df.index, df['value'], color='blue', linewidth=2)
    plt.title('Daily freeCodeCamp Forum Page Views 5/2016-12/2019')
    plt.xlabel('Date')
    plt.ylabel('Page Views')
    plt.xticks(rotation=45)
    plt.tight_layout()

    # Save the figure
    plt.savefig('line_plot.png')
    plt.show()


def draw_bar_plot():
    # Load the data
    df = pd.read_csv('fcc-forum-pageviews.csv', index_col='date', parse_dates=True)

    # Clean the data
    df = df[(df['value'] <= df['value'].quantile(0.975)) & (df['value'] >= df['value'].quantile(0.025))]

    # Create a new dataframe for the bar plot
    df['year'] = df.index.year
    df['month'] = df.index.month_name()

    # Calculate average daily page views for each month grouped by year
    bar_data = df.groupby(['year', 'month'])['value'].mean().unstack()

    # Draw a bar plot
    bar_data.plot(kind='bar', figsize=(10, 5))
    plt.title('Average Daily Page Views per Month')
    plt.xlabel('Years')
    plt.ylabel('Average Page Views')
    plt.legend(title='Months', labels=bar_data.columns)
    plt.xticks(rotation=45)
    plt.tight_layout()

    # Save the figure
    plt.savefig('bar_plot.png')
    plt.show()


def draw_box_plot():
    # Load the data
    df = pd.read_csv('fcc-forum-pageviews.csv', index_col='date', parse_dates=True)

    # Clean the data
    df = df[(df['value'] <= df['value'].quantile(0.975)) & (df['value'] >= df['value'].quantile(0.025))]

    # Create a new dataframe for the box plot
    df['year'] = df.index.year
    df['month'] = df.index.month_name()

    # Draw a box plot
    plt.figure(figsize=(12, 6))

    # Year-wise box plot
    plt.subplot(1, 2, 1)
    sns.boxplot(x='year', y='value', data=df)
    plt.title('Year-wise Box Plot (Trend)')
    plt.xlabel('Year')
    plt.ylabel('Page Views')

    # Month-wise box plot
    plt.subplot(1, 2, 2)
    sns.boxplot(x='month', y='value', data=df, order=[
        'January', 'February', 'March', 'April', 'May', 'June',
        'July', 'August', 'September', 'October', 'November', 'December'
    ])
    plt.title('Month-wise Box Plot (Seasonality)')
    plt.xlabel('Month')
    plt.ylabel('Page Views')

    plt.tight_layout()

    # Save the figure
    plt.savefig('box_plot.png')
    plt.show()


# Uncomment to run the plots
# draw_line_plot()
# draw_bar_plot()
# draw_box_plot()
