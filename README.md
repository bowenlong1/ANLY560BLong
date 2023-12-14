### calculate daily change
day1 = 20231023
day2 = 20231212
df['dt'] = pd.to_datetime(df['day_key'], format = '%Y%m%d')
day = df[(day1<=df.day_key)&(df.day_key<=day2)]
day['mnth2_cat'] = 0
day.loc[day.cat_dpd_2_mnth_max.isin(['[-inf, 4)', 'missing', '[15, 30)']), 'mnth2_cat'] = 1
mnth2 = pd.DataFrame(day.groupby('dt')['mnth2_cat'].agg(['size', 'sum']).reset_index())
mnth2['ratio'] = mnth2['sum']/mnth2['size']
sns.lineplot(data=mnth2, x="dt", y="ratio")
