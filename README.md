# EDA-of-Healthcare-
import pandas as pd
import numpy as np
import datetime
from time import strftime
import matplotlib.pyplot as plt
%matplotlib inline

import seaborn as sns
base_data =pd.read_csv("C:\\Users\\anssh\\Downloads\\healthcare eda.csv")
base_data
PatientId	AppointmentID	Gender	ScheduledDay	AppointmentDay	Age	Neighbourhood	Scholarship	Hipertension	Diabetes	Alcoholism	Handcap	SMS_received	No-show
0	2.987250e+13	5642903	F	2016-04-29T18:38:08Z	2016-04-29T00:00:00Z	62	JARDIM DA PENHA	0	1	0	0	0	0	No
1	5.589980e+14	5642503	M	2016-04-29T16:08:27Z	2016-04-29T00:00:00Z	56	JARDIM DA PENHA	0	0	0	0	0	0	No
2	4.262960e+12	5642549	F	2016-04-29T16:19:04Z	2016-04-29T00:00:00Z	62	MATA DA PRAIA	0	0	0	0	0	0	No
3	8.679510e+11	5642828	F	2016-04-29T17:29:31Z	2016-04-29T00:00:00Z	8	PONTAL DE CAMBURI	0	0	0	0	0	0	No
4	8.841190e+12	5642494	F	2016-04-29T16:07:23Z	2016-04-29T00:00:00Z	56	JARDIM DA PENHA	0	1	1	0	0	0	No
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
110522	2.572130e+12	5651768	F	2016-05-03T09:15:35Z	2016-06-07T00:00:00Z	56	MARIA ORTIZ	0	0	0	0	0	1	No
110523	3.596270e+12	5650093	F	2016-05-03T07:27:33Z	2016-06-07T00:00:00Z	51	MARIA ORTIZ	0	0	0	0	0	1	No
110524	1.557660e+13	5630692	F	2016-04-27T16:03:52Z	2016-06-07T00:00:00Z	21	MARIA ORTIZ	0	0	0	0	0	1	No
110525	9.213490e+13	5630323	F	2016-04-27T15:09:23Z	2016-06-07T00:00:00Z	38	MARIA ORTIZ	0	0	0	0	0	1	No
110526	3.775120e+14	5629448	F	2016-04-27T13:30:56Z	2016-06-07T00:00:00Z	54	MARIA ORTIZ	0	0	0	0	0	1	No
110527 rows × 14 columns

base_data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 110527 entries, 0 to 110526
Data columns (total 14 columns):
 #   Column          Non-Null Count   Dtype  
---  ------          --------------   -----  
 0   PatientId       110527 non-null  float64
 1   AppointmentID   110527 non-null  int64  
 2   Gender          110527 non-null  object 
 3   ScheduledDay    110527 non-null  object 
 4   AppointmentDay  110527 non-null  object 
 5   Age             110527 non-null  int64  
 6   Neighbourhood   110527 non-null  object 
 7   Scholarship     110527 non-null  int64  
 8   Hipertension    110527 non-null  int64  
 9   Diabetes        110527 non-null  int64  
 10  Alcoholism      110527 non-null  int64  
 11  Handcap         110527 non-null  int64  
 12  SMS_received    110527 non-null  int64  
 13  No-show         110527 non-null  object 
dtypes: float64(1), int64(8), object(5)
memory usage: 11.8+ MB
base_data.shape
(110527, 14)
#modifying the date and time into standard form
base_data['ScheduledDay'] = pd.to_datetime(base_data['ScheduledDay']).dt.date.astype('datetime64[ns]')
base_data['AppointmentDay'] = pd.to_datetime(base_data['AppointmentDay']).dt.date.astype('datetime64[ns]')
base_data.head(5)
PatientId	AppointmentID	Gender	ScheduledDay	AppointmentDay	Age	Neighbourhood	Scholarship	Hipertension	Diabetes	Alcoholism	Handcap	SMS_received	No-show
0	2.987250e+13	5642903	F	2016-04-29	2016-04-29	62	JARDIM DA PENHA	0	1	0	0	0	0	No
1	5.589980e+14	5642503	M	2016-04-29	2016-04-29	56	JARDIM DA PENHA	0	0	0	0	0	0	No
2	4.262960e+12	5642549	F	2016-04-29	2016-04-29	62	MATA DA PRAIA	0	0	0	0	0	0	No
3	8.679510e+11	5642828	F	2016-04-29	2016-04-29	8	PONTAL DE CAMBURI	0	0	0	0	0	0	No
4	8.841190e+12	5642494	F	2016-04-29	2016-04-29	56	JARDIM DA PENHA	0	1	1	0	0	0	No
for the schedule day and appointment day storing the weekdays only into a variable

