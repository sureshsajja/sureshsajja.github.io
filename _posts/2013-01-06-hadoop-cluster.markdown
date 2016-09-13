---
layout: post
title: Setup Hadoop cluster on Amazon EC2 using Apache Whirr
description: Setup Hadoop cluster on Amazon EC2 using Apache Whirr
comments: true
categories:
- Hadoop
tags:
- Hadoop
- Whirr
author: sureshsajja
---

### Hadoop cluster
This post will be a guide to set up Hadoop to run on a cluster of machines using Apache Whirr. Using Apache Whirr is one of the easy ways to set up cluster.
Whirr provides: (Ref: [Apache Whirr](http://whirr.apache.org/))

* A cloud-neutral way to run services. You don't have to worry about the idiosyncrasies of each provider.
* A common service API. The details of provisioning are particular to the service.
* Smart defaults for services. You can get a properly configured system running quickly, while still being able to override settings as needed.

Initially, we need a Linux system to start setup of cluster. I chose t1.micro instance of Amazon EC2.

* Download, extract Whirr 0.8.1 tar ball from [here](http://www.apache.org/dyn/closer.cgi/whirr/) to Linux machine
* Similarly Download Hadoop 1.1.1 tar ball from [here](http://hadoop.apache.org/releases.html#Download)
* Install Sun JDK, if required


Series of commands executed at my end

{% gist 0672ea80d1752df7eda58305a5f324f8 %}

setup all required environment variables

```shell    
export JAVA_HOME=$HOME/jdk1.7.0_10/
    
export HADOOP_PREFIX=$HOME/hadoop-1.1.1
    
export WHIRR_HOME=$HOME/whirr-0.8.1
    
export PATH=$JAVA_HOM/bin:$HADOOP_PREFIX/bin:$WHIRR_HOME/bin:$PATH
```


We need AWS key and secret key to access cloud services. You can obtain them from [here](https://portal.aws.amazon.com/gp/aws/securityCredentials)
Add these security credentials as environment variables for later use.

```java    
export AWS_ACCESS_KEY_ID=<<KEY>>
export AWS_SECRET_ACCESS_KEY=<<SECRET_KEY>>
```

Whirr requires you to create an RSA public/private key pair that will be used to ensure that access to VMs is secured. Run the following command and make sure defaults are accepted.

```    
ssh-keygen -t rsa
```

### Configuring whirr

There are few template configurations available at $WHIRR_HOME/recipes. we can modify one of them or create a new one with the following values

```    
#For description of each property see here http://whirr.apache.org/docs/0.8.1/configuration-guide.html
    
whirr.cluster-name=hadoopsuresh
whirr.instance-templates=1 hadoop-namenode+hadoop-jobtracker,1 hadoop-datanode+hadoop-tasktracker
whirr.provider=aws-ec2
whirr.identity=${env:AWS_ACCESS_KEY_ID}
whirr.credential=${env:AWS_SECRET_ACCESS_KEY}
whirr.private-key-file=${sys:user.home}/.ssh/id_rsa
whirr.public-key-file=${whirr.private-key-file}.pub
whirr.hadoop.version=1.1.1
whirr.hadoop.tarball.url=http://www.us.apache.org/dist/hadoop/common/hadoop-1.1.1/hadoop-1.1.1-bin.tar.gz
whirr.hardware-id=t1.micro
whirr.image-id=us-east-1/ami-1624987f
whirr.location-id=us-east-1b
whirr.java.install-function=install_oracle_jdk7
```

Now we are set to launch our cluster, use the following command to launch

```    
whirr launch-cluster --config $WHIRR_HOME/recipes/myhadoop.properties
```

Hopefully, output should be clean and make sure that cluster is set up successfully.

Add the following to environment variables to set the Hadoop configuration to use the cluster.

```    
export HADOOP_CONF_DIR=/home/ec2-user/.whirr/hadoopsuresh
```


### Run a proxy:
As a security measure, many of the ports on Hadoop cluster in EC2 are closed and Whirr automatically configures a proxy server which we can run to access Hadoop cluster.
Run the following command in a new terminal.

```    
sh /.whirr/<<Cluster-Name>>/hadoop-proxy.sh
```


### Run a Test
Now, our cluster is ready to use, run one of the example programs provided with Hadoop ($HADOOP_PREFIX/hadoop-examples-1.1.1.jar)


