1. Describe the CI/CD process for the project in terms of workflow diagrams. You can provide a general, high-level workflow diagram that illustrates the typical CI/CD process without going into the specific implementation details.

 ![image](https://github.com/user-attachments/assets/8c5222a2-d0c1-4a21-977e-7d485c16c78e)

Description:

1. Developer Writes Code & Raises PR:
  - Developer: Writes new code or modifies existing code in a feature branch.
  - Code Review: The developer raises a Pull Request (PR) to merge their changes into a target branch (e.g., develop or main). The PR is reviewed by peers for code quality, adherence to coding standards, and 
   potential issues.
2. Merge & Trigger Pipeline:
  - Merge PR: Once the PR is approved, it gets merged into the target branch.
    This merge action automatically triggers the CI/CD pipeline configured for the project.
Pipeline Stages:
3. Install Tools:
  - Description: Set up the build environment with necessary tools.
  - Actions:
       - Use a script or configuration file (e.g., Ansible, Chef, Puppet) to install tools like Java JDK, Node.js, Docker, Maven, etc.
       - Ensure version consistency across different environments.
4. Install Dependencies:
  - Description: Download and install all project dependencies.
  - Actions:
       - Use package managers such as npm for Node.js, pip for Python, or Maven for Java to install required libraries and frameworks.
       - Create a clean environment for each build to ensure no leftover dependencies affect the process.
5. Run Test Cases:
  - Description: Execute automated tests to validate the code.
  - Actions:
       - Unit Tests: Check individual components for correctness using frameworks like JUnit, NUnit, or Mocha.
       - Integration Tests: Validate interactions between components.
       - Code Coverage: Measure how much of the codebase is covered by tests.
6. Run SonarQube Analysis:
  - Description: Perform static code analysis for quality and security.
  - Actions:
       - Use SonarQube to scan the code for code smells, bugs, and vulnerabilities.
       - Generate detailed reports and ensure the code meets defined quality gates.
7. Run Trivy File System Scan:
  - Description: Scan the file system for vulnerabilities and compliance issues.
  - Actions:
       - Use Trivy to scan for known vulnerabilities in OS packages, application dependencies, and configuration files.
       - Review and address any identified issues before proceeding.
8. Build App:
  - Description: Compile the source code into a deployable artifact.
  - Actions:
       - Use build tools like Maven, Gradle, or npm to compile the code.
       - Generate artifacts such as JAR, WAR, or binary files.
9. Publish Artifacts to Nexus:
  - Description: Store the built artifacts in a repository manager.
  - Actions:
       - Upload artifacts to Nexus Repository Manager.
       - Version control the artifacts for traceability and rollback capabilities.
10. Build Docker Image:
  - Description: Package the application into a Docker image.
  - Actions:
       - Use a Dockerfile to define the environment and dependencies.
       - Build the Docker image and tag it with appropriate version numbers.
11. Scan Docker Image:
  - Description: Ensure the Docker image is secure and free of vulnerabilities.
  - Actions:
       - Use tools like Trivy, Clair, or Aqua Security to scan the Docker image.
       - Address any vulnerabilities before proceeding.
12. Deploy to Kubernetes:
  - Description: Deploy the Docker image to a Kubernetes cluster
  - Actions:
       - Use Kubernetes manifests or Helm charts to define the deployment.
       - Deploy the application to the cluster, managing pods, services, and ingress rules.
13. Functional Testing:
  - Description: Validate the application’s functionality in the deployed environment.
  - Actions:
       - Use tools like Selenium, Postman, or Cucumber to run automated functional tests.
       - Ensure the application meets all functional requirements and behaves as expected.
14. Penetration Testing:
  - Description: Perform security testing to identify potential vulnerabilities.
  - Actions:
       - Use tools like OWASP ZAP to conduct penetration testing.
       - Identify and mitigate any security vulnerabilities found.

================================================================================

3. What tools are you going to use for monitoring and alerting purposes, and how they are going to be integrated with the infrastructure?
You can suggest a few popular monitoring and alerting tools, and provide a general overview of how they could be integrated with the infrastructure, without getting into the technical implementation.

We are going to use ultimate monitoring tools Prometheus & Grafana.

Introduction: 

In our project, we implemented a comprehensive monitoring solution using Prometheus and various exporters to ensure the reliability and performance of a web application hosted on AWS EC2 instances. This setup includes Node Exporter for hardware and OS metrics, Blackbox Exporter for probing endpoints, and Alertmanager for handling alerts. Gmail integration was also configured to receive notifications for critical alert.

Components: 

1. EC2 Instances:
  • Instance 1: Hosts the web application, Node Exporter, and Nginx.
  • Instance 2: Hosts Prometheus, Blackbox Exporter, and Alertmanager.
2. Prometheus: Centralized monitoring tool for collecting and querying 
metrics.
3. Node Exporter: Collects hardware and OS-level metrics from the web 
server.
4. Blackbox Exporter: Probes endpoints to monitor uptime and response 
time.
5. Alertmanager: Manages alerts sent by Prometheus based on defined 
rules.
6. Gmail Integration: Sends email notifications for critical alerts.

These all above components are integrated with our infrastructure and we have configured the metrics when alerts should be triggered through the mail that is given below:

Alerts:

1. InstanceDown: Triggers when an instance is down for more than 1 
minute.
2. WebsiteDown: Triggers when the website is down for more than 1 
minute.
3. HostOutOfMemory: Triggers when available memory is less than 25% 
for more than 5 minutes.
4. HostOutOfDiskSpace: Triggers when the root filesystem has less than 
50% available space.
5. HostHighCpuLoad: Triggers when CPU load is above 80% for more than 
5 minutes.
6. ServiceUnavailable: Triggers when the node exporter service is 
unavailable for more than 2 minutes.
7. HighMemoryUsage: Triggers when memory usage exceeds 90% for 
more than 10 minutes.
8. FileSystemFull: Triggers when any filesystem has less than 10% available 
space for more than 5 minutes

For Above process we have allowed necessary ports in the respective security group:

• Prometheus: 9090
• Alert manager: 9093
• Blackbox Exporter: 9115
• Node Exporter: 9100
• Email transmissions: 587

I would like to suggest some monitoring and alerting tools which are given below:

1. Amazon CloudWatch:

Purpose: CloudWatch is a native AWS service that monitors AWS resources and the applications you run on AWS in real-time. It can collect and track metrics, monitor log files, and set alarms.

Integration:
 
 EC2 Instances: CloudWatch can be used to monitor CPU usage, memory, disk I/O, network traffic, etc.
 EKS Cluster: CloudWatch integrates with EKS to monitor Kubernetes pods, services, and nodes.
 API Gateway & RDS: It can monitor API Gateway requests and database performance, allowing you to set alarms for unusual activities or thresholds.
 CloudWatch Alarms: These can be configured to trigger notifications via Amazon SNS (Simple Notification Service) if certain thresholds are breached (e.g., high CPU usage, increased error rates).

2. SolarWinds :

Overview: 

Network Performance Monitor (NPM):
Purpose: Monitors the performance and availability of network devices and interfaces.
Integration: Can be used to monitor AWS network components like VPN connections, Direct Connect, or any on-premise networking that interfaces with your cloud environment.

Server & Application Monitor (SAM):
Purpose: Monitors server health, application performance, and service availability.
Integration: Can be deployed to monitor EC2 instances, databases, and microservices within your AWS environment.

Log & Event Manager (LEM):
Purpose: Provides centralized log collection, real-time monitoring, and automated responses to security events.
Integration: Can aggregate logs from AWS services like CloudTrail, VPC Flow Logs, or EC2 instance logs for security monitoring and compliance.

Database Performance Analyzer (DPA):
Purpose: Monitors and analyzes the performance of databases.
Integration: Can be connected to Amazon RDS instances to provide insights into query performance, wait times, and overall database health.

How SolarWinds Tools Integrate with AWS Infrastructure:

 - SolarWinds Network Performance Monitor (NPM) could monitor the network connectivity between your on-premises data center and your AWS environment, as well as within your VPCs.

 - Server & Application Monitor (SAM) would integrate directly with your EC2 instances, providing detailed monitoring of operating systems, applications, and services running on them.

 - Log & Event Manager (LEM) could be used to centralize log management by collecting logs from AWS CloudTrail, CloudWatch, and EC2 instances. It would then analyze and correlate these logs to detect security 
   threats or operational issues.

 - Database Performance Analyzer (DPA) could be linked with your Amazon RDS instances to track and optimize database performance, ensuring that queries are executed efficiently.
 