# 5 is Saturday, 6 is Sunday 

base_data['sch_weekday'] = base_data['ScheduledDay'].dt.dayofweek
base_data['app_weekday'] = base_data['AppointmentDay'].dt.dayofweek
base_data['sch_weekday'].value_counts()
sch_weekday
1    26168
2    24262
0    23085
4    18915
3    18073
5       24
Name: count, dtype: int64
base_data['app_weekday'].value_counts()
app_weekday
2    25867
1    25640
0    22715
4    19019
3    17247
5       39
Name: count, dtype: int64
base_data.columns
Index(['PatientId', 'AppointmentID', 'Gender', 'ScheduledDay',
       'AppointmentDay', 'Age', 'Neighbourhood', 'Scholarship', 'Hipertension',
       'Diabetes', 'Alcoholism', 'Handcap', 'SMS_received', 'No-show',
       'sch_weekday', 'app_weekday'],
      dtype='object')
#changing the name of some cloumns
base_data= base_data.rename(columns={'Hipertension': 'Hypertension', 'Handcap': 'Handicap', 'SMS_received': 'SMSReceived', 'No-show': 'NoShow'})
base_data.columns
Index(['PatientId', 'AppointmentID', 'Gender', 'ScheduledDay',
       'AppointmentDay', 'Age', 'Neighbourhood', 'Scholarship', 'Hypertension',
       'Diabetes', 'Alcoholism', 'Handicap', 'SMSReceived', 'NoShow',
       'sch_weekday', 'app_weekday'],
      dtype='object')
base_data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 110527 entries, 0 to 110526
Data columns (total 16 columns):
 #   Column          Non-Null Count   Dtype         
---  ------          --------------   -----         
 0   PatientId       110527 non-null  float64       
 1   AppointmentID   110527 non-null  int64         
 2   Gender          110527 non-null  object        
 3   ScheduledDay    110527 non-null  datetime64[ns]
 4   AppointmentDay  110527 non-null  datetime64[ns]
 5   Age             110527 non-null  int64         
 6   Neighbourhood   110527 non-null  object        
 7   Scholarship     110527 non-null  int64         
 8   Hypertension    110527 non-null  int64         
 9   Diabetes        110527 non-null  int64         
 10  Alcoholism      110527 non-null  int64         
 11  Handicap        110527 non-null  int64         
 12  SMSReceived     110527 non-null  int64         
 13  NoShow          110527 non-null  object        
 14  sch_weekday     110527 non-null  int32         
 15  app_weekday     110527 non-null  int32         
dtypes: datetime64[ns](2), float64(1), int32(2), int64(8), object(3)
memory usage: 12.6+ MB
# dropping some columns which have no significance
base_data.drop(['PatientId', 'AppointmentID', 'Neighbourhood'], axis=1, inplace=True)
base_data
Gender	ScheduledDay	AppointmentDay	Age	Scholarship	Hypertension	Diabetes	Alcoholism	Handicap	SMSReceived	NoShow	sch_weekday	app_weekday
0	F	2016-04-29	2016-04-29	62	0	1	0	0	0	0	No	4	4
1	M	2016-04-29	2016-04-29	56	0	0	0	0	0	0	No	4	4
2	F	2016-04-29	2016-04-29	62	0	0	0	0	0	0	No	4	4
3	F	2016-04-29	2016-04-29	8	0	0	0	0	0	0	No	4	4
4	F	2016-04-29	2016-04-29	56	0	1	1	0	0	0	No	4	4
...	...	...	...	...	...	...	...	...	...	...	...	...	...
110522	F	2016-05-03	2016-06-07	56	0	0	0	0	0	1	No	1	1
110523	F	2016-05-03	2016-06-07	51	0	0	0	0	0	1	No	1	1
110524	F	2016-04-27	2016-06-07	21	0	0	0	0	0	1	No	2	1
110525	F	2016-04-27	2016-06-07	38	0	0	0	0	0	1	No	2	1
110526	F	2016-04-27	2016-06-07	54	0	0	0	0	0	1	No	2	1
110527 rows × 13 columns

base_data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 110527 entries, 0 to 110526
Data columns (total 13 columns):
 #   Column          Non-Null Count   Dtype         
