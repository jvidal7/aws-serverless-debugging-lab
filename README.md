<img width="1919" height="885" alt="image" src="https://github.com/user-attachments/assets/c5f0bce1-e8d6-4515-a2b2-cd68fd9b9935" /># aws-serverless-debugging-lab
Debugging and restoring a broken AWS serverless contact form workflow using API Gateway, Lambda, DynamoDB, SNS, CloudWatch, and IAM. This project focuses on troubleshooting distributed cloud services, resolving permission issues, analyzing logs, and restoring production functionality.

# Overview of Project

## Scenario

A media company recently reported a critical issue in their production environment. Their contact form, previously working fine, has suddenly stopped sending email notifications and storing messages in their database. This contact form is part of a serverless architecture using:

- Amazon API Gateway  
- AWS Lambda  
- Amazon DynamoDB  
- Amazon SNS  

Since the issue started, the company has missed several important client messages, risking customer trust and reputation.

---

## Our Solution

We’ll take the role of a **Cloud Support Engineer** and investigate the broken serverless workflow.

Using AWS tools such as:

- Amazon CloudWatch  
- AWS IAM  
- AWS Console  

We will diagnose the issue by tracing the flow of data and identifying where the failure occurs. After identifying the root cause, we’ll apply fixes to restore full functionality, ensuring that:

- Form submissions are successfully stored in DynamoDB  
- Email notifications are properly sent through SNS  

---

## About Project

As a Cloud Support Engineer, my responsibility is to identify and resolve issues in production serverless workflows.

In this project, I simulate a real-world support ticket by investigating a broken contact form integration. I’ll:

- Explore CloudWatch logs for Lambda functions
- Verify API Gateway configuration
- Review IAM permissions and policies
- Check DynamoDB table functionality
- Validate SNS subscription health

---

# 👩‍💻 Steps to be Performed
In the next few lessons, I will complete the following tasks:

1. Deploy the Broken CloudFormation Stack  
2. Reproduce the Contact Form Issue  
3. Investigate the Lambda Logs  
4. Trace and Verify the Full Architecture  
5. Apply Fixes to Restore Functionality  
6. Write a Professional Support Response  

---

# 🛠 Services Used
| AWS Service | Purpose |
|---|---|
| Amazon API Gateway | Exposes the contact form as an HTTP endpoint |
| AWS Lambda | Handles incoming form submissions |
| Amazon DynamoDB | Stores user-submitted form data |
| Amazon SNS | Sends email notifications to the support team |
| Amazon CloudWatch | Monitors Lambda execution and logs errors |
| AWS IAM | Controls permissions between AWS services |

---

# ➡️ Diagram
This is the architectural diagram for the project.

### Screenshot
![AWS Serverless Architecture Diagram](images/aws-serverless-architecture.gif)

---

# ✅ Final Result

A fully functional serverless contact form workflow that demonstrates:

- Smooth integration between API Gateway, Lambda, DynamoDB, and SNS  
- End-to-end debugging using CloudWatch Logs  
- Fixing permission issues and SNS subscription confirmation problems  
- Writing a professional support response documenting the root cause and resolution  

This project gave me hands-on troubleshooting experience across multiple AWS services — essential knowledge for any Cloud Support Engineer working in a production support role.

---

# ☁️ 4.2 Deploying the Broken CloudFormation Stack

## Introduction

In this section, I will deploy a CloudFormation stack containing intentionally broken serverless components. This setup simulates a real-world troubleshooting scenario where a production contact form workflow is failing.

The stack will create the following AWS resources:

- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB
- Amazon SNS
- AWS IAM Roles

The environment is intentionally misconfigured so we can investigate and resolve the issues throughout the lab.

---

# 👩‍💻 Step 1: Creating the CloudFormation Stack

### 1. Download the CloudFormation Template

I would download the template file:

```bash
Broken-contact-form.yml
```

### Screenshot

![CloudFormation Template Download](images/template-download.png)

---

### 2. Navigate to AWS CloudFormation

1. I will open the AWS Console
2. Search for **CloudFormation**
3. Open the CloudFormation service dashboard

### Screenshot
![AWS CloudFormation Console](images/cloudformation-console.png)

---

### 3. Create the Stack

1. Click **Create stack**
2. Select **Choose an existing template**
3. Upload the YAML template file
4. Click **Next**

### Screenshot

![Create CloudFormation Stack](images/create-stack.png)

---

# ⚙️ Step 2: Specify Stack Details

### Configure Stack Name

I would enter the following stack name:

```bash
broken-contact-form
```

### Configure Parameters

Under the **EmailAddress** parameter:

- I Replaced the default value with my personal email address

```bash
jorgev192@gmail.com
```

