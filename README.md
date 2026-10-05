Decode Labs — AWS RDS MySQL & EC2 Bastion Host Setup

![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![Amazon EC2](https://img.shields.io/badge/Amazon-EC2-blue)
![Amazon RDS](https://img.shields.io/badge/Amazon-RDS-red)
![MySQL](https://img.shields.io/badge/Database-MySQL-blue)
![Amazon Linux](https://img.shields.io/badge/OS-Amazon%20Linux%202023-green)
![Internship](https://img.shields.io/badge/Decode%20Labs-Internship-purple)

> A hands-on AWS cloud infrastructure project completed as part of my **Decode Labs internship**, involving Amazon RDS MySQL, Amazon EC2, Amazon Linux 2023, security groups, MySQL client configuration, and database connectivity testing.

---

# 📌 Project Overview

This project demonstrates the practical deployment and configuration of AWS infrastructure using **Amazon RDS and Amazon EC2**.

The objective was to create a managed MySQL database using Amazon RDS and configure an Amazon EC2 instance as a bastion host from which the database could be accessed and tested.

Throughout the implementation, I worked with:

- Amazon RDS
- Amazon EC2
- Amazon Linux 2023
- AWS VPC
- AWS Security Groups
- EC2 Instance Connect
- MySQL/MariaDB command-line client
- Linux terminal commands
- RDS connectivity
- Network troubleshooting

This project was completed as part of my **Decode Labs internship** and provided hands-on experience with real AWS infrastructure rather than only theoretical concepts.

---

# 🎯 Objectives

The main objectives of this task were:

- [x] Create an Amazon RDS MySQL database
- [x] Configure the RDS database
- [x] Create an Amazon EC2 instance
- [x] Configure EC2 as a bastion host
- [x] Access the EC2 instance using EC2 Instance Connect
- [x] Prepare Amazon Linux 2023
- [x] Install the MySQL-compatible command-line client
- [x] Verify the MySQL client installation
- [x] Identify the RDS endpoint and port
- [x] Attempt EC2 → RDS connectivity
- [x] Document the implementation and troubleshooting process

---

# 🏗️ Architecture

```text
                         AWS CLOUD
                             │
                             ▼
                         VPC NETWORK
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
       ┌───────────────┐             ┌───────────────┐
       │      EC2      │             │      RDS      │
       │ Bastion Host  │────────────►│     MySQL     │
       │   t3.micro    │   TCP 3306  │  db.t4g.micro │
       └───────────────┘             └───────────────┘
              ▲
              │
              │ EC2 Instance Connect
              │
          Developer

The intended connection flow is:

Developer
    │
    │ EC2 Instance Connect
    ▼
EC2 Bastion Host
    │
    │ MySQL / TCP 3306
    ▼
Amazon RDS MySQL
☁️ AWS Services Used
AWS Service	Purpose
Amazon EC2	Bastion host
Amazon RDS	Managed MySQL database
Amazon VPC	Network environment
Security Groups	Network access control
EC2 Instance Connect	Browser-based EC2 access
🗄️ Step 1 — Create Amazon RDS MySQL Database

An Amazon RDS database was created using the AWS Management Console.

RDS Configuration
Property	Configuration
Database Identifier	database-1
Database Engine	MySQL Community
DB Instance Class	db.t4g.micro
Region	Europe (Stockholm)
Availability Zone	eu-north-1c
Port	3306
Master Username	admin
Network Type	IPv4

The RDS database reached the Available state successfully.

RDS Endpoint

The RDS endpoint was obtained from:

RDS
→ Databases
→ database-1
→ Connect
→ Code snippets

The database uses the standard MySQL port:

3306

The database password is intentionally not included anywhere in this repository.

🔐 Step 2 — Configure RDS Security Group

The RDS database was associated with an AWS Security Group.

The Security Group controls network traffic reaching the database.

The intended rule is:

Protocol: TCP
Port: 3306
Source: EC2 Bastion Security Group

This allows the EC2 bastion host to communicate with the RDS MySQL database.

🖥️ Step 3 — Create EC2 Bastion Host

An EC2 instance was launched to act as a bastion host.

EC2 Configuration
Property	Value
Instance Name	decodelabs-bastion
Instance Type	t3.micro
Operating System	Amazon Linux 2023
Region	Europe (Stockholm)
Availability Zone	eu-north-1b
Username	ec2-user
Purpose	Bastion Host

The instance successfully reached:

Running

with successful instance status checks.

🔑 Step 4 — Access EC2 Using EC2 Instance Connect

Instead of using a local SSH client, the EC2 instance was accessed through the AWS browser-based connection.

The process was:

AWS Console
    ↓
EC2
    ↓
Instances
    ↓
decodelabs-bastion
    ↓
Connect
    ↓
EC2 Instance Connect
    ↓
Connect

The Linux terminal was successfully opened.

Example prompt:

[ec2-user@ip-172-31-44-205 ~]$

This confirmed successful access to the Amazon Linux server.

🐧 Step 5 — Prepare Amazon Linux

The EC2 instance was running Amazon Linux 2023.

The Linux terminal was used to configure the environment and prepare it for database connectivity.

🐬 Step 6 — Install MySQL Client

Initially, the MySQL command-line client was not available.

The following command was used:

mysql --version

The system initially returned:

-bash: mysql: command not found

This confirmed that the MySQL client needed to be installed.

The MariaDB-compatible MySQL client was then installed using:

sudo dnf install mariadb105 -y

After installation, the client was verified:

mysql --version

The installed version was:

mysql Ver 15.1 Distrib 10.5.29-MariaDB

This confirmed that the MySQL-compatible command-line client was successfully installed.

🔗 Step 7 — Identify RDS Connection Details

The RDS connection information was obtained from the AWS RDS console.

The important connection parameters were:

Host:
database-1.cdui8sgwuck0.eu-north-1.rds.amazonaws.com

Port:
3306

Username:
admin

The password was entered interactively and was never stored in the repository.

🔌 Step 8 — Test EC2 → RDS Connectivity

The database connection was tested from the EC2 bastion host.

The command used was:

mysql -h database-1.cdui8sgwuck0.eu-north-1.rds.amazonaws.com -P 3306 -u admin -p

The command requested the RDS master password interactively.

The connection attempt returned:

ERROR 2002 (HY000): Can't connect to MySQL server
🛠️ Step 9 — Troubleshooting

The connectivity test demonstrated that the MySQL client itself was functioning correctly, but the EC2 instance was not able to establish the network connection to the RDS endpoint.

The troubleshooting process identified AWS networking and Security Group configuration as the next area to investigate.

The intended network path is:

EC2
 │
 │ TCP 3306
 ▼
RDS MySQL

The RDS Security Group must allow traffic from the Security Group associated with the EC2 bastion host.

📊 Implementation Status
Component	Status
AWS Environment	✅ Completed
RDS MySQL Deployment	✅ Completed
RDS Configuration	✅ Completed
RDS Security Group Setup	✅ Configured
EC2 Bastion Host	✅ Completed
Amazon Linux 2023	✅ Completed
EC2 Instance Connect	✅ Completed
MySQL/MariaDB Client	✅ Completed
MySQL Client Verification	✅ Completed
RDS Endpoint Identification	✅ Completed
EC2 → RDS Connectivity Test	✅ Tested
Successful RDS Login	⏳ Pending
Database Operations	⏳ Pending
📸 Screenshots

All implementation screenshots are available in the:

screenshots/

folder.

Recommended screenshots:

screenshots/
│
├── 01-rds-database.png
├── 02-rds-connectivity.png
├── 03-ec2-instance.png
├── 04-ec2-connect.png
├── 05-ec2-terminal.png
├── 06-mysql-installation.png
├── 07-mysql-version.png
└── 08-rds-connection-test.png

These screenshots document the actual implementation process from AWS resource creation through connectivity testing.

🧪 Important Commands Used
Check MySQL client
mysql --version
Install MySQL-compatible client
sudo dnf install mariadb105 -y
Verify installation
mysql --version
Connect to RDS
mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p
Exit MySQL
exit;
🔒 Security

Sensitive credentials were not included in this repository.

The following should never be committed:

❌ RDS passwords
❌ AWS access keys
❌ AWS secret keys
❌ Private SSH keys
❌ .pem files
❌ .env files containing credentials
❌ Database credentials

A .gitignore file is included to help prevent accidental exposure of sensitive files.

🧠 What I Learned

This project gave me practical experience with several important cloud concepts.

Amazon EC2

I learned how to deploy and access a Linux-based cloud server.

Amazon RDS

I learned how to deploy and configure a managed relational database using AWS.

Bastion Hosts

I learned how an EC2 instance can act as an intermediate server for accessing infrastructure.

Security Groups

I gained practical understanding of AWS network access control and inbound/outbound rules.

Linux

I worked with Amazon Linux 2023 through the command line.

MySQL

I learned how to install and use a MySQL-compatible command-line client.

Networking

I learned how AWS resources communicate through:

VPC
 │
 ├── Subnets
 │
 ├── Security Groups
 │
 ├── EC2
 │
 └── RDS
Troubleshooting

One of the most valuable parts of the task was encountering a real connectivity issue and investigating whether the problem was related to the database client, credentials, or AWS networking.

🚀 Future Improvements

The next stage of this project is to complete the EC2 → RDS connection and perform actual database operations.

Planned improvements include:

 Correct EC2/RDS Security Group connectivity
 Successfully connect to RDS
 Create a sample database
 Create tables
 Insert sample records
 Execute SQL queries
 Perform CRUD operations
 Configure secure database connectivity
 Add monitoring using CloudWatch
 Explore IAM-based authentication
 Recreate infrastructure using Terraform
 Connect a web application to the RDS database
💼 Internship Context

This project was completed as part of my Decode Labs Internship.

The project helped me move beyond theoretical cloud concepts and gain practical experience with:

AWS
+
Linux
+
Databases
+
Networking
+
Security
+
Troubleshooting
👨‍💻 Author
Saketh Raju

B.Tech Computer Science & Engineering (AI & ML)

Areas of Interest
Cloud Computing
AWS
Artificial Intelligence & Machine Learning
Software Development
Databases
DevOps
⭐ Final Outcome

The AWS infrastructure required for the project was successfully deployed and configured.

Completed
✅ Amazon RDS MySQL
✅ Amazon EC2
✅ Amazon Linux 2023
✅ EC2 Bastion Host
✅ EC2 Instance Connect
✅ MySQL/MariaDB Client
✅ RDS Configuration
✅ Security Group Configuration
✅ Connectivity Testing

The remaining work is to resolve the EC2 → RDS network connectivity and perform database operations.

📚 Technologies
Amazon Web Services
Amazon EC2
Amazon RDS
Amazon VPC
AWS Security Groups
Amazon Linux 2023
MySQL
MariaDB Client
Linux
Bash