---  ------          --------------   -----         
 0   Gender          110527 non-null  object        
 1   ScheduledDay    110527 non-null  datetime64[ns]
 2   AppointmentDay  110527 non-null  datetime64[ns]
 3   Age             110527 non-null  int64         
 4   Scholarship     110527 non-null  int64         
 5   Hypertension    110527 non-null  int64         
 6   Diabetes        110527 non-null  int64         
 7   Alcoholism      110527 non-null  int64         
 8   Handicap        110527 non-null  int64         
 9   SMSReceived     110527 non-null  int64         
 10  NoShow          110527 non-null  object        
 11  sch_weekday     110527 non-null  int32         
 12  app_weekday     110527 non-null  int32         
dtypes: datetime64[ns](2), int32(2), int64(7), object(2)
memory usage: 10.1+ MB
base_data.describe()
ScheduledDay	AppointmentDay	Age	Scholarship	Hypertension	Diabetes	Alcoholism	Handicap	SMSReceived	sch_weekday	app_weekday
count	110527	110527	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000
mean	2016-05-08 20:33:18.179630080	2016-05-19 00:57:50.008233472	37.088874	0.098266	0.197246	0.071865	0.030400	0.022248	0.321026	1.851955	1.858243
min	2015-11-10 00:00:00	2016-04-29 00:00:00	-1.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000
25%	2016-04-29 00:00:00	2016-05-09 00:00:00	18.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	1.000000	1.000000
50%	2016-05-10 00:00:00	2016-05-18 00:00:00	37.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	2.000000	2.000000
75%	2016-05-20 00:00:00	2016-05-31 00:00:00	55.000000	0.000000	0.000000	0.000000	0.000000	0.000000	1.000000	3.000000	3.000000
max	2016-06-08 00:00:00	2016-06-08 00:00:00	115.000000	1.000000	1.000000	1.000000	1.000000	4.000000	1.000000	5.000000	5.000000
std	NaN	NaN	23.110205	0.297675	0.397921	0.258265	0.171686	0.161543	0.466873	1.378520	1.371672
base_data['NoShow'].value_counts().plot(kind="bar",figsize=(5,6))
plt.xlabel("Count")
plt.ylabel("Target Variable")
plt.title("Count of TARGET Variable per category");
No description has been provided for this image
# calculating the % of appointments or not 
100*base_data['NoShow'].value_counts()/len(base_data['NoShow'])
NoShow
No     79.806744
Yes    20.193256
Name: count, dtype: float64
# Assuming 'data' is your DataFrame
# Calculate the percentage of missing values
missing = pd.DataFrame((base_data.isnull().sum()) * 100 / base_data.shape[0]).reset_index()
missing.columns = ['column', 'percentage']  # Rename columns for better readability

# Plotting the missing values
plt.figure(figsize=(16, 5))
ax = sns.pointplot(x='column', y='percentage', data=missing)
plt.xticks(rotation=90, fontsize=7)
plt.title("Percentage of Missing Values")
plt.ylabel("PERCENTAGE")
plt.show()
No description has been provided for this image
Missing Data - Initial Intuition Here, we don't have any missing data. General Thumb Rules:

For features with less missing values- can use regression to predict the missing values or fill with the mean of the values present, depending on the feature. For features with very high number of missing values- it is better to drop those columns as they give very less insight on analysis. As there's no thumb rule on what criteria do we delete the columns with high number of missing values, but generally you can delete the columns, if you have more than 30-40% of missing values.

Data Cleaning Create a copy of base data for manupulation & processing

new_data = base_data.copy()
new_data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 110527 entries, 0 to 110526
Data columns (total 13 columns):
 #   Column          Non-Null Count   Dtype         
---  ------          --------------   -----         
 0   Gender          110527 non-null  object        
 1   ScheduledDay    110527 non-null  datetime64[ns]
 2   AppointmentDay  110527 non-null  datetime64[ns]
 3   Age             110527 non-null  int64         
 4   Scholarship     110527 non-null  int64         
 5   Hypertension    110527 non-null  int64         
 6   Diabetes        110527 non-null  int64         
 7   Alcoholism      110527 non-null  int64         
 8   Handicap        110527 non-null  int64         
 9   SMSReceived     110527 non-null  int64         
 10  NoShow          110527 non-null  object        
 11  sch_weekday     110527 non-null  int32         
 12  app_weekday     110527 non-null  int32         
dtypes: datetime64[ns](2), int32(2), int64(7), object(2)
memory usage: 10.1+ MB
As we don't have any null records, there's no data cleaning required

# Get the max tenure
print(base_data['Age'].max()) #72
115
Data Exploration
  Cell In[36], line 1
    Data Exploration
         ^
