To launch and configure an Amazon EC2 instance, follow this step-by-step procedure using the AWS Management Console.

---

1. **Navigate to the EC2 Dashboard:** Prerequisite: Active AWS Account.
Log in to the **AWS Management Console**. In the search bar at the top, type **EC2** and select **EC2** from the services list. In the top-right corner, ensure you have selected your desired AWS Region.

*Verification:* You should see the main EC2 Dashboard displaying resources like running instances, security groups, and key pairs.


2. **Start the Instance Launch Wizard:** Dashboard Action.
On the EC2 Dashboard, click the orange **Launch instance** button.

*Verification:* The "Launch an instance" configuration wizard page opens.


3. **Set Instance Name and Choose OS Image (AMI):** Basic Details.
1. Under **Name and tags**, enter a name for your instance (e.g., `MyWebServer`).
2. Under **Application and OS Images (Amazon Machine Image)**, select your operating system.
* *Recommended for beginners:* Choose **Amazon Linux 2023** or **Ubuntu Server 24.04 LTS** (ensure the **Free tier eligible** badge is visible if using a free account).



*Verification:* The selected AMI box shows a checkmark next to your chosen operating system.


4. **Select Instance Type:** Hardware Sizing.
Under **Instance type**, open the dropdown menu.

* Choose **`t2.micro`** or **`t3.micro`** (both are Free tier eligible depending on your region).

*Verification:* The instance type field displays `t2.micro` or `t3.micro`.


5. **Create or Assign a Key Pair:** Access & Security.
Under **Key pair (login)**:

1. Click **Create new key pair**.
2. Enter a **Key pair name** (e.g., `my-ec2-key`).
3. Choose **RSA** as the key pair type.
4. Select **`.pem`** for OpenSSH/macOS/Linux terminal or **`.ppk`** for PuTTY on Windows.
5. Click **Create key pair**.

*Verification:* A file (e.g., `my-ec2-key.pem`) automatically downloads to your local machine. Store this safely; AWS will not let you download it again.


6. **Configure Network & Security Group:** Firewall Settings.
Under **Network settings**, click **Edit**:

1. Keep default **VPC** and **Subnet** settings.
2. Ensure **Auto-assign Public IP** is set to **Enable** so your instance gets an internet IP.
3. Select **Create security group**.
4. Check the box for **Allow SSH traffic from** and set it to **My IP** for maximum security.
5. *(Optional)* Check **Allow HTTP traffic from the internet** if hosting a web app.

*Verification:* The network rules list shows Port 22 (SSH) restricted to your IP address.


7. **Configure Storage & Launch:** Finalize.
1. Under **Configure storage**, keep the default allocation (8 GiB for Linux or 30 GiB for Windows; gp3 volume type).
2. On the right-side summary panel, click **Launch instance**.

*Verification:* A success banner displays with your Instance ID (e.g., `i-0abcd1234efgh5678`). Click the Instance ID to view your instance status changing from `pending` to `running`.


---

---

To connect an Amazon S3 bucket to an AWS Lambda function so that uploading a file automatically triggers your code, follow this step-by-step procedure using the AWS Management Console.

*Important: Create both the S3 bucket and the Lambda function in the same AWS Region.*

---

1. **Create the S3 Bucket:** Step 1.
1. Log in to the AWS Management Console, search for **S3**, and click on the service.
2. Click **Create bucket**.
3. Enter a **Bucket name** (must be globally unique across all AWS accounts).
4. Keep all other default settings (such as keeping public access blocked) and scroll to the bottom.
5. Click **Create bucket**.

*Verification:* Your new bucket should appear in the S3 bucket list.


2. **Create the IAM Policy & Role for Lambda:** Step 2.
1. Open the **IAM** service in the console.
2. Go to **Roles** in the left menu and click **Create role**.
3. Select **AWS service** as the trusted entity type and choose **Lambda** as the use case.
4. On the permissions page, search for and select the managed policy **`AWSLambdaBasicExecutionRole`** (allows logging to CloudWatch).
5. *(Optional)* If your function needs to read or download files from the S3 bucket, search for and attach **`AmazonS3ReadOnlyAccess`**.
6. Click **Next**, name the role (e.g., `LambdaS3ExecutionRole`), and click **Create role**.

*Verification:* Search for `LambdaS3ExecutionRole` under IAM Roles to confirm it exists.


3. **Create the Lambda Function:** Step 3.
1. Navigate to the **AWS Lambda** console and click **Create function**.
2. Choose **Author from scratch**.
3. Enter a **Function name** (e.g., `ProcessS3Uploads`).
4. Select your preferred **Runtime** (e.g., *Python 3.12* or *Node.js 20.x*).
5. Expand **Change default execution role**.
6. Choose **Use an existing role** and select the `LambdaS3ExecutionRole` you created in Step 2.
7. Click **Create function**.

*Verification:* You are taken to the function overview page showing your new Lambda function.


4. **Connect S3 to Lambda (Add Event Trigger):** Step 4.
1. On your Lambda function detail page, look at the **Function overview** section at the top.
2. Click the **+ Add trigger** button.
3. In the **Trigger configuration** dropdown, select **S3**.
4. Under **Bucket**, select the S3 bucket you created in Step 1.
5. Under **Event type**, select **All object create events** (or specific ones like `s3:ObjectCreated:Put`).
6. Check the acknowledgment box regarding recursive invocations and click **Add**.

*Verification:* AWS automatically configures the resource-based policy allowing S3 to invoke Lambda, and "S3" now appears as a connected source in the Lambda Function Overview diagram.


5. **Test the Integration:** Step 5.
1. Open your Lambda function's **Code** tab and add a simple print/log statement to verify execution:
* **Python Example:**



```python
def lambda_handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        print(f"File uploaded: {key} to bucket: {bucket}")
    return {'statusCode': 200}

```

2. Click **Deploy** to save your code.
3. Go to the **S3 console**, open your bucket, and click **Upload** to add any sample file.
4. Return to your Lambda function, click the **Monitor** tab, and click **View CloudWatch logs**.

*Verification:* In CloudWatch Log Streams, open the latest stream to confirm your print log output displays details about the uploaded file.


---

---
