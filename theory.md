# SNOWFLAKE 

### Creating a Warehouse

  ```
  create or replace warehouse <warehouse name>
  with
  warehouse_size =  <XS|S|M|XL|XXL>
  max_cluster_count = <number>
  auto_suspend = <time in number>
  auto_resume = <TRUE | FALSE>
  comment = '<>'
```

### Creating File Format

```
  create or replace file format <format name>
  type = <CSV|JSON|PARQUET>
  field_delimiter = '<>'
  skip_header = '<number>'
  null_if = ('NULL','null')
  empty_field_as_null = <TRUE | FALSE>
  field_optionally_enclosed_by '"' // Based upon the situation
```

### Creating Stage

```
  create or replace stage <stage name>
  URL = ''
  storage_integration = ''
  file_format = <file format>
```

### Data Loading from s3

  1. Create AWS Account.
  2. Press ALT+S, type and search for s3
  3. Create bucket -> Specify Unique Name 
  4. Create Folders if needed -> Navigate to the folder -> Upload the files
  5. Press ALT+S, type and search for IAM -> Navigate to roles -> Create Role -> Different AWS Account -> Enable optional External ID and specify temporary external id
  6. Under add permission, search and check on AmazonS3FullAcess and click next, then specify the role name
  7. Click on newly created role, copy the arn
  8. In the snowflake worksheet, Create new database and scheme if needed.
  9. Create New storage integration object by
       ```
         create or replace storage integration <name>
         type = external_stage
         storage_provider = s3
         enabled = true
         storage_aws_role_arn = '<Paste the ARN>'
         storage_allowed_locations = ('s3://<bucket name>/path')
         comment = '<comment>'
       ```
  10. Once Created,
    ```
    DESC storage integration <name>
    ```
  Copy the the STORAGE_AWS_IAM_USER_ARN and STORAGE_AWS_EXTERNAL_ID.
  11. Navigate to the AWS console, ALT+S, search for the IAM -> Roles -> Search for the created role, Click trust policy -> edit trust policy -> Paste the ARN and External ID in the respective fields and click update trust policy
  12. Navigate back to Snowflake, Create a Stage, then use Copy option to copy the data into the table.
