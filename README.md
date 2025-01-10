# Enhancing E-Commerce Agility With Advanced ETL Pipeline

## Overview
This project implements an end-to-end automated ETL pipeline for an e-commerce company. The pipeline handles data uploads from the Order and Returns teams, performs a join operation using AWS Glue and PySpark, stores the results in Amazon Redshift, and sends notifications about the pipeline's status using Amazon SNS. The entire workflow is orchestrated using AWS Step Functions.

## Features
- **File Upload:** Secure file upload via a Streamlit web application.
- **AWS S3 Integration:** Files are stored in designated S3 buckets for Orders and Returns teams.
- **ETL Process:** Data is processed using AWS Glue and PySpark.
- **Data Storage:** Processed data is stored in an Amazon Redshift data warehouse.
- **Notifications:** Success or failure notifications are sent via AWS SNS.
- **Workflow Orchestration:** AWS Step Functions monitor and manage the pipeline.

## Technologies Used
- **Streamlit**: For user-friendly file uploads.
- **AWS S3**: To store uploaded files.
- **AWS Lambda**: To trigger Glue jobs upon file uploads.
- **AWS Glue**: To perform ETL operations.
- **PySpark**: For data transformation and joining.
- **Amazon Redshift**: To store the final dataset.
- **AWS SNS**: To send pipeline execution status notifications.
- **AWS Step Functions**: To orchestrate the workflow.

## Architecture
![Architecture Diagram](path-to-your-image.png)

## Setup Instructions

### 1. Prerequisites
- AWS Account
- IAM roles with the required permissions
- Python 3.7 or higher
- Streamlit installed (`pip install streamlit`)
- AWS CLI configured with credentials

### 2. AWS Resource Setup

#### S3 Buckets
- Create two buckets:
  - `ecommerce-orders`
  - `ecommerce-returns`

#### Amazon Redshift
- Create a Redshift cluster with a table:
  ```sql
  CREATE TABLE public.joined_data (
      order_id VARCHAR(50),
      product_name VARCHAR(100),
      return_status VARCHAR(50),
      order_date DATE
  );
  ```

#### SNS Topics
- Create two topics:
  - `pipeline-success`
  - `pipeline-failure`

#### IAM Roles
- Grant permissions for S3, Glue, Redshift, Lambda, and SNS.
- Example IAM policy for Glue:
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": ["s3:GetObject", "redshift:CopyClusterSnapshot", "sns:Publish"],
        "Resource": "*"
      }
    ]
  }
  ```

#### AWS Glue Job
- Create a Glue job named `ecommerce-etl-job`.
- Upload the provided PySpark script.

#### AWS Step Functions
- Configure the workflow using the provided state machine definition.

### 3. Run the Streamlit App
1. Install required Python libraries:
   ```bash
   pip install boto3 streamlit
   ```
2. Run the Streamlit app:
   ```bash
   streamlit run app.py
   ```
3. Upload Order and Returns files via the web interface.

### 4. Lambda Function
- Deploy the Lambda function to trigger the Glue job upon file upload to S3.

### 5. Test the Pipeline
1. Upload files through Streamlit.
2. Verify S3 triggers the Lambda function.
3. Check data processing in Glue and storage in Redshift.
4. Confirm notifications in SNS.

## Project Structure
```
.
├── app.py                 # Streamlit application
├── lambda_function.py     # Lambda function script
├── glue_etl_script.py     # PySpark script for Glue
├── state_machine.json     # Step Functions state machine definition
├── README.md              # Project documentation
```

## How It Works
1. **File Upload:** The Orders and Returns teams upload data files via Streamlit.
2. **S3 Storage:** Files are securely stored in respective S3 buckets.
3. **Lambda Trigger:** An S3 event triggers the Lambda function, which starts the Glue job.
4. **Glue ETL:** Glue reads, transforms, and joins the datasets, and writes the result to Redshift.
5. **Step Functions:** The workflow tracks execution and sends success or failure notifications via SNS.
6. **Streamlit Dashboard:** Displays the pipeline status based on SNS messages.

## Future Enhancements
- Implement data validation checks before processing.
- Add detailed monitoring and alerting mechanisms.
- Explore using Databricks for ETL as an alternative to Glue.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---

For any queries or feedback, feel free to reach out!

