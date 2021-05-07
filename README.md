# Chia AWS Quickstart Cloudformation
Chia-Single-Server-Mining-Template

This template can help you start all the resources you want to play with during single-server mining for Chia coin on AWS!
It will help you create:
1. One vpc
2. One i3.large instance
4. One 500G HHD EBS (not attached)
5. All security group and roles
6. And all the things you do not want to care about

It's just a quick start for AWS miners to quickly start one-server mining environment!

Maybe you can do a POC on this environment.

**But not for production!!!**

Prerequisite:
1. AWS Global account
2. One Key pair ready

Please follow the steps to have a try:
1. Launch this template in your AWS console
2. Switch to us-west-2 (the only supported region for now [05/07/2021])
3. After successful deployment, go to EC2 > Volumes and attach that 500G hhd to your new instance
4. ssh into that new i3 instance
5. type in the following cmd:

#list all the disks

    lsblk

#create file-system in that disk

    sudo mkfs -t xfs /dev/xvdf (disk name might change)

    sudo mkfs -t xfs /dev/nvme0n1

#mount two disks to your instance

    sudo mount /dev/nvme0n1 /tmp1

    sudo mount /dev/xvdf /data1

#change mode for your directory

    sudo chown -R ec2-user.ec2-user /tmp1

    sudo chown -R ec2-user.ec2-user /data1

#download chia-blockchain package and run the .sh

    sudo yum update -y
  
    sudo yum install python3 git -y

    git clone https://github.com/Chia-Network/chia-blockchain.git -b latest

    cd chia-blockchain

    sh install.sh

#go to the virtual environment of python

    . ./activate

#init

    chia init

    chia keys add -f <file> / chia keys generate
  
    chia start farmer
  
#start ploting your disk!

    nohup chia plots create -k 32 -b 8000 -r 2 -n 4 -t /tmp1 -d /data1
  
For more details: https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference

Have fun! :)