SyntaxError: invalid syntax
list(base_data.columns)
['Gender',
 'ScheduledDay',
 'AppointmentDay',
 'Age',
 'Scholarship',
 'Hypertension',
 'Diabetes',
 'Alcoholism',
 'Handicap',
 'SMSReceived',
 'NoShow',
 'sch_weekday',
 'app_weekday']
#having a loook into the values of count of each columns and there count in respect to NoShow column
for i, predictor in enumerate(base_data.drop(columns=['NoShow'])):
    print('-'*10,predictor,'-'*10)
    print(base_data[predictor].value_counts())    
    plt.figure(i)
    sns.countplot(data=base_data, x=predictor, hue='NoShow')
---------- Gender ----------
Gender
F    71840
M    38687
Name: count, dtype: int64
---------- ScheduledDay ----------
ScheduledDay
2016-05-03    4238
2016-05-02    4216
2016-05-16    4120
2016-05-05    4095
2016-05-10    4024
              ... 
2016-04-16       1
2016-01-28       1
2015-11-10       1
2016-03-19       1
2016-03-05       1
Name: count, Length: 111, dtype: int64
---------- AppointmentDay ----------
AppointmentDay
2016-06-06    4692
2016-05-16    4613
2016-05-09    4520
2016-05-30    4514
2016-06-08    4479
2016-05-11    4474
2016-06-01    4464
2016-06-07    4416
2016-05-12    4394
2016-05-02    4376
2016-05-18    4373
2016-05-17    4372
2016-06-02    4310
2016-05-10    4308
2016-05-31    4279
2016-05-05    4273
2016-05-19    4270
2016-05-03    4256
2016-05-04    4168
2016-06-03    4090
2016-05-24    4009
2016-05-13    3987
2016-05-25    3909
2016-05-06    3879
2016-05-20    3828
2016-04-29    3235
2016-05-14      39
Name: count, dtype: int64
---------- Age ----------
Age
 0      3539
 1      2273
 52     1746
 49     1652
 53     1651
        ... 
 115       5
 100       4
 102       2
 99        1
-1         1
Name: count, Length: 104, dtype: int64
---------- Scholarship ----------
Scholarship
0    99666
1    10861
Name: count, dtype: int64
---------- Hypertension ----------
Hypertension
0    88726
1    21801
Name: count, dtype: int64
---------- Diabetes ----------
Diabetes
0    102584
1      7943
Name: count, dtype: int64
---------- Alcoholism ----------
Alcoholism
0    107167
1      3360
Name: count, dtype: int64
---------- Handicap ----------
Handicap
0    108286
1      2042
2       183
3        13
4         3
Name: count, dtype: int64
---------- SMSReceived ----------
SMSReceived
0    75045
1    35482
Name: count, dtype: int64
---------- sch_weekday ----------
sch_weekday
1    26168
2    24262
0    23085
4    18915
3    18073
5       24
Name: count, dtype: int64
---------- app_weekday ----------
app_weekday
2    25867
1    25640
0    22715
4    19019
3    17247
5       39
Name: count, dtype: int64
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
No description has been provided for this image
base_data['NoShow'] = np.where(base_data.NoShow == 'Yes',1,0)
base_data.NoShow.value_counts()
NoShow
0    88208
1    22319
Name: count, dtype: int64
base_data_dummies = pd.get_dummies(base_data)
base_data_dummies.head()
ScheduledDay	AppointmentDay	Age	Scholarship	Hypertension	Diabetes	Alcoholism	Handicap	SMSReceived	NoShow	sch_weekday	app_weekday	Gender_F	Gender_M
0	2016-04-29	2016-04-29	62	0	1	0	0	0	0	0	4	4	True	False
1	2016-04-29	2016-04-29	56	0	0	0	0	0	0	0	4	4	False	True
2	2016-04-29	2016-04-29	62	0	0	0	0	0	0	0	4	4	True	False
3	2016-04-29	2016-04-29	8	0	0	0	0	0	0	0	4	4	True	False
4	2016-04-29	2016-04-29	56	0	1	1	0	0	0	0	4	4	True	False
plt.figure(figsize=(20,8))
base_data_dummies.corr()['NoShow'].sort_values(ascending = False).plot(kind='bar')
<Axes: >
No description has been provided for this image
plt.figure(figsize=(12,12))
sns.heatmap(base_data_dummies.corr(),cmap="Paired")
<Axes: >
No description has been provided for this image
 
 
-------------------------------------------------------------------
