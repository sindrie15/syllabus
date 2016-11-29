# Day 3 - Provision test/production environments.

## Objectives

* To be able to run application in an AWS EC2 instance.

## Steps


### Provision production-like environment spike

Number one: Log carefully all steps you take. I recommend creating a text document
and copying/pasting all commands you make into that document. If you forget to log
a significant command, you can access it later using the "history" command.

Set up your AWS account. Follow instructions here:

[Get set up for AWS ECS](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/get-set-up-for-amazon-ecs.html)

You can skip the last optional step but instead you need to install the [AWS Comman Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)

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
server automatically. One of the things aws ec2 commands allow, are to specify
a script that is run on the newly provisioned server.

Create a folder named "provisioning" in the project and store this script there.




