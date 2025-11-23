# manual-api-deploy-aws
This repository registers my first AWS deploy of a Flag's API for my post graduation course.
## Gabriel Berno Koto

# Project Description

The goal was to develop and implement a safe and functional infrastructure on AWS to host a RESTFUL API with data persistency.

This project involves:

* **Network Isolation:** VPC creation with a public and a private subnet.
* **Computing:** EC2 Instance provision exposed to internet.
* **Database:** PostgreSQL RDS provisioning isolated on private subnet.
* **Security:** Granular configuration of Security Groups, assuring the DB only accepts connections from the EC2 running the aplication.
* **Configuration:** Managing credentials and connections using enviorment variables, with no hard-coding.

The infrastructure was developed to assure the DB stays inaccessible directly from the internet, with the application working as a gateway for the requests.

<img width="477" height="624" alt="image" src="https://github.com/user-attachments/assets/b1ac1a53-de7c-47ba-b1f5-487c1a76cfe9" />

# Technologies Involved
* **Cloud Provider:** Amazon Web Services (AWS).
* **OS:** Amazon Linux 2023.
* **Database:** PostgreSQL 13 (RDS.
* **Framework:** Python 3, Flask, Gunicorn.
* **Deploy tools:** Git, SSH, Bash Scripting.

# Permission (AWS IAM)
To reproduce this, the IAM user must have the following permissions:

* **AmazonEC2FullAccess:** To create instances, Security Groups, Key Pairs and Elastic IPs.
* **AmazonRDSFullAccess:** To create DB instances and Subnet Groups.
* **AmazonVPCFullAccess:** To create VPC, Subnets, Internet Gateways and Route tables.
* **Learner Lab imposed several limitation, like the iam:CreateRole permission being blocked.

# Cost projetction
This architecture was developed thinking about AWS Free Tier.

| Service | Cost (USD) |
| ------------- | ------------- |
| EC2 | Free Tier or ~$8.50/m |
| RDS  | Free Tier or ~$12.00/m  |
| EBS  | Free Tier or ~$2.40/m  |
| Data Transfer  | Free Tier (First 100GB)  |
| Elastic IP | Free if in use  |
| Total  | Free Tier or ~$12.00/m  |

# Deploy

### 1. Provisionamento (Console AWS)**

Before running the code, manualy create the infrastructure on AWS:

1. VPC: Create the VPC pos-vpc with 1 public subnet and 2 private subnets (different AZs).

2. RDS: Create a PostgreSQL 13 instance in the private subnet.

3. EC2: Create an Amazon Linux 2023 instance in the public subne, associating it with the Security Group that allows SSH and  Port 5000.



### 2. Setting up the Server**

Access the EC2 using SSH:

```
ssh -i your-key.pem ec2-user@your_public_ip
```

Install all dependencies:
```
sudo yum update -y
sudo yum install -y git python3 python3-pip postgresql15
```



### 3. Application Installation:**
Clone this repo:
```
git clone <(https://github.com/dougls/toggle-master-monolith.git)>
```
Set up the Python virtual env:
```
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```



### 4. Configuring variables and DB:**

Export the enviroment variables (use your RDS information):
```
export DB_HOST='your_endpoint.rds.amazonaws.com'
export DB_NAME='togglemaster'
export DB_USER='your_user'
export DB_PASSWORD='your_pass'
export DB_PORT='5432'
```
Manually create the database:
```
psql -h $DB_HOST -U $DB_USER -d postgres -c "CREATE DATABASE togglemaster;"
# (input the password when needed)
```



### 5. Running the application:**
Run the entrypoint.sh script, that wait for DB connection, create the tables and starts the Gunicorn server:
```
chmod +x entrypoint.sh
nohup ./entrypoint.sh > app.log 2>&1 &
```
Check if the app is running:
```
curl http://localhost:5000/health
# Expected: {"status":"ok"}
```



