# DEPRECATED ansorio

[![No Maintenance Intended](http://unmaintained.tech/badge.svg)](http://unmaintained.tech/)

### What
DEPRECATED - was a little pet/educational project a few years ago, cleaned this up a little as I deprecate it just in case someone wants to use it still. I removed the EC2 playbooks because of a lack of separation of concerns and there's better ways to get yourself an instance these days. If you want factorio via ansible there seems to be an ansible galaxy role that's several years newer than this is. 

ansorio is an ansible playbook and role for AWS-centric headless server setup of factorio server.

### Why
For me to practice ansible on something useful to me (and others)

### Who
This ansible role is intended for someone familiar with Ansible, AWS and Factorio, to quickly get an instance up-and-running.

Any and all contributions and PRs are welcome! I would love for this to become a community project.

## Current status
This ansible playbook will successfully create a running Factorio server on an AWS EC2 spot instance. Savegames are backed up to a specified S3 bucket, and restored if a server is launched fresh with the same game name.

## To use
1. Install the requirements:
  `pip install -r requirements.txt`
  We recommend using a virtualenv.

1. If you have not already, ensure that you have AWS CLI set up:
  `aws configure`
  and enter the requested values. Recommended to create an IAM user specifically for this task. 

1. Configure the server and game settings
  `cp factorio-config.example.yml factorio-config.yml` and edit the copied file, making any changes desired

1. Run the playbook
  `ansible-playbook ec2-spot.yml`
  This will create the following in AWS specifically for the Factorio server, in order to isolate from other AWS services you may already be running:
    * VPC, subnet, gateway, and route
    * Security Group with access to Factorio server UDP port to everywhere, and SSH from the local IP
    * keypair (from your local ~/.ssh/id_rsa.pub)
    * ec2 spot instance c4.large (at current spot request price, would be $18/mo for 24/7 usage)
  NOTE: this will incur costs in AWS for the EC2 instance, the S3 bucket, and other services; you are responsible for these costs

1. Play Factorio
  The IP address of the factorio server will be shown in the ansible output. Connect to this in-game

1. Terminate the server when done playing
  `ansible-playbook ec2-terminate.yml`

# TODO:

1. Management utilities: manual saves, map generation preview, web-based?
1. Optional DNS registration for server (route53?)
