
# Project Title

ETL : Incremental data load from local system to AWS S3 to AWS Redshift using AWS Glue.


## Prerequisite

AWS Account

Data

## Documentation

Incremental loading involves extracting and loading only the data that has changed since the last ETL cycle.


Flow architecture of this process given below.

![alt text](<Blank diagram-1.jpeg>)
Process 

Goal-We are doing icremental load of our data from AWS S3 -> Glue-> to AWS Redshift.

Steps - 1. We uploading our local data(exel file) to AWS S3.

Step - 2 : Create a IAM role with policies -> AmazonS3FullAccess,AWSLambdaExcute,CloudWatchfullAccess,AdmistratorAccess with trust relationshp waith redshift and glue services. 

Step - 3 : Open AWS redshif and create workgroup and namespace.(It will create redshift cluster.)Once workgroup created click on newly created workgroup VPC(open in new tab).
Check subnets, route tables, network and security.
In network and security -> edit Enhanced VPC routing= ON and public accessiblity =ON .


Click on Endpoint ->(create S3 endpoint-> endpoint type -> Gateway)

Click on securty grop -> check inbound rules.

step 4 : ( creating destination table in redshift). click on query editor. in query editor tab -> Serverless workspace->Dev->public->table.Now create a table( keep in mind all the column name and data type should be same as the data you want to upload.)

Now Our source and destination ready. We have to now work with connection between them.

step 5 : Go to AWS Glue. Click on crawlers -> create new crawler( add data source S3 -> crawl all subfolders ->chosse IAM role created above )-> next -> create new database/choose existing database -> select on demand ->create crawler.

crawler created now create connection between services.

Step 6 : on same AWS Glue conssole, Click on connections ->chosse redsfift -> choose cluster(already created redshif above)-. fill required fields -> create connection. After connection created click on Action -> test connection.(rename the job)

Step 7 : click on ETL jobs.click on Visual etl -> source- s3 ->targer - redsift.
Click on S3 diagram -> choose data catalog table ->chosse table -> put IAM role created above. It will automatacally take data from s3.
click on target diagram ->choose s3 ->chosse redshift connection->table ->Merge data to target table ->run the job.
After job run successfully go to query editor and do all select query to table . you will se data in redshift now .

Step 8 : do modification in you data in you system (add/delete) and upload in s3. Run the crawler again. After success run go to query editor and do select all query, you can see new data added/modified.

Thats all now you successfully created ETL JOB.
## Screenshots

Screenshoot of query editor 1st time ETL job run successfully.

![alt text](<Screenshot (6).png>)


Screenshoot of query deitor after doing Incremental data load.


![alt text](<Screenshot (7).png>)