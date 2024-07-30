# OIB DS Task2
import pandas as pd
import seaborn as sns
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np
import plotly.figure_factory as ff
import plotly.express as px
import plotly.graph_objects as go
%matplotlib inline

from google.colab import drive
drive.mount('/content/drive')

unemp_data = pd.read_csv('/content/drive/MyDrive/Unemployment in India.csv')

unemp_data.describe()

unemp_data.isnull().sum()

unemp_data.duplicated().sum()

unemp_data.columns=['state','date','frequency','estimated unemployment rate','estimated employed','estimated labour participation rate','area']

unemp_data['state'].value_counts().idxmax()
unemp_data['state'].value_counts().idxmin()

import pandas as pd
import calendar

unemp_data['date'] = pd.to_datetime(unemp_data['date'], dayfirst=True)

unemp_data['month_int'] = unemp_data['date'].dt.month

unemp_data['month'] = unemp_data['month_int'].apply(lambda x: calendar.month_abbr[int(x)] if pd.notnull(x) else '')

unemp_data['month'].value_counts().idxmax()
unemp_data['month'].value_counts().idxmin()

unemp_data.drop(columns=['frequency','month'])

df=unemp_data[['state','estimated unemployment rate']].groupby('state').sum().sort_values('estimated unemployment rate',ascending=False)

fig=plt.figure()
ax0=fig.add_subplot(1,2,1)
df[:10].plot(kind='bar',color='red',figsize=(30,5),ax=ax0)
ax0.set_title('Top 10 states with highest unemployment rate')
ax0.set_xlabel('States')
ax0.set_ylabel('Unemployment Rate')

df1=unemp_data[['month','estimated unemployment rate']].groupby('month').sum().sort_values(by='estimated unemployment rate',ascending=False)
df1.head(10)

fig=plt.figure()
ax0=fig.add_subplot(1,2,1)
df1[:12].plot(kind='bar',color='pink',figsize=(30,5),ax=ax0)
ax0.set_title('month with highest unemployment rate')
ax0.set_xlabel('month')
ax0.set_ylabel('Unemployment Rate')

from os import name
unemp_data_EEE=unemp_data.groupby(['month'])[['estimated unemployment rate','estimated employed','estimated labour participation rate']].mean()
unemp_data_EEE=pd.DataFrame(unemp_data_EEE).reset_index()
month=unemp_data_EEE.month
unemployment_rate=unemp_data_EEE['estimated unemployment rate']
labour_participation_rate=unemp_data_EEE['estimated labour participation rate']
fig=go.Figure()
fig.add_trace(go.Bar (x=month,y=unemployment_rate,name='unemployment rate'))
fig.add_trace(go.Bar (x=month, y=labour_participation_rate,name='labour participation rate'))
fig.update_layout(title='unemployment rate and labour participation rate',xaxis={'categoryorder':'array','categoryarray':['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']})
fig.show()

df2=unemp_data[['state','estimated employed']].groupby('state').sum().sort_values(by='estimated employed',ascending=False)
df2.head(10)

df2=unemp_data[['state','estimated employed']].groupby('state').sum().sort_values(by='estimated employed',ascending=False)
df2.head(10)

fig=plt.figure()
ax1=fig.add_subplot(1,2,2)
df2[:10].plot(kind='bar',color='green',figsize=(15,6),ax=ax1)
ax1.set_title('estimated emloyed people in every state')
ax1.set_xlabel('States')
ax1.set_ylabel('no of emplyed')

df2_a=unemp_data[['state','estimated unemployment rate']].groupby('state').sum().sort_values('estimated unemployment rate',ascending=False)
df2_a

df2_a=unemp_data[['state','estimated unemployment rate']].groupby('state').sum().sort_values('estimated unemployment rate',ascending=False)
df2.head(10)

fig=plt.figure()
ax1=fig.add_subplot(1,2,2)
df2[:10].plot(kind='bar',color='green',figsize=(15,6),ax=ax1)
ax1.set_title('estimated emloyed rate in every state')
ax1.set_xlabel('States')
ax1.set_ylabel('estimated employed rate (in%)')

df2_a=unemp_data[['state','estimated unemployment rate']].groupby('state').sum().sort_values('estimated unemployment rate',ascending=False)
df2_a

df2_a=unemp_data[['state','estimated unemployment rate']].groupby('state').sum().sort_values('estimated unemployment rate',ascending=False)
df2_a.head(10)

fig=plt.figure()
ax1=fig.add_subplot(1,2,2)
df2_a[:10].plot(kind='bar',color='green',figsize=(15,6),ax=ax1)
ax1.set_title('estimated unemloyed rate in every state')
ax1.set_xlabel('States')
ax1.set_ylabel('estimated unemployed rate (in%)')

df3=unemp_data[['month','estimated employed']].groupby('month').sum().sort_values('estimated employed',ascending=False)
df3

df3=unemp_data[['month','estimated employed']].groupby('month').sum().sort_values('estimated employed',ascending=False)
df3.head(10)

df3=unemp_data[['month','estimated employed']].groupby('month').sum().sort_values('estimated employed',ascending=False)
df3.head(10)

fig=plt.figure()
ax1=fig.add_subplot(1,2,2)
df3[:10].plot(kind='bar',color='red',figsize=(15,6),ax=ax1)
ax1.set_title('estimated emloyed people in every month')
ax1.set_title('month')
ax1.set_ylabel('no of emplyed')

fig=px.bar(unemp_data,x='state',y='estimated unemployment rate',animation_frame='month',color='state',title='unemployment rate in every month')
fig.update_layout(xaxis={'categoryorder':'total descending'})
fig.layout.updatemenus[0].buttons[0].args[1]['frame']['duration']=2000
fig.show()
