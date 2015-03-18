# Ansible Scalable and Load Balanced (SLB) Tomcat

An ansible playbook to deploy on Amazon AWS a scalable set of instances of Apache Tomcat 7 container proxied via a “load-balancing purpose” instance driven by HAProxy, as a bonus I  Nagios3 on all the instance, so even an example of infrastructure monitoring is given.

So the ansible playbook will launch:

* 2 EC2 micro instances tagged as *tomcat* 
    * Security Group with port 8080 and 22
    * Tomcat 7 installed via apt
     
* 1 EC2 micro instances tagged as *harpoxy* 
    * Security Group with port 80, 8888, 1936 and 22
    * HAProxt and Socat installed via apt
    * the instance will proxy and balance (using Round-Robin strategy) from port 8888 to the Tomcat instances 
    * an HAproxy amdin interface is available on port 1936

## Prerequisites
* [Ansible](http://docs.ansible.com/intro_installation.html)
* an account on [Amazon AWS](http://aws.amazon.com/) and a corresponding IAM role

## Configuration

###1. AWS Credentials
Export system env variables for Amazon AWS IAM role credentials like the following:

	export ANSIBLE_HOST_KEY_CHECKING=False
	export AWS_ACCESS_KEY_ID='AK123'
	export AWS_SECRET_ACCESS_KEY='abc123'

or put the previous commands in a file (e.g. secret_iam.env) and launch form terminal:

 	source secret_iam.env

###2. AWS regions, vpc, subnets 

Fill the configuration file under ec2/var/main.yml

    keypair: <<your keipair name>>
    region: us-east-1
    instance_type: t2.micro
    image: ami-9eaa1cf6 # Ubuntu Server 14.04 LTS (HVM), SSD Volume Type
    tag_ec2_current: launched
    subnets:
    - subnet-fbe306d0
    vpc_id: vpc-9ed752fb

All the ec2 instances will be configured starting from this file.    

## Running

Now you are able to launch the playbook

    ansible-playbook -i ec2.py --private-key=<path_to_your_aws-pem_key ec2-lauch.yml  -v
    
## Notes

* Templates and palybooks are massively compiled from variables yaml files. Remeber to check them before installing your inventory.

## Known Issues

* Launching the playbook for the first time may fail resulting in *AnsibleUndefinedVariable* for some values of ec2 tags (*ec2_tag_Name*). Running the same for a new attempt will succeed silently.