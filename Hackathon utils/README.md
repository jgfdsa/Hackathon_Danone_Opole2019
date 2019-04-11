# Submission instructions

During the first 12 hours of the hackathon, you will have the opportunity to submit your results from evaluating your algorithm in the test dataset.
There are 3 basic measurements that are needed: Accuracy (based on a specially designed class), [Root-mean-squared deviation](https://en.wikipedia.org/wiki/Root-mean-square_deviation) and the [coefficient of determination or "R squared"](https://en.wikipedia.org/wiki/Coefficient_of_determination)
There is a dashboard created that will display how well all of you are doing. The dashboard displays the info for each of the recipes and a table that shows the metrics. To submit your results, please insert a new row into the [dbo].[results_test_dataset] as follow:

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
           ,[created_date]
           ,[last_modified])
     VALUES
           (<recipe, bigint,>
           ,<team_name, nvarchar(250),>
           ,<team_id, nvarchar(50),>
           ,<github_repository_url, nvarchar(max),>
           ,<commit_hash, nvarchar(40),>
           ,<accuracy, float,>
           ,<RMSE, float,>
           ,<R2, float,>
GO
```
In the last hours of the hackathon, you will receive new processing orders for each recipe. In this case, you are required to submit the results from your model, and not your evaluation as for the test dataset.
We will calculate the accuracy, RMSE and R-squared for you.
