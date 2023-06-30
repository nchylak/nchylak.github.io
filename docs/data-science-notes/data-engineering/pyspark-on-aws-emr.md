---
layout: default
title: PySpark on AWS EMR
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/pyspark-on-aws-emr/
nav_order: 3
---

# PySpark on AWS EMR

## Create EMR cluster

```bash
aws emr create-cluster \
--name "udacity_cluster" \
--use-default-roles \
--release-label emr-5.28.0 \
--instance-count 3 \
--applications Name=Spark  \
--ec2-attributes KeyName=udacity_kp,SubnetId=subnet-02cc9819c677085ef \
--instance-type m5.xlarge \
--profile default \
--region us-west-2
```

## Modify master node's security group

We need to modify the inbound rules of the master node's security group in order to allow for SSH traffic from your computer.

1. Find the security group of the master mode

   1. Find the cluster ID of the master cluster

   ```bash
   aws emr list-clusters --region us-west-2
   ```

   2. Find its security group `EmrManagedMasterSecurityGroup`

   ```bash
   aws emr describe-cluster --region us-west-2 --cluster-id j-XXXXXXXX
   ```

2. Edit its inbound rules to authorize SSH traffic (port 22) from your computer

   * Protocol: TCP

   * Port range: 22 (port for SSH)

   * Source: My IP (automatically adds your IP address as the source address)

   [Documentation](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-ssh-prereqs.html)

   Or via AWS CLI:

   ```bash
   aws ec2 authorize-security-group-ingress \
   --group-id sg-02bb6f3915cd90c1b \
   --protocol tcp \
   --port 22 \
   --cidr 51.154.21.47/32 \
   --region us-west-2
   ```

## Test connection

1. Find the master node's public DNS

   1. Find the cluster ID of the master cluster

   ```bash
   aws emr list-clusters --region us-west-2
   ```

   2. Find its public DNS name `PublicDnsName`

   ```bash
   aws emr list-instances --cluster-id j-XXXXXXXX --region us-west-2
   ```

2. Connect to it by running in your terminal: 

```bash
sudo ssh -i ~/.ssh/udacity_kp.pem hadoop@ec2-###-##-##-###.us-west-2.compute.amazonaws.com
```

3. `exit` to exit the connection.

[Documentation](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-ssh.html)

## View Spark UI

### Setup a proxy in your browser

#### Setup SSH tunnel to the master node using dynamic port forwarding

```bash
sudo ssh -i ~/.ssh/udacity_kp.pem -N -D 8157 hadoop@ec2-###-##-##-###.compute-1.amazonaws.com
```

If port 8157 is taken, replace 8157 with an unused, local port number.

[Documentation](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel.html)

#### Configure a SOCKS for your browser

1. Add the extension SwitchyOmega to Chrome

2. Create a new profile with name `emr-socks-proxy` and select "PAC profile" as type

3. Save the following script in your new profile:

   ```R
   function FindProxyForURL(url, host) {
    if (shExpMatch(url, "*ec2*.amazonaws.com*")) return 'SOCKS5 localhost:8157';
    if (shExpMatch(url, "*ec2*.compute*")) return 'SOCKS5 localhost:8157';
    if (shExpMatch(url, "http://10.*")) return 'SOCKS5 localhost:8157';
    if (shExpMatch(url, "*10*.compute*")) return 'SOCKS5 localhost:8157';
    if (shExpMatch(url, "*10*.amazonaws.com*")) return 'SOCKS5 localhost:8157';
    if (shExpMatch(url, "*.compute.internal*")) return 'SOCKS5 localhost:8157';
    if (shExpMatch(url, "*ec2.internal*")) return 'SOCKS5 localhost:8157';
    return 'DIRECT';
   }
   ```

4. Enable the `emr-socks-proxy` profile.

[Documentation](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-master-node-proxy.html)

### Access Spark UI

You can access the Spark UI under the URL:

`http://ec2-###-##-##-###.us-west-2.compute.amazonaws.com:18080/`

## Run a Spark script

### Write a Spark script

```python
from pyspark.sql import SparkSession
import pyspark.sql.types as T
import pyspark.sql.functions as F

# your functions

if __name__ == "__main__":
	spark = (SparkSession.builder
         .appName("Connect to redshift")
         .getOrCreate())
  
  s3_file_path = 's3n://my_bucket/path/to/file/file.json'
  df = spark.read.json(s3_file_path)
	
	# your program
	
	spark.stop()
```

### Execute a Spark script

```bash
spark-submit --master yarn path-to-your-script.py
```
