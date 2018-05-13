

```python
import pandas as pd
import numpy as np
```


```python
# read students data csv file
file="raw_data/students_complete.csv"
file1="raw_data/schools_complete.csv"
df_students=pd.read_csv(file)
df_schools=pd.read_csv(file1)
df_students.head()
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
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_schools.head()
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
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>



**District Summary**

* Create a high level snapshot (in table form) of the district's key metrics, including:
  * Total Schools
  * Total Students
  * Total Budget
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)



```python
district_schools_df=df_schools.groupby('type')
no_schools=district_schools_df['name'].agg(np.size)
size_budget=district_schools_df['size','budget'].agg(np.sum)
district_summary=size_budget
district_summary['Number of Schools']=no_schools
district_summary.reset_index(level=0, inplace=True)
```


```python
merged_df=pd.merge(df_schools,df_students,left_on="name",right_on="school",how="outer",suffixes=('_school','_student'))
merged_df['pass_math']=merged_df['math_score']>=70
merged_df['pass_reading']=merged_df['reading_score']>=70

merged_df.head()
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
      <th>School ID</th>
      <th>name_school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>name_student</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>pass_math</th>
      <th>pass_reading</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
district_students=merged_df.groupby('type')
no_students=district_students['name_student'].agg(np.size)
score_data=district_students['reading_score','math_score'].agg(np.mean)
score_data.reset_index(level=0, inplace=True)
pass_nos=district_students['pass_math','pass_reading'].agg(np.sum)
pass_nos.reset_index(level=0, inplace=True)

district_summary1=pd.merge(district_summary,score_data,on="type")
district_summary2=pd.merge(district_summary1,pass_nos,on="type")
# district_summary2.append(no_students)
# pass_read=district_students['pass_reading'].value_counts()
# district_summary['reading_score','math_score']=score_data
# district_summary['pass_math','pass_reading']=pass_nos
```


```python
no_students_d=no_students.to_frame()
no_students_d.reset_index(level=0, inplace=True)
no_students_d
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
      <th>type</th>
      <th>name_student</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Charter</td>
      <td>12194</td>
    </tr>
    <tr>
      <th>1</th>
      <td>District</td>
      <td>26976</td>
    </tr>
  </tbody>
</table>
</div>




```python
district_summary3=pd.merge(district_summary2,no_students_d,on="type")
```


```python
sum_students=district_summary3['name_student'].sum()
district_summary3['pass_math']=district_summary3['pass_math']/sum_students*100
district_summary3['pass_reading']=district_summary3['pass_reading']/sum_students*100
district_summary3['Overall Passing Rate']=(district_summary3['pass_math']+district_summary3['pass_reading'])/2
district_summary3.columns=["type","Total Students","Total Budget","Total Schools","Average Math Score","Average Reading Score",
                          "% Passing Math","% Passing Reading","no_students","Overall Passing Rate"]
del district_summary3['no_students']
district_summary4=district_summary3[["type","Total Schools","Total Budget","Average Math Score","Average Reading Score",
                                   "% Passing Math","% Passing Reading","Overall Passing Rate"]]
```

# District Summary


```python
district_summary4.set_index('type')
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
      <th>Total Schools</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>8</td>
      <td>7301505</td>
      <td>83.902821</td>
      <td>83.406183</td>
      <td>29.170283</td>
      <td>30.086801</td>
      <td>29.628542</td>
    </tr>
    <tr>
      <th>District</th>
      <td>7</td>
      <td>17347923</td>
      <td>80.962485</td>
      <td>76.987026</td>
      <td>45.810569</td>
      <td>55.718662</td>
      <td>50.764616</td>
    </tr>
  </tbody>
</table>
</div>



**School Summary**

* Create an overview table that summarizes key metrics about each school, including:
  * School Name
  * School Type
  * Total Students
  * Total School Budget
  * Per Student Budget
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)



```python
df_students['pass_math']=df_students['math_score']>=70
df_students['pass_reading']=df_students['reading_score']>=70

