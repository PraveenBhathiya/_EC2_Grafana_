# Monitor EC2 Instances Using Grafana and AWS CloudWatch

This project sets up **Grafana** to visualize AWS EC2 metrics using **CloudWatch** as the data source. It helps monitor instance performance (CPU, memory, network, disk, etc.) using interactive dashboards.

## 🎯 Objective

Visualize EC2 instance metrics from AWS CloudWatch in Grafana dashboards for real-time monitoring and analysis.

## 🛠️ Prerequisites

- An AWS account with CloudWatch enabled (by default on EC2).
- IAM credentials with read access to CloudWatch.
- An Ubuntu EC2 instance (or local machine) to run Grafana.

## 📦 Install Grafana

```bash
# Download and install Grafana
wget https://dl.grafana.com/oss/release/grafana_10.4.1_amd64.deb
sudo dpkg -i grafana_10.4.1_amd64.deb
```
# Start Grafana
```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

## 🌐 Access Grafana
* Open your browser:
```http://<Your-EC2-Public-IP>:3000```
* Default login:
  * Username: admin
  * Password: admin

### 🔌 Add CloudWatch as a Data Source

1. Go to Settings > Data Sources > Add data source
2. Select CloudWatch
3. Configure the following:
     * Default Region: Choose the AWS region (e.g., ```ap-south-1```)
     * Authentication Provider:
         * If running Grafana on EC2 with an IAM role, choose ```Credentials file or IAM role```
         * Or provide an Access key and Secret key for a user with ```CloudWatchReadOnlyAccess```
4. Click Save & Test

### 📊 Create or Import EC2 Monitoring Dashboard

#### Option 1: Use a Prebuilt Dashboard
1. Go to Dashboards > Import
2. Use CloudWatch EC2 dashboard ID: 11156
```Link: https://grafana.com/grafana/dashboards/11156/```
3. Select your CloudWatch data source
4. Click Import

#### Option 2: Create Your Own Dashboard
1. Go to Dashboards > New > Add Panel
2. Choose CloudWatch as the data source
3. Set:
  * Namespace: ```AWS/EC2```
  * Metric Name: e.g., ```CPUUtilization```, ```NetworkIn```, ```DiskReadOps```
  * Dimensions: ```InstanceId``` (select specific EC2)
4. Save the panel and repeat for other metrics

### ✅ Common EC2 Metrics

* CPUUtilization
* NetworkIn / NetworkOut
* DiskReadBytes / DiskWriteBytes
* StatusCheckFailed
* MemoryUtilization (only if custom CloudWatch Agent is configured)

### 🔐 Security Recommendations

* Restrict access to port ```3000``` (Grafana) via Security Groups or VPN.
* Use HTTPS and set up authentication (OAuth or Grafana users).
* Do not expose AWS credentials in plain text — prefer IAM roles on EC2.
