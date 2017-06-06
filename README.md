# ansorio
Factorio ansible playbook and role for AWS-centric headless server setup

# Current status
This ansible playbook will successfully create a running Factorio server on an AWS EC2 spot instance. Savegames are backed up to a specified S3 bucket, and restored if a server is launched fresh with the same game name.

# To use
1. Install the requirements:
  `pip install -r requirements.txt`

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