studentsdf_grouped=df_students.groupby('school')
score_summary=studentsdf_grouped["reading_score","math_score"].agg([np.mean])
score_summary.columns=['Average Reading Score','Average Math Score']
score_summary.reset_index(level=0,inplace=True)
t_students=studentsdf_grouped['reading_score'].agg(np.size)
# t_students.reset_index(level=0,inplace=True)
t_students1=t_students.to_frame(name="total students")


pass_summary=studentsdf_grouped["pass_math","pass_reading"].agg(np.sum)
pass_summary['% Passing Math']=pass_summary['pass_math']/(t_students1['total students'])*100


pass_summary['% Passing Reading']=pass_summary['pass_reading']/(t_students1['total students'])*100
del pass_summary['pass_math']
del pass_summary['pass_reading']
pass_summary['Overall Passing Rate']=(pass_summary["% Passing Math"]+pass_summary["% Passing Reading"])/2
pass_summary.reset_index(level=0,inplace=True)

```


```python
test_score=pd.merge(score_summary,pass_summary,on="school")
# test_score
```


```python
school_summary_df=pd.merge(df_schools,test_score,left_on="name",right_on="school")
del school_summary_df["School ID"];del school_summary_df['school']
school_summary_df1=school_summary_df.rename(columns={"name":"School Name",
                                 "type":"School Type",
                                 "size":"Total Students",
                                  "budget":"Total Budget"
                                 })
school_summary_df1['Per Student Budget']=school_summary_df1['Total Budget']/school_summary_df1['Total Students']
```

# School Summary


```python
school_summary_df1.set_index('School Name',drop=True,inplace=True)
school_summary_df1
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>639.0</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>600.0</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>652.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>625.0</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
      <td>578.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>582.0</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
      <td>628.0</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
      <td>581.0</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
      <td>609.0</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
      <td>583.0</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
      <td>637.0</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
      <td>650.0</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
      <td>644.0</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
      <td>638.0</td>
    </tr>
  </tbody>
</table>
</div>



# Top Performing Schools (By Overall Passing Rate)


```python
school_summary_df1.sort_values("Overall Passing Rate",ascending=False).head()
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>582.0</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
      <td>638.0</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
      <td>609.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>625.0</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
      <td>578.0</td>
    </tr>
  </tbody>
</table>
</div>



# Bottom Performing Schools (By Overall Passing Rate)


```python
school_summary_df1.sort_values("Overall Passing Rate").head()
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
      <td>637.0</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>639.0</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
      <td>650.0</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
      <td>644.0</td>
    </tr>
  </tbody>
</table>
</div>



* Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.



```python
df_students.head()
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
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>pass_math</th>
      <th>pass_reading</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>



# Average Math Score at each Grade Level


```python
df_9thgrade=df_students[df_students['grade']=='9th']
df_10thgrade=df_students[df_students['grade']=='10th']
df_11thgrade=df_students[df_students['grade']=='11th']
df_12thgrade=df_students[df_students['grade']=='12th']
grouped_9th=df_9thgrade.groupby('school')
summary_9th=grouped_9th['math_score'].agg(np.mean)
grouped_10th=df_10thgrade.groupby('school')
summary_10th=grouped_10th['math_score'].agg(np.mean)
grouped_11th=df_11thgrade.groupby('school')
summary_11th=grouped_11th['math_score'].agg(np.mean)
grouped_12th=df_12thgrade.groupby('school')
summary_12th=grouped_12th['math_score'].agg(np.mean)
math_summary_bygrade=pd.concat([summary_9th,summary_10th,summary_11th,summary_12th],axis=1)

```


```python
math_summary_bygrade
# summary_10th
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
      <th>math_score</th>
      <th>math_score</th>
      <th>math_score</th>
      <th>math_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



# Average Reading Score at each Grade Level


