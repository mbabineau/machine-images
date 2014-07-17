## Packer

Packer (http://packer.io) is an open source tool for creating machine images.

We use Packer to create Amazon Machine Images (AMIs) from which our EC2 instances will be launched.

### Installation

Instructions here: http://www.packer.io/docs/installation.html

### Usage

To build an AMI, make sure your keys are set:
```
$ export AWS_ACCESS_KEY_ID="<your_access_key>"
$ export AWS_SECRET_ACCESS_KEY="<your_secret_key>"
```

Then, `cd` into the target image build directory and run `packer build <machine_template>`:
```
$ cd ubuntu-12.04-docker/
$ packer build packer.json
```

Build times are typically 5-15 minutes, and you should see streamed output like this:
```
$ packer build packer.json 
amazon-ebs output will be in this color.

==> amazon-ebs: Creating temporary keypair: packer 53331748-f8b0-34c5-6f92-527505860627
==> amazon-ebs: Creating temporary security group for this instance...
==> amazon-ebs: Authorizing SSH access on the temporary security group...
==> amazon-ebs: Launching a source AWS instance...
    amazon-ebs: Instance ID: i-1278911a
==> amazon-ebs: Waiting for instance (i-1278911a) to become ready...
==> amazon-ebs: Waiting for SSH to become available...
==> amazon-ebs: Connected to SSH!
==> amazon-ebs: Provisioning with shell script: packer.sh
    amazon-ebs: Hit http://archive.ubuntu.com precise Release.gpg
    amazon-ebs: Get:1 http://archive.ubuntu.com precise-updates Release.gpg [198 B]
    amazon-ebs: Hit http://archive.ubuntu.com precise Release
[... REMOVED FOR BREVITY ...]
    amazon-ebs: Setting up git-man (1:1.7.9.5-1) ...
    amazon-ebs: Setting up git (1:1.7.9.5-1) ...
    amazon-ebs: Setting up lxc-docker-0.9.1 (0.9.1) ...
    amazon-ebs: docker start/running, process 11097
    amazon-ebs: Setting up lxc-docker (0.9.1) ...
    amazon-ebs: Processing triggers for libc-bin ...
    amazon-ebs: ldconfig deferred processing now taking place
==> amazon-ebs: Stopping the source instance...
==> amazon-ebs: Waiting for the instance to stop...
==> amazon-ebs: Creating the AMI: ubuntu-12.04-docker-2014-03-26T18-07-04Z
    amazon-ebs: AMI: ami-863b53b6
==> amazon-ebs: Waiting for AMI to become ready...
==> amazon-ebs: Adding tags to AMI (ami-863b53b6)...
    amazon-ebs: Adding tag: "OS_Flavor": "Ubuntu"
    amazon-ebs: Adding tag: "OS_Version": "12.04 LTS"
==> amazon-ebs: Terminating the source AWS instance...
==> amazon-ebs: Deleting temporary security group...
==> amazon-ebs: Deleting temporary keypair...
Build 'amazon-ebs' finished.

==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:

us-west-2: ami-863b53b6
```

To copy the AMI to another region, just use the AWS Console or [awscli](http://aws.amazon.com/cli/):
```
$ aws ec2 copy-image --source-image-id ami-863b53b6 --source-region us-west-2 --region us-east-1
```
