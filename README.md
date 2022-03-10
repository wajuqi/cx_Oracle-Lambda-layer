# Introduction
This repo provides the instructions and the cx_Oracle layer (python 3.7 version) that can be used as a Lambda Layer on AWS. 
To skip the steps and use the lambda layer directly, download the zip file from https://drive.google.com/file/d/15gYtlo2qzL_OFjICSfNLkNSZ1IFF_1tK/view?usp=sharing.
## About cx_Oracle: 
cx_Oracle is a Python extension module that enables access to Oracle Database. See the cx_Oracle’s documentation. Here are the steps of how to create a cx_Oracle lambda layer (steps referenced from https://github.com/dennisoft/cxoracle).
## Steps to create cx_Oracle layer
 
1. EC2 Instance
    * Launch an Amazon EC2 Instance with Amazon Linux 2 AMI.The reason for using Amazon Linux 2 is that it comes with Python 3.7 available in the repositories.
    * Now you can connect to your Linux Instance using an SSH Client and start building the AWS Lambda Layer.

2. Install and configure the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html).
3. Update the existing system packages and install Python 3.7
   ``` 
    $ sudo yum update -y 
    $ sudo yum install python3 -y
   ```
4. Create a working directory and structure it by creating the "python" and "lib" folders which are required for a Python Layer.

    ```
     $ mkdir ~/my_layer
     $ cd ~/my_layer
     $ mkdir python/ lib/
    ```
5. Install the "cx_Oracle" module using "pip-3.7" under "python/" directory. The function code will access it without additional configuration.

    ```$ pip-3.7 install cx_Oracle -t python/``` 
6. Download the [Oracle Instant Client (version 18.5 Basic Light)](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html) and extract the contents to the "lib/" directory.
    ```
     $ wget https://download.oracle.com/otn_software/linux/instantclient/185000/instantclient-basiclite-linux.x64-18.5.0.0.0dbru.zip -O oracle.zip
     $ unzip -j oracle.zip -d lib/
    ```
7. Install the "libaio" library and copy it to the "lib/" folder of the layer. This library is preinstalled in Amazon Linux 2, if you are using it ycan skip the installation.
    ``` 
    $ (optional) sudo yum install libaio -y
    $ cp /lib64/libaio.so.1 lib/libaio.so.1
    ```
8. Create a ZIP archive with "python/" and "lib/" folders.
    ```
    $ zip -r -y layer.zip python/ lib/
    ```
9. Create a layer using the lambda layer zip file
10. Upload the lambda layer to AWS and add environment variable to the lambda function: 
    ```
    LD_LIBRARY_PATH = /var/lang/lib:/lib64:/usr/lib64:/var/runtime:/var/runtime/lib:/var/task:/var/task/lib:/opt/lib:/opt/python
    ```
 