```python
df_9thgrade=df_students[df_students['grade']=='9th']
df_10thgrade=df_students[df_students['grade']=='10th']
df_11thgrade=df_students[df_students['grade']=='11th']
df_12thgrade=df_students[df_students['grade']=='12th']
grouped_9th=df_9thgrade.groupby('school')
summary_9th=grouped_9th['reading_score'].agg(np.mean)
grouped_10th=df_10thgrade.groupby('school')
summary_10th=grouped_10th['reading_score'].agg(np.mean)
grouped_11th=df_11thgrade.groupby('school')
summary_11th=grouped_11th['reading_score'].agg(np.mean)
grouped_12th=df_12thgrade.groupby('school')
summary_12th=grouped_12th['reading_score'].agg(np.mean)
reading_summary_bygrade=pd.concat([summary_9th,summary_10th,summary_11th,summary_12th],axis=1)
```


```python
reading_summary_bygrade
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
      <th>reading_score</th>
      <th>reading_score</th>
      <th>reading_score</th>
      <th>reading_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Spending


```python
bins=[570,590,610,630,650,670]
group_names=["570-590","590-610","610-630","630-650","650-670"]
spending_groups=pd.cut(school_summary_df1['Per Student Budget'],bins,labels=group_names)
school_summary_df1['Average Spending Range']=spending_groups
school_summary_df2=school_summary_df1[["Average Spending Range","Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","Overall Passing Rate"]]
grped_df2=school_summary_df2.groupby('Average Spending Range')
school_summary_df3=grped_df2.agg(np.mean)
school_summary_df3

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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Average Spending Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>570-590</th>
      <td>83.455399</td>
      <td>83.933814</td>
      <td>93.460096</td>
      <td>96.610877</td>
      <td>95.035486</td>
    </tr>
    <tr>
      <th>590-610</th>
      <td>83.599686</td>
      <td>83.885211</td>
      <td>94.230858</td>
      <td>95.900287</td>
      <td>95.065572</td>
    </tr>
    <tr>
      <th>610-630</th>
      <td>80.199966</td>
      <td>82.425360</td>
      <td>80.036217</td>
      <td>89.536122</td>
      <td>84.786170</td>
    </tr>
    <tr>
      <th>630-650</th>
      <td>78.229576</td>
      <td>81.492857</td>
      <td>71.998877</td>
      <td>83.757921</td>
      <td>77.878399</td>
    </tr>
    <tr>
      <th>650-670</th>
      <td>76.959583</td>
      <td>81.058567</td>
      <td>66.218444</td>
      <td>81.089710</td>
      <td>73.654077</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Size


```python
bins=[400,1000,2000,3000,4000,5000]
group_names=["400-1000","1000-2000","2000-3000","3000-4000","4000-5000"]
size_groups=pd.cut(school_summary_df1['Total Students'],bins,labels=group_names)
school_summary_df1['Size Group Range']=size_groups
school_summary_df4=school_summary_df1[["Size Group Range","Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","Overall Passing Rate"]]
grped_df_size=school_summary_df4.groupby('Size Group Range')
school_summary_df5=grped_df_size.agg(np.mean)
school_summary_df5

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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Size Group Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>400-1000</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>93.550225</td>
      <td>96.099437</td>
      <td>94.824831</td>
    </tr>
    <tr>
      <th>1000-2000</th>
      <td>83.374684</td>
      <td>83.864438</td>
      <td>93.599695</td>
      <td>96.790680</td>
      <td>95.195187</td>
    </tr>
    <tr>
      <th>2000-3000</th>
      <td>78.429493</td>
      <td>81.769122</td>
      <td>73.462428</td>
      <td>84.473577</td>
      <td>78.968003</td>
    </tr>
    <tr>
      <th>3000-4000</th>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>4000-5000</th>
      <td>77.136883</td>
      <td>80.978256</td>
      <td>66.496861</td>
      <td>81.339570</td>
      <td>73.918215</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Types


```python
school_summary_type=school_summary_df1[["School Type","Average Math Score","Average Reading Score","% Passing Math","% Passing Reading","Overall Passing Rate"]]


group_type=school_summary_type.groupby("School Type")
school_performance_type=group_type.agg(np.mean)
school_performance_type
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>93.620830</td>
      <td>96.586489</td>
      <td>95.103660</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>66.548453</td>
      <td>80.799062</td>
      <td>73.673757</td>
    </tr>
  </tbody>
</table>
</div>


