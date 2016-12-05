
## Objectives
Setup Jenkins Continuous Integration Server and implement the following stories:
* Can get feedback on failed tests in CI
* Can update latest version in production by push of a button

## Steps
**Setup Ubuntu**  
  1. We have created a Ubuntu instance in the Advania Cloud, pick a instance by adding your name and email to the following [Google Spreadsheet](https://docs.google.com/spreadsheets/d/1ubEZNa1ehYiifsu0hfZSLcSxEythk_5Xr03iH0qaOxI/edit?usp=sharing).  
Optionally you can upload you ssh-key for authorization.

**Install Jenkins**
  1. Install Jenkins following this [tutorial](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)

  2. If everything is configured correctly you should get the installation wizard from Jenkins by typing in your machines ip address and chosen port: [ip address]:[8080]
  3. Follow the Jenkins wizard. You should install suggested plugins.
  4. We recommend that you start by installing the Green Balls plugin.

**Create Commit Stage project**
  1. Create Jenkins Project that gets the latest version of from git and runs the build script.
  2. Later we will add a post build step that published the unit test results.  
  Hint:  
      * You need to install NodeJs and Npm
      https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions
      * User node -v and npm -v to check version
      * Install git
      * You also need to install Docker We recommend you follow this [tutorial](https://docs.docker.com/engine/installation/linux/ubuntulinux/)
      * docker login on jenkins account
      * Restart Jenkins after installation sudo service jenkins restart
      * If you are using private git repository you need to add deploy key to github. (Test the key using: ssh -T git@github.com)

**Create Deployment project**
  1. Create new project that deploys to AWS.
  Hint:
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
