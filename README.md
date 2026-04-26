# deploy-nodejs-docker-ec2-rds
Deployment of a Dockerized Node.js application on AWS EC2 with RDS integration, accessible via public IP.

## About the Project:

This project demonstrates how to deploy a **pre-built Dockerized Node.js application** on an AWS EC2 instance and connect it to an AWS RDS database.

Instead of building the application manually, the Docker image is **pulled directly from Docker Hub** and executed using environment variables to establish a connection with the RDS database.

## Technologies Used:

- **Docker** → Container runtime
- **AWS EC2** → Hosting server
- **AWS RDS (MySQL)** → Managed database
- **Docker Hub** → Pre-built image repository
- **Linux (Ubuntu)** → Server environment

## Prerequisites:

- AWS account
- EC2 instance (Ubuntu)
- RDS instance (MySQL)

## Step 1: Launch EC2 Instance

1. Go to AWS Console → EC2
2. Launch Amazon Linux instance
3. Configure security group:
    - Allow SSH (22)
    - Allow HTTP (80)

![Project Screenshot](/images/instance1.png)
![Project Screenshot](/images/instance2.png)
![Project Screenshot](/images/instanceDone.png)

## Step 2: Setup RDS Database

1. Go to AWS Console → RDS
2. Create MySQL database
3. Enable public access (for testing)
4. Configure security group:
    - Allow port **3306**
    - Source: EC2 security group
5. Copy endpoint

![Project Screenshot](/images/createRDS1.png)
![Project Screenshot](/images/createRDS2.png)
![Project Screenshot](/images/createRDS3.png)
![Project Screenshot](/images/createRDS4.png)
![Project Screenshot](/images/RDSdone.png)

## Step 3: Install Docker on EC2

1. connect EC2 instance via Git bash
2. Install Docker

```bash
sudo yum update
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
```

## Step 4: Pull Docker Image

1. Pull the per created image on the your server

```bash
sudo docker pull philippaul/node-mysql-app:02
#check the another repo that i will showing how to create docker images, push, pull, etc
```

## Step 5: Run Application Container

```bash
sudo docker run --rm -p 80:3000 \
-e DB_HOST="database-1.c2bguysa4229.us-east-1.rds.amazonaws.com" \
-e DB_USER="admin" \
-e DB_PASSWORD="fBJhgxS9Io4NNRt9fjTJ" \
-d philippaul/node-mysql-app:02
```

![Project Screenshot](/images/dockercmd.png)

## Step 6: Access Application

Open browser:

```
http://<EC2-PUBLIC-IP>
http://54.86.206.216/
```
![Project Screenshot](/images/nodeappRUN.png)
![Project Screenshot](/images/DATAadded.png)

## Step 7: Verify Database Connection & Check Data

```bash
sudo docker run -it --rm mysql:8.0 \
mysql -h database-1.c2bguysa4229.us-east-1.rds.amazonaws.com -u admin -p
```
![Project Screenshot](/images/dbmscheck.png)
![Project Screenshot](/images/DataSaved.png)

## Step 8: Delete All Resources

1. Go to AWS RDS console
2. Then select the database and click the delete
3. Go to the AWS EC2 console 
4. Then select instance which you want to delete
