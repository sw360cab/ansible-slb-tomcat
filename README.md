Ecample 

	export AWS_ACCESS_KEY_ID='AK123'
	export AWS_SECRET_ACCESS_KEY='abc123'

oppure
 	source secret_iam.env

----

	cd to folder

----

	ansible-playbook -i ec2.py --private-key=~/.ssh/pem/wimlabs.pem ec2-lauch.yml  -v