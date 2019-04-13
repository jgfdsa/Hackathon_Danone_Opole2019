
# Danone HACKATHON technical instructions

The big day is here, and its time to code, but first some instructions to define the rules of the game and make the event as fair as possible and to give the same opportunity to everyone to win.

The data have been explained already. At first glance, this challenge looks like a good candidate for Regression-based algorithms. 

We have prepared for you the historical data for two of the recipes. All the manufacturing data (sensor data) is available to you tagged through the timestamp and for each Drum Dryer line (*line_dd*). You also have the information about each production order that was executed since 2018-01-01. It is up to you how you decide to aggregate this data (pre-processing).

The data is quite clean, but as in any other real-world data processing, it is a good strategy to perform an exploratory data analysis. This EDA is a good way of consolidating your learnings on the business case, and a good baseline to discuss with your colleagues or request for support to the mentors.

Before going to the specifics for this hackathon, some general remarks:
- During the hackathon you will have available a VM with Ubuntu 16, ODBC drivers installed and anaconda with a ready environment for data processing.
- You can install as many tools/libraries as needed for your developments.
- Al the data is available in an MS-SQL server. You should have received your credential together with the access to the VM.
- In this repository you will find, in the notebooks folder, ready notebooks you can directly re-use to extract the data or feel free to copy the part of the code that interests you.
- It is strongly advised you create a simple script to store your credentials. And then, load them from your scripts.

# Data split

For each recipe, you have a table *orders_details*. The column [data_split] give you the information to split the data between training and test.

# Submission instructions

During the first 12 hours of the hackathon, you will have the opportunity to submit the results from evaluating your algorithm in the test dataset.

There are 3 basic measurements that are needed:
- Accuracy (based on a specially designed class),
- [Root-mean-squared deviation](https://en.wikipedia.org/wiki/Root-mean-square_deviation) and the,
- [coefficient of determination or "R squared"](https://en.wikipedia.org/wiki/Coefficient_of_determination)

There is a dashboard created that will display how well all of you are doing. The dashboard displays the information for each of the recipes and a table that shows the metrics. To submit your results, please insert a new row into the [dbo].[results_test_dataset] as follow:

```sql
USE [hackathon_danone]
GO

INSERT INTO [dbo].[results_test_dataset]
           ([recipe]
           ,[team_name]
           ,[team_id]
           ,[github_repository_url]
           ,[commit_hash]
           ,[accuracy]
           ,[RMSE]
           ,[R2]
     VALUES
           (<recipe, bigint,> -- it has to be 10312361 OR 10312369
           ,<team_name, nvarchar(250),>
           ,<team_id, nvarchar(50),>
           ,<github_repository_url, nvarchar(max),>
           ,<commit_hash, nvarchar(40),>
           ,<accuracy, float,>
           ,<RMSE, float,>
           ,<R2, float,>
GO
```
**For the commit_hash**, after commiting your model, you need to run: `git show -s --format=%H`. You will get a 40 characters string similar to: `124a9a0ee1d8f1e15e833aff432fbb3b02632105`

In the last hours of the hackathon, you will receive new Process orders for each recipe. In this case, you are required to submit the results from your model, and not the evaluation as for the test dataset. We will calculate the accuracy, RMSE and R-squared for you. The instruction for this will be provided at the time of releasing the first validation dataset.


# Special class label - Evaluation of the Production order outputs.

During the elaboration of baby-food, the food-engineers have defined a metric that is called as "efficiency". As you can see from the "out_semi_finished_production" tables, this measurement fluctuates around 100, which represents the optimal condition of the product after the elaboration of the recipe.
This class label is a simple as:
- Efficiencies below 98 are mapped as _"below"_
- Efficiencies between 98 and 110 (inclusive) are mapped as _"optimal"_
- Efficiencies above 110 are mapped as _"above"_

While having a model capable of estimating the efficiency (numeric) given the input raw materials and machine sensor data is very valuable to improve the elaboration of baby-food, it represents a big challenge that might not be possible to solve during a 24h hackathon.

In this sense, you are required to calculated the accuracy of your model. Below is a simple example for a given Production order:
```python
import numpy as np
from sklearn.metrics import accuracy_score

y_pred = ['optimal','optimal','below','above'] # estimated from your model
y_true = ['optimal','below','below','above'] # obtained from the data

accuracy_score(y_true, y_pred, normalize=True)
```

# Bulk density and moisture estimation

For this task, your goal is to estimate the bulk density and moisture. These two output variables are, for each recipe,  in the *out_test_during_production* table.

To evaluate how well your model is performing we will be using multioutput aggregation available in [*sklearn.metrics*](https://scikit-learn.org/stable/modules/model_evaluation.html). Using the sklearn examples:

### [Root Mean Squared Error](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html#sklearn.metrics.mean_squared_error)
```python
y_true = [[0.5, 1],[-1, 1],[7, -6]]
y_pred = [[0, 2],[-1, 2],[8, -5]]
np.sqrt(metrics.mean_squared_error(y_true, y_pred, multioutput='uniform_average'))
```
### [R-squared](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.r2_score.html#sklearn.metrics.r2_score)
```python
from sklearn.metrics import r2_score
y_true = [[0.5, 1],[-1, 1],[7, -6]]
y_pred = [[0, 2],[-1, 2],[8, -5]]
r2_score(y_true, y_pred, multioutput='uniform_average') 
```

# Connectivity

You should be able to connect to an Ubuntu virtual machine. Using _sqlalchemy_ and/or _pyodbc_ you should be able to connect to the DB and gather the data.

Some examples of code to connect from python:

```python
# using SQLalchemy 
from sqlalchemy import create_engine
import pandas as pd

database = 'hackathon_danone'
username = ''
password = ''
driver= '/opt/microsoft/msodbcsql17/lib64/libmsodbcsql-17.3.so.1.1'
server = '192.168.250.3'

my_engine = create_engine(f'mssql+pyodbc://{username}:{password}@192.168.250.3:1433/{database}?driver={driver}', fast_executemany=True)

def db_sql(sql):
    cnxn = pyodbc.connect(f'DRIVER={driver};PORT=1433;SERVER={server};DATABASE={database};UID={username};PWD={password}')
    data = pd.read_sql(sql,cnxn)
    cnxn.close()    
    return data
```

# ANNOUCEMENT - 2019-04-12 20:50

- In the slurry, 66% of water content stands for: 380 liters for recipe 0 (10312361)
- For the recipe 1 (10312369) 66% of water content stands for: 420 liters.
- A correction was made in the sqlalchemy engine

# ANNOUCEMENT - Evaluation insert script

```python
USE [hackathon_danone]
GO

INSERT INTO [dbo].[results_validation_dataset]
           ([recipe]
           ,[team_name]
           ,[team_id]
           ,[github_repository_url]
           ,[commit_hash]
           ,[orders_details_id]
           ,[bigbad_id]
           ,[efficiency]
           ,[moisture]
           ,[bulk_density]
     VALUES
           (<recipe, bigint,>
           ,<team_name, nvarchar(250),>
           ,<team_id, nvarchar(50),>
           ,<github_repository_url, nvarchar(max),>
           ,<commit_hash, nvarchar(40),>
           ,<orders_details_id, int,>
           ,<bigbad_id, float,>
           ,<efficiency, float,>
           ,<moisture, float,>
           ,<bulk_density, float,>
GO
```

