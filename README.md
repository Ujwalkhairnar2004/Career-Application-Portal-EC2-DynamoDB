
# Career Application Portal — EC2 + DynamoDB

A full-stack job/career application form built with **HTML/CSS**, **Node.js/Express**, and **AWS DynamoDB**, hosted on an **EC2** instance. This project demonstrates a real-world serverless-data-backed web app using core AWS services and secure, credential-free access via IAM Roles.

---

## 🏗️ Architecture

```
              INTERNET
                  |
                  v
             EC2 INSTANCE
                  |
            Nginx / Node.js
                  |
                  v
        Career Application Form
                  |
                  v
          Express Backend
                  |
                  v
              DynamoDB
                  |
                  v
          Application Records
```

---

## ✨ Features

- Responsive, modern career/job application form (HTML + CSS)
- Sections for Personal Info, Career Info, Location, and Skills/Profile
- Client-side validation and async submission (Fetch API, no page reload)
- Express.js backend with a `/register` API endpoint
- Data persisted in **DynamoDB** using the AWS SDK v3
- No hardcoded AWS credentials — access via an **IAM Role** attached to EC2
- Success/error messaging on the frontend

---

## 🧰 Tech Stack

| Layer      | Technology                                  |
|------------|----------------------------------------------|
| Frontend   | HTML5, CSS3, Vanilla JavaScript               |
| Backend    | Node.js, Express                              |
| Database   | AWS DynamoDB                                  |
| Hosting    | AWS EC2                                       |
| SDK        | `@aws-sdk/client-dynamodb`, `@aws-sdk/lib-dynamodb` |
| Auth       | AWS IAM Role (no static credentials)          |

---

## 📁 Project Structure

```
career-application-portal/
│
├── server.js
├── package.json
│
└── public/
    └── index.html
```

---

## 📋 Prerequisites

- An AWS account
- An EC2 instance (Amazon Linux) with SSH access
- Basic familiarity with the AWS Console

---

## 🚀 Setup Guide

### 1. Create the DynamoDB Table

In the AWS Console:

`DynamoDB → Tables → Create table`

- **Table name:** `CareerApplications`
- **Partition key:** `applicationId` (String)
- Leave the remaining settings as default

### 2. EC2 Setup

SSH into your instance:

```bash
ssh -i your-key.pem ec2-user@YOUR_EC2_PUBLIC_IP
```

Install Node.js:

```bash
sudo dnf update -y
sudo dnf install -y nodejs npm
```

Verify installation:

```bash
node -v
npm -v
```

Create and initialize the project:

```bash
mkdir career-application-portal
cd career-application-portal
npm init -y
npm install express @aws-sdk/client-dynamodb @aws-sdk/lib-dynamodb
```

### 3. Add the Backend (`server.js`)

Create `server.js` and set up an Express app that:

- Serves the static frontend from `public/`
- Exposes a `POST /register` endpoint
- Writes submitted fields (name, email, mobile, dob, position, experience, qualification, salary, city, address, skills, linkedin, workMode, about) to the `Students` DynamoDB table using `PutCommand`
- Generates a unique `studentId` per submission with `crypto.randomUUID()`

### 4. Add the Frontend (`public/index.html`)

Create the `public` folder and `index.html` with the Career Application form (Personal Information, Career Information, Location, and Skills & Profile sections), styled with the included CSS for a polished, professional look.

### 5. Grant EC2 Permission to DynamoDB (IAM Role)

Avoid hardcoding AWS keys. Instead, attach an IAM Role:

`IAM → Roles → Create role`

- **Trusted entity:** AWS service
- **Use case:** EC2
- **Policy:** `AmazonDynamoDBFullAccess` (for learning/testing — scope this down for production)
- **Role name:** e.g. `EC2-DynamoDB-Role`

Attach the role to your instance:

`EC2 → Instances → Select instance → Actions → Security → Modify IAM role → EC2-DynamoDB-Role`

### 6. Run the Application

```bash
cd ~/career-application-portal
node server.js
```

You should see:

```
Server running on port 3000
```

### 7. Open the Port in the Security Group

`EC2 → Security Groups → Inbound rules → Edit inbound rules`

- **Type:** Custom TCP
- **Port:** 3000
- **Source:** `0.0.0.0/0` (restrict this for production, or front with Nginx/a load balancer)

### 8. Access the App

```
http://YOUR-EC2-PUBLIC-IP:3000
```

### 9. Verify Data in DynamoDB

`AWS Console → DynamoDB → Tables → Students → Explore table items`

You should see submitted records with fields like `studentId`, `name`, `email`, `position`, `city`, `createdAt`, etc.

---

## 🔐 Security Notes

- Never commit AWS access keys/secrets to source control.
- Use IAM Roles for EC2-to-AWS-service access instead of static credentials.
- Restrict inbound Security Group rules in production; place Nginx or a load balancer in front of the app instead of exposing the app port directly.

---

## 📚 What You'll Learn

This project ties together several core AWS/backend concepts:

- EC2 provisioning and configuration
- IAM Roles for secure, credential-free AWS access
- Node.js/Express backend development
- DynamoDB as a NoSQL data store
- AWS SDK v3 usage (`DynamoDBClient`, `DynamoDBDocumentClient`)
- Building and serving a responsive frontend

---
## OUTPUT
<img width="1366" height="768" alt="{087D4F47-453E-4F7E-934C-755D6E2A9561}" src="https://github.com/user-attachments/assets/e7f91dab-81e4-4354-9666-93e6437d6b7a" />
<img width="1366" height="768" alt="{EDB00CB2-FB43-4C35-BC5F-A4C4A7ED203F}" src="https://github.com/user-attachments/assets/c79d025e-8d42-485a-9613-b11fc93c3eab" />
<img width="1366" height="768" alt="{D33DD1C0-6097-4E10-AB39-E9B9FC9EF12F}" src="https://github.com/user-attachments/assets/bcd86d31-8b1f-4273-92b2-36c02d24a380" />
<img width="1366" height="768" alt="{3F838115-E105-457D-8C26-B6EA04C66375}" src="https://github.com/user-attachments/assets/9e73afcc-6bfb-423d-9d55-ee9636d0e5d6" />
<img width="1366" height="768" alt="{E623E376-FB31-47C7-A349-ED34AECBF9FD}" src="https://github.com/user-attachments/assets/5fc8b5af-d4b3-47d2-b68d-1ca6f77c1cbe" />
<img width="1366" height="768" alt="{4FAE36DE-76AA-489F-9EEB-BD44B5F10119}" src="https://github.com/user-attachments/assets/a6072e37-9524-4a42-b5f1-a41ff013616c" />

## 📄 License

This project is provided for learning and demonstration purposes. Feel free to adapt it for your own use.
