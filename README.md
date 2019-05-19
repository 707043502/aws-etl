# ETL For Sparkify V2.0.0

## 1.INTRODUCTION

🏡Sparkify has been collecting on songs and user activity on their new music streaming. 🙏In order to support further data analysis which is really important for such online bussiness, we launched 👨‍👦`ETL For Sparkify v1.0.2`. However with the growth of our market😁, it becomes hard🤣 to maintain the ETL process on a single node any more, and we are urged to move this service to AWS. Here comes our new project of 👨‍👨‍👦‍👦 `ETL For Sparkify v2.0.0`

## 2.DATA ORGANIZATION
Moving our service to the cloud offers a lot of benifits🎅. Since Amazon Redshift offers fast, scalabe cost effictive dataware hourse services✈, it is reasonable we choose it as our ☁cloud service provider.


**THINGS UNCHANGED:**
We keep using the star schema as it brings convinience to our data analysis team👌
Tables are organized as:
- FACTS: songplays
- DIMENSION: users, songs, artists, time


The following picture shows how we organize our tables.

<div align=center>
<img src="https://github.com/707043502/aws-etl/blob/master/pic/schema.png" width="150" height="200">
</div>


**DECISIONS WE MAKE:** We develop 2 strategies to accommondate our growth of data for different tables

`TABLE songplays`: This table records user transactions is most likely to grow super fact🙈 in the future, so we sort, split then distribute the segments to different nodes based on songplay_id.

`TABLE users, songs, artists, time`: These dimension table are frequently refered by analysis team, so we copy them to all our nodes for 🚄faster access.





## 3.PRACTICE
Users can run the scripts follow steps bellow😋

`Step0`: **configure your AWS**
- a lot of things to do.

`Step1`: **data preparation**
- Copy data from S3 storage below and store them to our staging table
    - Song data: ` s3://udacity-dend/song_data` to table `staing songs`
    - Log data: `s3://udacity-dend/log_data` to table `staging events`
    
`Step2`: **prepare database env**
- create tables using<br>     &emsp;&emsp;```python create_tables.py```

`Step3`: **start ETL**
- using<br> &emsp;&emsp;```python etl.py```
    
## 4.FILEs 
In case any customization,we post out our file organization strategies for referencing.👨

```create_tables.py```: process to prepare database

```sql_queries.py```: basic queries used in this project, including:💀
- create table
- queries of updating 
- queries of inserting
    - Redshift DO NOT force UNIQUE constrains for us, we had a hard time tuning our query to make our unique field distinct.
    - And we are not sure if we did it right.
- drop table

```etl.py```: main logic that maintain the ETL process

- parse json logs to update table `staging songs` and `staging events`

- use data from staging table to build data analysis tables of  star schema 

```dwh.cfg```: config files provides configuration of cloud environment


