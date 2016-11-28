# Day 1

### DOCKER

Get docker up and running on your development environment. Linux is preferred.

https://docs.docker.com/engine/installation/

To check whether you have docker up and running:
```
docker ps
```
If it complains that docker is not running, ensure the service is running by typing
```
sudo service docker start
docker ps
```
If it still complains make sure you setup up a Unix group for the users who should have access to docker without using sudo (This is described in installation instructions above).

* Create an account on [docker](https://www.docker.com/), <yourname> refers to docker username. This is neccessary in order to be able to do docker push and use the public docker registry.

Follow the tutorial here:

https://docs.docker.com/engine/tutorials/dockerizing/

When done with the tutorial, publish your hello world docker image to dockerhub:
* Further information on how to tag and push docker images are [here](https://docs.docker.com/engine/getstarted/step_six/)   
```
docker login
docker push <yourname>/helloworld
```

Now you have your image published on dockerhub.

### Hints
* Docker command order is kill, rm, pull, run. Use --name parameter to make this easy.
* Start with making sure you can execute each step successfully from the command line before integrating into script and testing in build.
