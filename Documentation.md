## Purpose
The purpose of this project is to deploy a web application on an Amazon EC2 instance and establish a Jenkins CI/CD pipeline. This deployment aims to provision a suitable EC2 instance, configure security groups, install Jenkins and required software packages, enhance Jenkins functionality with the "Pipeline Keep Running Step" plugin, set up Nginx as a reverse proxy, implement monitoring agents (CloudWatch or Datadog), update the Jenkinsfile for build and deployment automation, create a multi-branch pipeline, assess server performance and scalability, configure performance-based alerts, and enable email notifications in Jenkins. This project serves as a demonstration of best practices in cloud-based web application deployment and CI/CD automation.

## STEPS:

## Initial Steps : Creating a new Repository in Github
1. Created a new Repository under the name Deployment_4
2. Downloaded the zip files from the Repository : C4_deployment-4 and unzipped them
3. Uploaded the unzipped files to the new repository that was created
 

Follow these steps to deploy your application on AWS EC2:

## Step 1: Create AWS Resources**

Before we start deploying our application, we must ensure that we have the required AWS resources set up. These include a Virtual Private Cloud (VPC), a public subnet, and the necessary networking configurations. If these resources are not already in place, we should create them following AWS best practices. Below are the steps:

1. **Create a Virtual Private Cloud (VPC):**
   - Log in to the AWS Management Console.
   - Navigate to the VPC service.
   - Create a new VPC with an appropriate CIDR block (e.g., 10.0.0.0/16).
   - Configure the VPC's route tables and security group settings to allow internet traffic on ports 80, 8080, 8000, and 22.

2. **Create a Public Subnet:**
   - Inside our VPC, create a public subnet with an associated route table.
   - Ensure that the subnet's route table includes a route to the internet gateway (0.0.0.0/0 via the internet gateway).

## Step 2: Launch an EC2 Instance**

Now that we have the network infrastructure in place, we can launch an EC2 instance where your application will be deployed. Here's how:

 **Launch an EC2 Instance:**
   - Go to the EC2 service in the AWS Management Console.
   - Launch a new EC2 instance, selecting the T2 Medium instance type.
   - Choose the VPC and public subnet you created in step 1.

## Step 3: Install Software and Plugins**

Next, we'll set up the necessary software and Jenkins plugins on our EC2 instance:

