

```python
import pandas as pd
import numpy as np
from prettytable import PrettyTable
```


```python
# Open and read data from json file
jsonfile="purchase_data.json"
df=pd.read_json(jsonfile)
df=df[["SN","Age","Gender","Item ID","Item Name", "Price"]]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aelalis34</td>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Eolo46</td>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Assastnya25</td>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pheusrical25</td>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aela59</td>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
    </tr>
  </tbody>
</table>
</div>



# Player Count


```python
#**Player Count**
player_count=len(df["SN"].unique())
# players.head()
#making table to output player count
t=PrettyTable(["Total Players"])
t.add_row([player_count])
print(t)
# player_count=players.count()

```

    +---------------+
    | Total Players |
    +---------------+
    |      573      |
    +---------------+
    

# Purchasing Analysis (Total)


```python
uniqueitem_count=len(df["Item Name"].unique())
no_of_purchases=df["Price"].count()
total_revenue=df["Price"].sum()
average_price=total_revenue/no_of_purchases
purchaseanalysistable=PrettyTable(["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"])
purchaseanalysistable.add_row([uniqueitem_count,'${:.2f}'.format(average_price),no_of_purchases,'${:,.2f}'.format(total_revenue)])
print(purchaseanalysistable)


```

    +------------------------+---------------+---------------------+---------------+
    | Number of Unique Items | Average Price | Number of Purchases | Total Revenue |
    +------------------------+---------------+---------------------+---------------+
    |          179           |     $2.93     |         780         |   $2,286.33   |
    +------------------------+---------------+---------------------+---------------+
    


```python
'''function for calculation of purchase data of dataframe of a group'''
def purchase_summary(dataframe,group):
    purchase_count=len(dataframe)
    mean,total=dataframe['Price'].agg([np.mean,np.sum])
    normalized_total=total/len(dataframe['SN'].unique())
    return([group,purchase_count,'${:.2f}'.format(mean),'${:.2f}'.format(total),'${:.2f}'.format(normalized_total)])

```

# Gender Demographics


```python
# **Gender Demographics**

# * Percentage and Count of Female Players
# * Percentage and Count of Other / Non-Disclosed
# creating df of unique players
df_unique_genders=df.drop_duplicates(subset="SN",keep='first')
# Creating list of index
gender_index=df_unique_genders["Gender"].value_counts().index.tolist()
gender_count=df_unique_genders["Gender"].value_counts().values.tolist()
total_count=sum(gender_count)
# * creating list of % count
gender_percentcount=[]
for i in range(len(gender_index)):
    gender_percentcount.append(gender_count[i]/total_count*100)
# Output table
table=PrettyTable(["Gender","Percentage of Players","Total Count"])
for j in range(len(gender_index)):
    table.add_row([gender_index[j],'{:.2f}%'.format(gender_percentcount[j]),gender_count[j]])
print("**Gender Demographics**")
print(table)




```

    **Gender Demographics**
    +-----------------------+-----------------------+-------------+
    |         Gender        | Percentage of Players | Total Count |
    +-----------------------+-----------------------+-------------+
    |          Male         |         81.15%        |     465     |
    |         Female        |         17.45%        |     100     |
    | Other / Non-Disclosed |         1.40%         |      8      |
    +-----------------------+-----------------------+-------------+
    

# Purchase Analysis (Gender)


```python
# Creating Output Table
t=PrettyTable(["Gender","Purchase Count","Average Purchase Price","Total Purchase Value","Normalized Totals"])
# Group by Gender
df_grouped = df.groupby("Gender")

# Separating groups by male
df_grouped_male=df_grouped.get_group('Male')
purchase_males=purchase_summary(df_grouped_male,"Male")
t.add_row([purchase_males[0],purchase_males[1],purchase_males[2],purchase_males[3],purchase_males[4]])

# Separating groups by female
df_grouped_female=df_grouped.get_group('Female')
purchase_females=purchase_summary(df_grouped_female,"Female")
t.add_row([purchase_females[0],purchase_females[1],purchase_females[2],purchase_females[3],purchase_females[4]])

# Separating groups by female
df_grouped_other=df_grouped.get_group('Other / Non-Disclosed')
purchase_other=purchase_summary(df_grouped_other,"Other / Non-Disclosed")
t.add_row([purchase_other[0],purchase_other[1],purchase_other[2],purchase_other[3],purchase_other[4]])
print("Purchase Analysis (Gender)")

