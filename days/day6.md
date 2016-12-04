
## Objectives
Setup Jenkins Continuous Integration Server and implement the following stories:
* Can get feedback on failed tests in CI
* Can update latest version in production by push of a button

## Steps
**Setup Ubuntu**
1. We have created a Ubuntu instance in the Advania Cloud, pick a instance by adding your name and email to the following [Google Spreadsheet](https://docs.google.com/spreadsheets/d/1ubEZNa1ehYiifsu0hfZSLcSxEythk_5Xr03iH0qaOxI/edit?usp=sharing).  
Optionally you can upload you ssh-key for authorization.
2. This instance does not have Docker preinstalled so you need to set up Docker. We recommend you follow this [tutorial](https://docs.docker.com/engine/installation/linux/ubuntulinux/) to the subheader "Adjust memory and swap accounting"

**Install Jenkins**
1. Get the this [Docker Container] (https://hub.docker.com/r/library/jenkins/) on Ubuntu machine and run.
Tips:
    * You need to create a folder for the jenkins data on the host and give the jenkins user access to that folder.
    * The jenkins user has the uid 1000
    * Use the command chown to change ownership of a folder.
    * Port 32700-32800 are open in the Advania firewall
    * User --detach or -d to run docker in detached mode.
    * Recommended to assign name to the docker container using the --name flag.
2. If everything is configured correctly you should get the installation wizard from Jenkin by typing in your machines ip address and chosen port:
[ip address]:[port]
3. Follow the Jenkins wizard. You should install suggested plugins.
4. We recommend that you install the Green Balls plugin.

**Create Commit Stage project**
1. Create Jenkins Project that gets the latest version of from git and runs the build script.
2. Add post build step that published the unit test results.

**Create Deployment project**
1. Create new project that deploys to AWS.
Tips:
    * You need to upload you public key to the host
    * You need to give jenkins user access to the public key
    * You should upload a build script on the AWS and run from Jenkins.
    * Try using the flag -y when using ssh. If that does not work you need to login the the Jenkins container and run ssh to AWS once to accept the credentials.

**Schedule**
1. Configure the commit step to run on every change
2. Run the deployment step automatically if the commit steps succeeds.

**Optional**
* Setup up pipeline plugin to view the steps.
* Create build steps using jenkinsfile and use pipelines.
* Create new AWS automatically as part of the deployment step using the Provision script from day 3. (later we will run automatic tests on this machine).
* Shut down AWS machine in separate step.


## Useful commands
chown -R 1000 [folder]

**Stop docker and remove docker**
docker kill/stop [#name]
docker rm [#name]

**Log in to a running to container instance**
docker attach [container name or id]  
*to get another bash instance you have to use the following command*  
docker exec -i -t [container name] /bin/bash
