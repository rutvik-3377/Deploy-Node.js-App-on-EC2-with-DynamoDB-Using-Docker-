# Deploy-Node.js-App-on-EC2-with-DynamoDB-Using-Docker-

**Step 1️⃣ - Launch EC2 Instance**
Go to AWS EC2 Console.

Launch a new EC2 instance (Amazon Linux 2 or Ubuntu recommended).

Allow Inbound Rules for:

- HTTP (80)
- SSH (22)

Connect to the EC2 instance via SSH.

**Step 2️⃣ - Install Docker on EC2**
For Amazon Linux 2:

Edit
```
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

```

Log out and log back in if needed for group changes to apply.

Pull Docker Image:
```
docker pull philippaul/node-dynamodb-demo
```

**Step 3️⃣ - Create IAM User for DynamoDB Access**
Go to IAM Console → Users → Create User.

Assign these Policies:

- AmazonEC2FullAccess

- AmazonDynamoDBFullAccess

Generate Access Key ID and Secret Access Key.

Store these keys securely.


**Step 4️⃣ - Run Docker Container on EC2 with AWS Credentials**
Run the following Docker command (replace placeholders):
```
docker run --rm -d -p 80:3000 \
--name node-dynamo-app \
-e AWS_REGION=your-region \
-e AWS_ACCESS_KEY_ID=your-access-key \
-e AWS_SECRET_ACCESS_KEY=your-secret-key \
philippaul/node-dynamodb-demo
```

Example for Mumbai (ap-south-1):
```
-e AWS_REGION=ap-south-1
```

**Step 5️⃣ - Create DynamoDB Table**
Go to DynamoDB Console.

Create a new Table:

- Table Name: (Match what your app expects, e.g., users)

- Primary Key: As per your app schema (e.g., id)

Region must match your EC2 app config (e.g., ap-south-1).

**Step 6️⃣ - Verify the Application**
- Visit your EC2 Public IP on port 80 in the browser.

- Example: http://your-ec2-public-ip

- If DynamoDB is connected properly, the app will work. If not, check logs:
  ```
  docker logs node-dynamo-app
  ```

**🎯 Final Structure of Your Project
**
<img width="744" height="339" alt="image" src="https://github.com/user-attachments/assets/f15ae9c4-5139-4e3b-b465-f74d9fc652ee" />



