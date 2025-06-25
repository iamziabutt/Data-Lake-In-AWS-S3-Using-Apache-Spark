## Datalake in AWS S3 with Apache Spark

This project builds an ETL pipeline that extracts songs data from aws s3, processes that data within Spark SQL and loads the data back to S3 as a set of persistent tables.This will allow the analytical team to understand which songs users are listening to.

---

⭐ **Project Dependencies**: 

- ♻️  Python 3.13.5
- ♻️  Spark 3.5.6 (built with hadoop 3.3.4, winutils 3.3.6)
- ♻️  Java 8.0.202.5
- ♻️  boto3
- ♻️  hadoop-aws-3.3.4.jar
- ♻️  aws-java-sdk-bundle-1.12.262.jar
- ♻️  AWS S3 buckets (input_bucket and output_bucket)
- ♻️  A `version compatibility` diagram is below:


![Reference Image](/resources/version_compatibility.png)



---
⭐ **Project architecture**: 

![Reference Image](/resources/flowchart.png)

---

⭐ **Key features**: 

- 🌿 we will be using `s3a://` protocol for direct access and better performance
- 🌿 Hadoop AWS Configuration (through jars) for authetication
- 🌿 Parquet format for processed data for optimal storage/performance
- 🌿 Ensuring IAM user has: `s3:GetObject`, `s3:PutObject`, `s3:ListBucket` permissions
- 🌿 Parquet format for processed data for optimal storage/performance
- 🌿 Uses botot3 exclusively for file listing in s3 bucket. This means we maintain `separation of concerns` that is `Boto3` for metadata operations and `spark` for data operations
- 🌿 Uses below path structure to extract the input data:

![Reference Image](/resources/input_bucket_path_structure.png)

---

⭐ **Setup instructions**: 

- ▶️ Replace Access_key and Secret_key with your own keys
- ▶️ Specify Input and output bucket paths
- ▶️ Using config.ini as configuration file to store credentials and paths
- ▶️ Verify AWS credentials have S3 read permissions 
- ▶️ Confirm file exists in S3 (using boto3 or AWS CLI)
- ▶️ Include `hadoop-aws` JAR matching your Hadoop version
- ▶️ Ensure Bucket `region` consistency
- ▶️ Both jars are stored in `spark/jars` directory and added to the `system variables` and to the `system path` 




---

⭐ **Output managment**: 

- 🔍 Data will be written in Parquet format
- 🔍 Overwrite mode that replaces the data with the refreshed data. Used append mode if you like to add incremental data
- 🔍 Output Path: Combines your bucket `(s3a://spark-datalake-bucket/)` and prefix `(processed-data/)` into a full path, with `songs_table/` as the final directory.

- 🔍 **Write configuration:**

   - `.mode("overwrite")`: Replaces existing data. Use "append" to add data instead.
   - `.format("parquet")`: Writes in Parquet format (recommended for datalakes). Replace with "csv", "json", etc., if needed.
   - `.option("path", output_path)`: Specifies the S3 destination.
   - `.save()`: Executes the write operation.










