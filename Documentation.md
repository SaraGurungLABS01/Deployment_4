## Purpose
The purpose of this project is to deploy a web application on an Amazon EC2 instance and establish a Jenkins CI/CD pipeline. This deployment aims to provision a suitable EC2 instance, configure security groups, install Jenkins and required software packages, enhance Jenkins functionality with the "Pipeline Keep Running Step" plugin, set up Nginx as a reverse proxy, implement monitoring agents (CloudWatch or Datadog), update the Jenkinsfile for build and deployment automation, create a multi-branch pipeline, assess server performance and scalability, configure performance-based alerts, and enable email notifications in Jenkins. This project serves as a demonstration of best practices in cloud-based web application deployment and CI/CD automation.

## STEPS:

## Initial Steps : Creating a new Repository in Github
1. Created a new Repository under the name Deployment_2
2. Downloaded the zip files from the Repository : C4_deployment-3 and unzipped them
3. Uploaded the unzipped files to the new repository that was created
 

Follow these steps to deploy your application on AWS EC2:

**Step 1: Create AWS Resources**

Before we start deploying our application, we must ensure that we have the required AWS resources set up. These include a Virtual Private Cloud (VPC), a public subnet, and the necessary networking configurations. If these resources are not already in place, we should create them following AWS best practices. Below are the steps:

1. **Create a Virtual Private Cloud (VPC):**
   - Log in to the AWS Management Console.
   - Navigate to the VPC service.
   - Create a new VPC with an appropriate CIDR block (e.g., 10.0.0.0/16).
   - Configure the VPC's route tables and security group settings to allow internet access.

2. **Create a Public Subnet:**
   - Inside your VPC, create a public subnet with an associated route table.
   - Ensure that the subnet's route table includes a route to the internet gateway (0.0.0.0/0 via the internet gateway).

**Step 2: Launch an EC2 Instance**

Now that you have the network infrastructure in place, you can launch an EC2 instance where your application will be deployed. Here's how:

3. **Launch an EC2 Instance:**
   - Go to the EC2 service in the AWS Management Console.
   - Launch a new EC2 instance, selecting the T2 Medium instance type.
   - Choose the public subnet you created in step 2.
   - Configure security groups to allow inbound traffic on ports 80, 8080, 8000, and 22 as specified in your instructions.

**Step 3: Install Software and Plugins**

Next, you'll set up the necessary software and Jenkins plugins on your EC2 instance:

4. **SSH into the EC2 Instance:**
   - Use SSH to connect to your EC2 instance. You'll need the private key associated with the instance.

5. **Install Jenkins and Required Software:**
   - Update the package manager: `sudo apt update`
   - Install Jenkins: Follow the Jenkins installation instructions for your Linux distribution (https://www.jenkins.io/doc/book/installing/linux/).
   - Install the required software packages: 
     ```bash
     sudo apt install python3.10-venv python3-pip nginx -y
     ```

6. **Install Jenkins Plugin:**
   - Access the Jenkins web interface by opening your EC2 instance's public IP address on port 8080 (e.g., http://<EC2_Public_IP>:8080).
   - Install the "Pipeline Keep Running Step" plugin from the Jenkins plugin manager.

**Step 4: Configure Nginx**

Now, configure Nginx on your EC2 instance to act as a reverse proxy:

7. **Edit Nginx Configuration:**
   - Edit the Nginx configuration file:
     ```bash
     sudo nano /etc/nginx/sites-enabled/default
     ```
   - Replace the content with the provided configuration (changing the port from 80 to 5000 and configuring the proxy settings).

**Step 5: Install Monitoring Agent**

To monitor your EC2 instance, you'll need a monitoring agent:

8. **Install CloudWatch Agent:**
   - 

**Step 6: Update Jenkinsfile**

Now, update your Jenkinsfile with the provided script:

