# Awschallenge
Terraform script for automating  an aws scalable webapp
---

## Infrastructure Overview

This project provisions a highly available, scalable, and secure web application infrastructure on AWS using Terraform. The architecture supports efficient deployment and follows DevOps best practices, focusing on automation, observability, and fault tolerance.

### The Key Resources includes;

- **VPC, Subnets, and Networking:**
  The infrastructure uses a custom VPC with public and private subnets across multiple availability zones. An Internet Gateway provides outbound internet access for the public subnets, while a NAT Gateway enables instances in the private subnets to connect to the internet securely.

- **Route Tables:**
  Public and private subnets are associated with separate route tables. The public route table directs internet-bound traffic through the Internet Gateway, while the private route table routes external traffic through the NAT Gateway.NAT Gateway in a public subnet is very essential for enabling private instances to access the internet while keeping those instances themselves isolated from direct external access.

- **Load Balancer and Auto Scaling Group:**
  An Application Load Balancer (ALB) handles incoming traffic, distributing it across EC2 instances in the Auto Scaling Group (ASG). The ASG ensures the application scales based on traffic demand, maintaining high availability by running across multiple availability zones.The Auto Scaling Group manages the lifecycle of EC2 instances, automatically scaling the number of instances up or down based on the defined policies and CloudWatch alarms. The ASG is connected to the load balancer to distribute traffic across the EC2 instances. When the ASG scales out (adds more instances) or scales in (removes instances), the load balancer automatically registers or deregisters these instances.The ASG is triggered by CloudWatch alarms that monitor metrics like CPU utilization. For example, when the CPU usage goes above a certain threshold, a CloudWatch alarm triggers the ASG to launch more instances.The Auto Scaling Group therefore  dynamically adjusts the number of EC2 instances based on demand. This elastic behavior allows the application to scale out during traffic spikes and scale in when demand decreases, optimizing costs.


- **RDS Database:**
  An Amazon RDS instance is used for persistent storage, offering automated backups and multi-AZ deployment for fault tolerance and data recovery.

- **S3 Bucket:**
  An S3 bucket stores static assets like images and front-end files, leveraging the scalability and cost-effectiveness of Amazon S3.The S3 bucket connects to your application stack to serve static content, improving performance by offloading these assets from your compute instances

- **IAM Roles and Policies:**
  The architecture enforces security best practices by using IAM roles with minimal permissions, ensuring that each service only has access to the resources it needs. An IAM policy is attached to the role, granting the necessary permissions to interact with S3 or other AWS services.

- **Monitoring and Alarms:**
  CloudWatch monitors critical metrics, triggering alarms to automatically scale the application based on CPU usage. This setup ensures resource efficiency while keeping the application responsive under varying loads.

- **KMS Key:**
  A KMS key is used for encryption of sensitive data, ensuring data security both at rest and in transit.

- **Backup Plan and Vault:**
  An AWS Backup Plan automates backups for critical resources, such as the RDS database. The backups are stored in a backup vault, ensuring data retention and compliance.

### Scalability and Efficiency

- **Decoupled Components:**
  The architecture follows best practices by decoupling compute, storage, and network resources. Each layer is independent, allowing changes and updates to be made without affecting the entire system.

### DevOps Best Practices

- **Infrastructure as Code (IaC):**
  The entire architecture is defined using Terraform, enabling consistent, repeatable deployments across environments. Version control for the code ensures traceability and allows for collaboration and rapid iteration.

- **Automation and CI/CD:**
  Automated deployment pipelines integrate seamlessly with the Terraform scripts, enabling continuous delivery of new features while maintaining stability.

- **Security and Compliance:**
  The architecture follows AWS security best practices, including isolated subnets, least privilege IAM roles, and encrypted storage using KMS. Security groups enforce tight control over inbound and outbound traffic.

Cost Considerations
EC2 Instances:
The Auto Scaling Group uses t2.micro instances, which are cost-effective for small to moderate workloads. Auto Scaling helps minimize costs by adjusting the number of instances based on demand.

RDS Instance:
The RDS instance uses the db.t2.micro class, which is suitable for development and testing. For production environments, consider upgrading to a higher instance class for better performance and cost management.

S3 Bucket:
The cost for S3 storage is based on the amount of data stored and the number of requests made. Using S3 for static assets leverages its pay-as-you-go model, which helps manage costs efficiently.

NAT Gateway:
NAT Gateways incur costs based on the amount of data processed and the hourly usage. To reduce costs, ensure that only necessary traffic is routed through the NAT Gateway and consider alternatives like VPC endpoints for certain services.

Load Balancer:
The Application Load Balancer charges are based on the number of hours it runs and the amount of data processed. For cost optimization, use it efficiently and review usage patterns to avoid over-provisioning.

CloudWatch:
CloudWatch monitoring and alarms are billed based on the number of metrics, dashboards, and alarm evaluations. Efficient use of CloudWatch can help manage costs while maintaining visibility into application performance.

Backup and Storage:
AWS Backup costs are based on the amount of data backed up and the retention period. Regularly review backup policies and retention settings to ensure they align with your cost management strategy.

IAM Roles and Policies:
There are no direct costs associated with IAM roles and policies, but improper configuration can lead to security risks that might incur indirect costs through data breaches or compliance violations.

Monitoring and Optimization:
Regularly monitor AWS usage and costs using AWS Cost Explorer and set up budgets to track spending. Optimize resource usage by adjusting instance types and utilizing reserved instances or savings plans for predictable workloads.

Rightsizing Resources:
Continuously evaluate and adjust resource sizes based on actual usage. Use CloudWatch metrics and other monitoring tools to make informed decisions about scaling and instance types.

Cost Alerts:
Set up cost alerts in AWS to notify you when spending exceeds predefined thresholds. This helps in proactively managing costs and avoiding unexpected charges.

aws_sns_topic: Defines an SNS topic where messages can be published.
aws_sns_subscription: Subscribes an endpoint (e.g., email) to the SNS topic.
aws_cloudwatch_metric_alarm: Uses SNS to send notifications when the alarm state is triggere

Then to apply all this changes;Run the following command.

Terraform init

Terraform plan

terraform apply 



### Conclusion

This infrastructure balances scalability, cost efficiency, efficiency, and best practices by leveraging AWS-managed services and Terraform automation. It provides a robust foundation for running web applications that can handle production-level traffic while ensuring operational simplicity and cost efficiency.