print(t)
```

    Purchase Analysis (Gender)
    +-----------------------+----------------+------------------------+----------------------+-------------------+
    |         Gender        | Purchase Count | Average Purchase Price | Total Purchase Value | Normalized Totals |
    +-----------------------+----------------+------------------------+----------------------+-------------------+
    |          Male         |      633       |         $2.95          |       $1867.68       |       $4.02       |
    |         Female        |      136       |         $2.82          |       $382.91        |       $3.83       |
    | Other / Non-Disclosed |       11       |         $3.25          |        $35.74        |       $4.47       |
    +-----------------------+----------------+------------------------+----------------------+-------------------+
    

# Age Demographics


```python
# **Age Demographics**
df_unique_players=df.drop_duplicates(subset="SN",keep='first')
# Creating bins for age datta
group_names=["<10yrs","10-14yrs","15-19yrs","20-24yrs","25-30yrs","31-45yrs"]
bins=[0,9,14,19,24,30,45]
age_group=pd.cut(df_unique_players["Age"],bins,labels=group_names)

# Adding age group column
df_unique_players['Age Groups']=age_group
# df_unique_players.head()

# Value_counts
key=df_unique_players['Age Groups'].value_counts().index.tolist()
value=df_unique_players['Age Groups'].value_counts().values.tolist()

total_players=sum(value)
percent_age_group=[]
for element in value:
    percent_age_group.append(element/total_players*100)

age_demographics=pd.DataFrame({
                            'Age Group':key,
                            '% of Players':percent_age_group,
                            'Total-Count':value
                            },index=key)

# fixing format by mapping
age_demographics['% of Players']=age_demographics['% of Players'].map('{:.2f}%'.format)
age_demographics=age_demographics[['% of Players','Total-Count']]
print('Age Demographics')
print(age_demographics.reindex(group_names))

```

    Age Demographics
             % of Players  Total-Count
    <10yrs          3.32%           19
    10-14yrs        4.01%           23
    15-19yrs       17.45%          100
    20-24yrs       45.20%          259
    25-30yrs       17.45%          100
    31-45yrs       12.57%           72
    

    C:\ProgramData\Anaconda3\envs\PythonData\lib\site-packages\ipykernel\__main__.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    

# Purchase Analysis By Age


```python
'''* Purchase analysis by age demographics'''


# Grouping df into age groups, bins and labels used from previous cell
df_agegroup=pd.cut(df["Age"],bins,labels=group_names)

# Adding age group column
df['Age Groups']=df_agegroup
df.head()

# grouping by age groups
group_by_age=df.groupby('Age Groups')
#loop to process each age group
purchase_data_grp=[]
for group in group_names:
    to_process=group_by_age.get_group(group)
    purchase_data_grp.append(purchase_summary(to_process,group))
# purchase_data_grp
# # get groups
# group1=group_by_age.get_group('<10yrs')
# purchase_data=purchase_age(group1,'<10yrs')

labels=['Age Group','Purchase Count','Average Purchase','Total Purchase','Normalized Totals']
output_df=pd.DataFrame.from_records(purchase_data_grp,columns=labels)
output_df.set_index('Age Group')
print("Purchase Analysis (Age)")
print(output_df)
#   * Purchase Count
#   * Average Purchase Price
#   * Total Purchase Value
#   * Normalized Totals

```

    Purchase Analysis (Age)
      Age Group  Purchase Count Average Purchase Total Purchase Normalized Totals
    0    <10yrs              28            $2.98         $83.46             $4.39
    1  10-14yrs              35            $2.77         $96.95             $4.22
    2  15-19yrs             133            $2.91        $386.42             $3.86
    3  20-24yrs             336            $2.91        $978.77             $3.78
    4  25-30yrs             143            $2.98        $426.34             $4.26
    5  31-45yrs             105            $2.99        $314.39             $4.37
    


```python
# **Top Spenders**

# * Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
    # Group by the individual player
df_player_grouped=df.groupby('SN')
player_index=df['SN'].value_counts().index.tolist()

# Loop every player to apply purchase_summary function
purchasedata_player=[]
for sn in player_index:
    df_getgroup=df_player_grouped.get_group(sn)
    purchasedata_player.append(purchase_summary(df_getgroup,sn))
