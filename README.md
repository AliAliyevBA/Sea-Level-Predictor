# Sea-Level-Predictor
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

def load_data(file_path):
    df = pd.read_csv(file_path)
    return df

def create_scatter_plot(df):
    # Create scatter plot
    plt.figure(figsize=(10, 6))
    plt.scatter(df['Year'], df['CSIRO Adjusted Sea Level'], color='blue', label='Data')

    slope, intercept, r_value, p_value, std_err = linregress(df['Year'], df['CSIRO Adjusted Sea Level'])
    
    x_fit = pd.Series(range(1880, 2051))
    y_fit = slope * x_fit + intercept
    plt.plot(x_fit, y_fit, color='red', label='Best Fit Line (1880 - 2050)')

    df_recent = df[df['Year'] >= 2000]
    
    slope_recent, intercept_recent, r_value_recent, p_value_recent, std_err_recent = linregress(df_recent['Year'], df_recent['CSIRO Adjusted Sea Level'])
    
    y_fit_recent = slope_recent * x_fit + intercept_recent
    plt.plot(x_fit, y_fit_recent, color='green', label='Best Fit Line (2000 - 2050)')

    plt.title('Rise in Sea Level')
    plt.xlabel('Year')
    plt.ylabel('Sea Level (inches)')
    plt.legend()
    plt.grid()
    plt.tight_layout()

    plt.savefig('sea_level_plot.png')
    plt.show()

def main():
    df = load_data('epa-sea-level.csv')
    
    create_scatter_plot(df)

if __name__ == "__main__":
    main()
