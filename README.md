# ansorio
Factorio ansible playbook and role for AWS-centric headless server setup

# Current status
This ansible playbook will successfully create a running Factorio server on an AWS EC2 spot instance. Savegames are not persisted to durable storage, so if the instance disappears so does the savegame. 

# To use
1. Install the requirements:
  `pip install -r requirements.txt`

1. If you have not already, ensure that you have AWS CLI set up:
  `aws configure`
  and enter the requested values. Recommended to create an IAM user specifically for this task. 

1. Configure the settings (WIP)
  Settings are currently defaults in the factorio ansible role, and the playbook itself; these should be extracted to a single settings file to be configured for overrides. For now, just edit them in-place.

1. Run the playbook
  `ansible-playbook ec2-spot.yml --extra-vars "local_ip=<your public IP address>"`
  This will create the following in AWS specifically for the Factorio server, in order to isolate from other AWS services you may already be running:
    * VPC, subnet, gateway, and route
    * Security Group with access to Factorio server UDP port to everywhere, and SSH from the local IP
    * keypair (from your local ~/.ssh/id_rsa.pub)
    * ec2 spot instance c4.large (at current spot request price, would be $18/mo for 24/7 usage)
  NOTE: this will incur costs in AWS; you are responsible for these costs

1. Play Factorio
  The IP address of the factorio server will be shown in the ansible output. Connect to this in-game

1. Terminate the server
  `ansible-playbook ec2-terminate.yml`
  NOTE: this does not currently save the game data, if you do not copy the savegame it will be lost. If you do not terminate the server it will continue to accrue AWS costs.

# TODO:

1. Sync savegame from S3 upon initial start (systemd oneshot? which factorio service can depend on so it syncs before starting, should compare file ages so we don't overwrite with old)
1. Extract all settings to a separate file at root of repo for convenience, to override defaults
1. Create service to periodically sync savegames to S3 (systemd timer?). Ensure we're not writing zip files that are being written to to avoid corruption somehow.
1. Management utilities: manual saves, map generation preview, web-based?
1. Optional DNS registration for server (route53?)