# Creating df containg aggregate data for each player
labels=['SN','Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Total']
df_eachplayer_agg=pd.DataFrame.from_records(purchasedata_player,columns=labels) #make df using rows
df_eachplayer_agg['Total Purchase Value']=df_eachplayer_agg['Total Purchase Value'].str.replace('$','') #remove $ sign 
df_eachplayer_agg['Total Purchase Value']=pd.to_numeric(df_eachplayer_agg['Total Purchase Value']) #convert to numeric
# print(df_eachplayer_agg.dtypes)
df_eachplayer_agg.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>5</td>
      <td>$3.41</td>
      <td>17.06</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Qarwen67</td>
      <td>4</td>
      <td>$2.49</td>
      <td>9.97</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saedue76</td>
      <td>4</td>
      <td>$3.39</td>
      <td>13.56</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sondastan54</td>
      <td>4</td>
      <td>$2.56</td>
      <td>10.24</td>
      <td>$10.24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hailaphos89</td>
      <td>4</td>
      <td>$1.47</td>
      <td>5.87</td>
      <td>$5.87</td>
    </tr>
  </tbody>
</table>
</div>



# Top Spenders


```python
df_sorted=df_eachplayer_agg.sort_values('Total Purchase Value',ascending=False)
df_sorted=df_sorted[['SN','Purchase Count','Average Purchase Price','Total Purchase Value']]
print('Top Spenders')
print(df_sorted.head())
```

    Top Spenders
                 SN  Purchase Count Average Purchase Price  Total Purchase Value
    0   Undirrala66               5                  $3.41                 17.06
    2      Saedue76               4                  $3.39                 13.56
    5   Mindimnya67               4                  $3.18                 12.74
    27   Haellysu29               3                  $4.24                 12.73
    20       Eoda93               3                  $3.86                 11.58
    


```python
# Groupby item ID
df_itemid=df.groupby('Item ID')
id_index=df['Item ID'].value_counts().index.tolist()

# applying function to the grouped df
item_data=[];item_name=[]
for element in id_index:
    df_item=df_itemid.get_group(element)
    item_data.append(purchase_summary(df_item,element))
    indx=df_item.index[df_item['Item ID'] == element]
    item_name.append(df_item.loc[indx,'Item Name'])
item_df=pd.DataFrame.from_records(item_data,columns=['Item ID','Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Total'])
item_df=item_df[['Item ID','Purchase Count','Average Purchase Price','Total Purchase Value']]
item_df['Average Purchase Price']=item_df['Average Purchase Price'].str.replace('$','')
item_df['Average Purchase Price']=pd.to_numeric(item_df['Average Purchase Price'])
item_df['Total Purchase Value']=item_df['Total Purchase Value'].str.replace('$','')
item_df['Total Purchase Value']=pd.to_numeric(item_df['Total Purchase Value'])
item_df['Item Name']=pd.Series(item_name)
item_df=item_df[['Item ID','Item Name','Purchase Count','Average Purchase Price','Total Purchase Value']]
item_df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84</td>
      <td>116    Arcane Gem
171    Arcane Gem
297    Arc...</td>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>39</td>
      <td>61     Betrayal, Whisper of Grieving Widows
62...</td>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31</td>
      <td>56     Trickster
75     Trickster
218    Trick...</td>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>3</th>
      <td>34</td>
      <td>57     Retribution Axe
126    Retribution Axe
...</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>4</th>
      <td>175</td>
      <td>81     Woeful Adamantite Claymore
121    Woefu...</td>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
  </tbody>
</table>
</div>



# 5 Most Popular Items (by purchase count)


```python
# * Identify the 5 most popular items by purchase count, then list (in a table):

sort_item_df=item_df.sort_values('Purchase Count',ascending=False)
# print('Most Popular Item')
sort_item_df.set_index('Item ID').head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>116    Arcane Gem
171    Arcane Gem
297    Arc...</td>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>39</th>
      <td>61     Betrayal, Whisper of Grieving Widows
62...</td>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>31</th>
      <td>56     Trickster
75     Trickster
218    Trick...</td>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <td>57     Retribution Axe
126    Retribution Axe
...</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>175</th>
      <td>81     Woeful Adamantite Claymore
121    Woefu...</td>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
  </tbody>
</table>
</div>



# 5 Most Profitable Items 


```python
# *  5 most profitable items by total purchase value, then list (in a table):

profitable_item=item_df.sort_values('Total Purchase Value',ascending=False)
profitable_item.set_index('Item ID').head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>57     Retribution Axe
126    Retribution Axe
...</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>107    Spectral Diamond Doomblade
250    Spect...</td>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>50     Orenmir
83     Orenmir
227    Orenmir
3...</td>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>100    Singed Scalpel
468    Singed Scalpel
47...</td>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>164    Splitter, Foe Of Subtlety
275    Splitt...</td>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