1. **Install Jenkins and Required Software:**
   - Update the package manager: `sudo apt update`
   - Install Jenkins: Follow the Jenkins installation instructions (https://pkg.jenkins.io/debian/).
   - Install the required software packages: 
     ```bash
     sudo apt install python3.10-venv python3-pip nginx -y
     ```

3. **Install Jenkins Plugin:**
   - Access the Jenkins web interface by opening your EC2 instance's public IP address on port 8080 (e.g., http://<EC2_Public_IP>:8080).
   - Install the "Pipeline Keep Running Step" plugin from the Jenkins plugin manager.

## Step 4: Configure Nginx**

Now, configure Nginx on our EC2 instance to act as a reverse proxy:

**Edit Nginx Configuration:**
   - Edit the Nginx configuration file:
     ```bash
     sudo nano /etc/nginx/sites-enabled/default
     ```
   - Replace the content with the provided configuration (changing the port from 80 to 5000 and configuring the proxy settings).

     ![image](https://github.com/SaraGurungLABS01/Deployment_4/assets/140760966/db2bec93-4def-44df-a01e-dc87a5805614)


## Step 5: Install Monitoring Agent

To monitor our EC2 instance, we'll need a monitoring agent:


 **Install CloudWatch Agent:**
- Follow the link for the steps : (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) on your EC2 instance.
- Run the configuration wizard to set up the CloudWatch agent. Ensure it collects metrics.
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```


![image](https://github.com/SaraGurungLABS01/Deployment_4/assets/140760966/1f827c66-1471-4bda-af2f-e4cf82308e72)

  
## Step 6: Jenkin Pipeline Configuration and Update JenkinsFile using Git

- Create a Jenkins job of the "multibranch pipeline" type. This job will automate tasks such as building and deploying based on your code's branches.
- Next, in the job's settings, establish a connection between Jenkins and your GitHub repository. This linkage allows Jenkins to interact with your repository, including fetching code and managing builds.
- Navigate to your GitHub repository's settings and set up a webhook that points to your Jenkins instance. This webhook acts as a communication channel, prompting Jenkins to automatically initiate builds whenever changes are pushed to your repository.
  

**Thenafter**

Now, update our Jenkinsfile with the provided script:

![image](https://github.com/SaraGurungLABS01/Deployment_4/assets/140760966/610b9c00-655d-4bc7-b9a3-0b815eb9e667)

 **Steps**
- Created a new branch "new_branch2" using "git checkout -b new_branch2" for Jenkinsfile changes.
- Made changes to the Jenkinsfile and used "git add" to stage them.
- Committed changes with "git commit" and provided a descriptive message.
- Pushed the committed changes to "new_branch2" on GitHub using "git push origin new_branch2."
- Created a pull request on GitHub detailing the changes.
- Successfully merged the pull request into the main branch.

## Result

![image](https://github.com/SaraGurungLABS01/Deployment_4/assets/140760966/2dc4fc63-343f-4540-8bd9-91de27a48463)

## Step 7: Performance Monitoring
Monitor Server Performance:
- Use tools like htop for real-time monitoring and utilize CloudWatch or Datadog for comprehensive performance metrics.
- Created a CloudWatch alarm to receive notifications when CPU usage exceeds 10% for 5 minutes.
    - Configure the alarm to trigger actions such as sending email notifications or executing automated responses when the threshold is breached.
    - By setting up CloudWatch alarms, we were able to proactively monitor and respond to CPU usage anomalies, ensuring the smooth operation of our application on AWS EC2.
 
 "C:\Users\Saraswati\Desktop\Screenshot 2023-10-03 024221.png"
 
Some helpful links:
- https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance-fleet.html
- https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html

## Step 8: Jenkins Email Notifications

To configure email notifications in Jenkins for build status updates, follow these steps:
- Provide System Admin Email Address:
  - Access the Jenkins web interface and navigate to "Manage Jenkins" > "Configure System."
  - In the configuration settings, locate the field to input the system admin email address. This ensures notifications are sent to the specified email.
- Enable Editable Email Notification:
   - To receive email alerts, especially in case of build failures, you can either add a post-build action in the Jenkinsfile or configure it within the "Post-build Actions" section.

Following these steps allows Jenkins to send email notifications, providing real-time updates on build status during the deployment process.

## System Diagram


## T2.micro Considerations:

A t2.micro instance may struggle with the current deployment due to its limited CPU resources, which could result in long build times due to a lack of resources, performance issues under even moderate loads, and stability problems when multiple processes compete for CPU. While a t2.medium instance, as used in this deployment, offers better performance and resource availability, it's essential to assess your application's resource demands. If the server can handle the installed software and workload effectively, the t2.medium instance proves suitable. However, if resource limitations become a concern, especially during resource-intensive tasks, transitioning to a t2.micro instance might result in longer build times, performance bottlenecks, and potential instability under moderate loads.

## Optimization

- Resource Scaling: Vertically scale the EC2 instance to larger types as our application's resource demands grow. This ensures adequate CPU, memory, and network resources are available, optimizing performance under varying workloads.

- Load Balancing: Implement horizontal scaling by deploying multiple instances behind a load balancer. This strategy enhances reliability and distributes incoming traffic evenly, allowing our application to handle increased loads seamlessly.

- Content Delivery Network (CDN): Integrate a Content Delivery Network (CDN) to serve static assets like images, stylesheets, and scripts from edge locations. CDNs significantly reduce latency, improve content delivery speed, and offload traffic from our EC2 instances, optimizing overall performance.
