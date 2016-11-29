# Day 3 - Provision test/production environments.

## Objectives

* To be able to run application in an EC2 instance or Digital Ocean hosted linux machine.

## Steps


### Provision production-like environment spike

Number one: Log carefully all steps you take. I recommend creating a text document
and copying/pasting all commands you make into that document. If you forget to log
a significant command, you can access it later using the "history" command.

Set up your AWS account. Follow instructions here:

[Get set up for AWS ECS](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/get-set-up-for-amazon-ecs.html)

Install amazon aws command line. Instructions here:
[aws cli](https://aws.amazon.com/cli/?sc_channel=PS&sc_campaign=acquisition_ND&sc_publisher=google&sc_medium=command_line_b&sc_content=aws_cli_bmm&sc_detail=%2Baws%20%2Bcli&sc_category=command_line&sc_segment=161194456241&sc_matchtype=b&sc_country=ND&s_kwcid=AL!4422!3!161194456241!b!!g!!%2Baws%20%2Bcli&ef_id=VZ6aFgAABJZt52AR:20161129143605:s)

Login to aws. You must have your AWS credentials first.

```
aws configure
```

Command line will ask you for your credentials. You can access this from the aws web console
console.aws.amazon.com -> "My Security Credentials"


When up and running with AWS, and you can access the management console web UI,
follow this tutorial to the subheading "Deploying Docker containers on ECS"
(finish the steps above)

http://www.ybrikman.com/writing/2015/11/11/running-docker-aws-ground-up/#installing-docker

Exchange the name/tag of the docker image used in the tutorial with the name of your docker image.


You will also have to run the postgres docker image on your server. At this point you should
have a running production-like environment on AWS EC2.

At this point you should have the chat application running on an EC2 instance.

### Provision production-like environment script

Apart from login and ssh keys generation, all the steps above are scriptable.
Create a script (bash, make, powershell), which allows you to provision a new
server automatically. AWS CLI allows you

Create a folder named "provisioning" in the project and store this script there.




