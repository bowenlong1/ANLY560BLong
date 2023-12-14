import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

def plot_daily_change(df, variable, day1, day2, categories=None):
    df['dt'] = pd.to_datetime(df['day_key'], format='%Y%m%d')
    day = df[(day1 <= df.day_key) & (df.day_key <= day2)]

    if categories is None:
        categories = ['[-inf, 4)', 'missing', '[15, 30)']

    df[f'{variable}_cat'] = 0
    df.loc[df[variable].isin(categories), f'{variable}_cat'] = 1

    variable_df = pd.DataFrame(df.groupby('dt')[f'{variable}_cat'].agg(['size', 'sum']).reset_index())
    variable_df['ratio'] = variable_df['sum'] / variable_df['size']

    sns.lineplot(data=variable_df, x="dt", y="ratio")
    plt.xticks(rotation=45)
    plt.title(f'Daily Ratio Change for {variable}')
    plt.xlabel('Date')
    plt.ylabel('Ratio')
    plt.legend(categories, title=f'{variable} Categories', loc='upper right')
    plt.show()

# Set your day1 and day2 variables here
day1 = 20231023
day2 = 20231212

# Set your dataframe df here
# Example: df = ...

# List of categorical variables
categorical_variables = ['cat_dpd_6_mnth_max', 'cat_dpd_5_mnth_max', 'cat_dpd_2_mnth_max']

# Generate line plots for each categorical variable with specific categories
for variable in categorical_variables:
    # Example usage with specific categories
    plot_daily_change(df, variable, day1, day2, categories=['[-inf, 4)', 'missing', '[15, 30)'])