This email address will later receive SNS notifications from the contact form system. 

### Screenshot

![Specify Stack Details](images/specify-stack-details.png)

---

# Step 3: Configure Stack Options

### Review Stack Configuration

1. I would leave all default settings unchanged
2. Scroll to the **Capabilities** section
3. Check the acknowledgment box:

```text
I acknowledge that AWS CloudFormation might create IAM resources with custom names
```

### Why This Matters

The CloudFormation template creates IAM roles that allow Lambda to interact with other AWS services. However, the permissions are intentionally incomplete and will later require troubleshooting.

### Screenshot

![CloudFormation IAM Capabilities](images/iam-capabilities.png)

---

# Step 4: Review and Create Stack

### Deploy the Stack

1. Review all stack configuration details
2. Click **Submit**
3. Wait for deployment to begin

### Screenshot

```md
![Review and Submit Stack](images/review-submit-stack.png)
```

---

# Understanding the CloudFormation Template

The YAML template defines a complete serverless contact form architecture.

### Resources Created

| Resource | Purpose |
|---|---|
| API Gateway | Receives HTTP form submissions |
| AWS Lambda | Processes contact form requests |
| DynamoDB | Stores submitted messages |
| SNS Topic | Sends notification emails |
| IAM Role | Controls Lambda permissions |

### Important Note

The environment contains intentional issues including:

- Missing IAM permissions
- Unconfirmed SNS subscriptions
- Faulty Lambda logic

These problems will be diagnosed and fixed later in the project.

---

# Step 5: Monitor Stack Creation Progress

### Verify Deployment Status

1. I then Navigate to the **Events** tab
2. And Monitor resource creation progress
3. I Wait until the stack status changes to:

```bash
CREATE_COMPLETE
```

### Screenshot
![CloudFormation Events Tab](images/cloudformation-events.png)

---

# Step 6: Collect Stack Outputs

Once deployment completes:

1. I then open the **Outputs** tab
2. Record the following values:

| Output | Description |
|---|---|
| ApiUrl | Contact form API endpoint |
| LambdaFunction | Lambda function name |
| DynamoDBTable | DynamoDB table name |
| SNSTopic | SNS Topic ARN |

These values will be recorded throughout the troubleshooting process. 

### Screenshot
![CloudFormation Outputs](images/cloudformation-outputs.png)

---

# 📧 Step 7: Verify Email Subscription

### SNS Subscription Confirmation

1. Check your inbox or spam folder
2. Locate the AWS notification email
3. Subject line:

```text
AWS Notification - Subscription Confirmation
```

### Important

Do **NOT** confirm the subscription yet. You will revisit this later while troubleshooting SNS delivery issues.

### Screenshot

```md
![SNS Subscription Email](images/sns-subscription-email.png)
```

---

# 🔍 Step 8: Verify AWS Resources

## Check Lambda Function

1. Open the AWS Lambda Console
2. Locate the function:

```bash
ContactFormProcessor
```

3. Review:
   - Function code
   - Execution role
   - Permissions
   - Environment variables

### Screenshot

```md
![Lambda Function Verification](images/lambda-verification.png)
```

---

## Check DynamoDB Table

1. Open DynamoDB Console
2. Navigate to **Tables**
3. Locate:

```bash
ContactFormSubmissions
```

4. Verify the table is empty

### Screenshot

```md
![DynamoDB Table Verification](images/dynamodb-table.png)
```

---

## Check SNS Topic

1. Open Amazon SNS Console
2. Navigate to **Topics**
3. Locate:

```bash
ContactFormNotifications
```

4. Review the email subscription

### Screenshot

```md
![SNS Topic Verification](images/sns-topic.png)
```

---

## Check API Gateway

1. Open API Gateway Console
2. Locate:

```bash
ContactFormApi
```

3. Verify:
   - `/submit` resource exists
   - POST method is configured

### Screenshot

```md
![API Gateway Verification](images/api-gateway.png)
```

---

# ⚠️ Understanding the Broken Components

The deployed architecture contains several intentional issues.

## 1. Missing IAM Permissions

The Lambda function lacks permissions to:

- Write to DynamoDB
- Publish messages to SNS

Without the correct IAM policies, services cannot communicate successfully.

---

## 2. Unconfirmed SNS Subscription

SNS email subscriptions require manual confirmation before notifications can be delivered.

---

## 3. Lambda Code Issues

The Lambda function contains logic errors that may prevent proper execution even if permissions are corrected. 

---

# ✅ Next Steps

With the broken CloudFormation stack successfully deployed, the environment is now ready for troubleshooting.

In the next section, we will:

- Reproduce the contact form issue
- Test the API endpoint
- Investigate failures using CloudWatch Logs
- Begin identifying the root cause
