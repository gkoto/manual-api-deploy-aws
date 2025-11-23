# manual-api-deploy-aws
This repository register my first AWS deploy of Flag's API.

The goal was to develop and implement a safe and functional infrastructure on AWS to host a RESTFUL API with data persistency.

This project involves:

* **Network Isolation:** VPC creation with a public and a private subnet.
* **Computing:** EC2 Instance provision exposed to internet.
* **Database:** PostgreSQL RDS provisioning isolated on private subnet.
* **Security:** Granular configuration of Security Groups, assuring the DB only accepts connections from the EC2 running the aplication.
* **Configuration:** Managing credentials and connections using enviorment variables, with no hard-coding.

The infrastructure was developed to assure the DB stays inaccessible directly from the internet, with the application working as a gateway for the requests.

<img width="477" height="624" alt="image" src="https://github.com/user-attachments/assets/b1ac1a53-de7c-47ba-b1f5-487c1a76cfe9" />
