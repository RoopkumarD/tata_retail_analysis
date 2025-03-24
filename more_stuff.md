- think of each analysis as something business would like to know, like here what are peak times as
  that time business will prepare itself well and also run ads. Have a open mind and think of ways
  the information can be used by business to generate more money.

- In per day sales plot, we can see most of the sales occur during 12pm - 2 pm, these are the times
when most customer are in the websites (cause online retail). Thus these are the times when we can
post ads in the website to promote more sales.

- plot trick to show K in y axis

```python
from matplotlib.ticker import StrMethodFormatter
plt.plot(BestTimeAdds['Hour'],BestTimeAdds['InvoiceNo']/1000)
plt.xlabel('Hour')
plt.ylabel('Sales')
plt.grid()
formatter = StrMethodFormatter('{x:.0f}k')
plt.gca().yaxis.set_major_formatter(formatter)
plt.show()
```

- sold together products

```python
soldTogether = data.groupby("InvoiceNo")['Description'].agg(lambda x : " , ".join(x)).reset_index()
soldTogether #we got items that are sold together, separated by ","
```

```python
from itertools import combinations
from collections import Counter
count = Counter()
for row in soldTogether['Description']:
    row_list = row.split(",")
    #item mostly solved together , here it shows 2 items sold together,we can change it to 3
    #to show 3 items sold together and so on ...
    count.update(Counter(combinations(row_list,2)))
#most_common is method from collections
for key,value in count.most_common(10):
    print(key,value)
```

- which day has most sales => kind of important because like how mum dad prepare early in Sundays to
  handle many customers. They can clearly check if all servers are working well or not to not miss
  out this big amount requests.

- rfm segmentation

```python
#Our Customer Segmentation
seg_map = {
r'[1-2][1-2][1-5]': 'Hibernating',
r'[1-2][3-4][1-5]': 'At risk',
r'[1-2]5[1-5]' :'Cannot lose them',
r'3[1-2][1-5]' : 'About to sleep',
r'33[1-5]' : 'Need Attention',
r'[3-4][4-5][1-5]' : 'Loyal Customers',
r'[4-5][1-3][1-5]' : 'Good Potential',
r'5[4-5][1-5]' : 'Champions',
}
rfm['Segment'] = rfm['RFM_Score'] .replace(seg_map ,regex =True)
```

- Use machine learning to predict which customer will spend how much for each segmentation.
Like for at risk customer, find the amount of money they would have spent if they kept buying from
the business.

- Heirarchical plot / Treemap for countries

```python
country_sales = df.groupby('Country')['Total_sales'].sum().reset_index()

# Create a hierarchical plot using Plotly Express
fig = px.treemap(country_sales, path=['Country'], values='Total_sales', title='Hierarchical Sales by Country')
fig.update_layout(template='plotly_dark')
fig.show()
```

- Which customers or countries are buy those top selling or top revenue generating products. So to
  understand if they are concentrated which means these small group is responsible for the product
  or spread out
