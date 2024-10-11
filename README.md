# Animeman
 
```markdown
# Deploying a Manhwa Web Application on AWS with Auto Scaling and Siege Stress Test

## Overview
This documentation details the process of deploying a Manhwa web application on AWS, including setting up EC2, RDS, and S3 for image storage, and configuring Auto Scaling. A stress test was performed using `siege` to ensure the application's performance under load.

## Steps to Deploy the Application

### 1. Launch an EC2 Instance

- **Step 1:** Go to the **AWS Management Console**.
- **Step 2:** Navigate to **EC2** and click on **Launch Instance**.
- **Step 3:** Choose an appropriate **Amazon Machine Image (AMI)** (e.g., Amazon Linux 2).
- **Step 4:** Select an **instance type** (e.g., t2.micro for the free tier).
- **Step 5:** Configure instance details:
  - **Network:** Choose the default VPC.
  - **Auto-assign Public IP:** Enable.
- **Step 6:** Add storage (default 8GB is sufficient).
- **Step 7:** Add tags if necessary.
- **Step 8:** Configure **Security Groups**:
  - Allow **SSH (22)** for your IP.
  - Allow **HTTP (80)** from anywhere.
- **Step 9:** Review and launch the instance. Download the key pair for SSH access.

### 2. Install Required Packages on EC2

1. SSH into the EC2 instance:

   ```bash
   ssh -i your-key.pem ec2-user@<EC2_Public_IP>
   ```

2. Update the instance and install necessary software:
   ```bash
   sudo yum update -y
   sudo yum install httpd php mysql -y
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

3. Install `siege` for stress testing:
   ```bash
   sudo amazon-linux-extras install epel
   sudo yum install siege
   ```

### 3. Configure RDS for Database

  **Step 1:** Go to **RDS** in the AWS Management Console.
- **Step 2:** Click on **Create Database**.
- **Step 3:** Choose **MySQL** as the database engine.
- **Step 4:** Select **DB instance size** (e.g., db.t2.micro for the free tier).
- **Step 5:** Configure the settings (username, password, etc.).
- **Step 6:** Make sure to allow connections from the EC2 instance's security group.
- **Step 7:** Save the endpoint and credentials for use in your application.

### 4. Store Images in S3

 **Step 1:** Go to **S3** in the AWS Console.
- **Step 2:** Create a new **S3 bucket** to store the manhwa images.
- **Step 3:** Upload your images.
- **Step 4:** Ensure the bucket has proper permissions (e.g., make it public for web access).

### 5. Configure Auto Scaling

1. Create a **Launch Template**:
   - Go to **EC2 Dashboard** > **Launch Templates**.
   - Create a new launch template with the AMI, instance type, and security group settings.

2. Create an **Auto Scaling Group**:
   - Go to **Auto Scaling** and create an Auto Scaling group.
   - Choose the launch template you created.
   - Set the desired capacity, min, and max instances (e.g., 1 min, 3 max).
   - Configure health checks and scaling policies (e.g., scale up based on CPU > 70%).

### 6. Enable HTTPS using AWS Certificate Manager (ACM)

- **Step 1:** Go to **AWS Certificate Manager** and request a certificate for your domain (if available).
- **Step 2:** Attach the certificate to your **EC2 load balancer** or domain setup to enable HTTPS.

### 7. Running Siege Stress Test

1. Run `siege` on your EC2 instance:
   ```bash
   siege -c 50 -t 5M http://<Your_IP>/C
   ```

2. Monitor the **Auto Scaling Group** in the EC2 dashboard to see if new instances are launched during the test.

## Screenshots

- **EC2 setup**
- 
- **Auto Scaling Group configuration**
- **RDS setup**
- **S3 bucket**
 ![Screenshot (587)](https://github.com/user-attachments/assets/818173df-c168-42c6-b0d8-f069261b12ed)

- **Siege stress test output**
![Screenshot (582)](https://github.com/user-attachments/assets/8eb14429-38cd-4a24-9b94-6d2aa2716d91)

 ![Screenshot (581)](https://github.com/user-attachments/assets/a24b975c-a849-41bf-8eef-772c7663c6bd)


## How to Deploy the Project

1. Clone the repository to your local machine:
   ```bash
   git clone https://github.com/Omkar0070/Animeman.git
   ```

2. SSH into your EC2 instance and upload your project:
   ```bash
   scp -i your-key.pem -r your-project/ ec2-user@<EC2_Public_IP>:/var/www/html/
   ```

3. Ensure the `httpd` service is running:
   ```bash
   sudo systemctl start httpd
   ```

4. Open the application in your browser using `http://<EC2_Public_IP>`.

## Conclusion

This project demonstrates the complete process of deploying a scalable and secure web application on AWS, including the use of EC2, RDS, S3, and Auto Scaling to handle increased traffic. A stress test using `siege` confirms the robustness of the setup.

 
```
