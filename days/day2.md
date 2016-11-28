# Day 2

## Objectives

* To be able to develop tictactoe on top of base application - have a running development environment.
* To be able to package web application into a docker container.
* To be able to use docker compose to run the web application along with postgres container,
without using host port mapping.

## Steps


### Get up and running
* Create a fork from the reference application on github:

https://github.com/hgop/reference-tictactoe/


* Important: Use the fork feature on github.com to create a fork.
* git clone your fork to an appropriate location on your computer.
* Follow instructions in the project's readme.md file to get up and running.

### Build docker container

* Ensure that the built version of the application works.
```
npm run build
cd build
export NODE_PATH=.
node run.js
```

* Create a script around packaging the built application into a docker container. This script needs to do the following:
* - npm build
* - build a docker image which includes the build output - not including node-modules.
* - runs npm install for the server.


https://github.com/KolibriDev/intro-docker


Example script - NOT TESTED. You can use this as scaffolding for your script, but it is NOT complete.
This script tags the docker image with the git commit hash and creates a HTML file which can be accessed
later to retrieve version information.


```
#!/bin/bash

echo Cleaning...
rm -rf ./dist

if [ -z "$GIT_COMMIT" ]; then
  export GIT_COMMIT=$(git rev-parse HEAD)
  export GIT_URL=$(git config --get remote.origin.url)
fi

# Remove .git from url in order to get https link to repo (assumes https url for GitHub)
export GITHUB_URL=$(echo $GIT_URL | rev | cut -c 5- | rev)


echo Building app
npm build

rc=$?
if [[ $rc != 0 ]] ; then
    echo "Npm build failed with exit code " $rc
    exit $rc
fi


cat > ./dist/githash.txt <<_EOF_
$GIT_COMMIT
_EOF_

cat > ./dist/public/version.html << _EOF_
<!doctype html>
<head>
   <title>App version information</title>
</head>
<body>
   <span>Origin:</span> <span>$GITHUB_URL</span>
   <span>Revision:</span> <span>$GIT_COMMIT</span>
   <p>
   <div><a href="$GITHUB_URL/commits/$GIT_COMMIT">History of current version</a></div>
</body>
_EOF_


cp ./Dockerfile ./build/

cd dist
echo Building docker image

docker build -t STUDENTNAME/tictactoe:$GIT_COMMIT .

rc=$?
if [[ $rc != 0 ]] ; then
    echo "Docker build failed " $rc
    exit $rc
fi

docker push STUDENTNAME/tictactoe:$GIT_COMMIT
rc=$?
if [[ $rc != 0 ]] ; then
    echo "Docker push failed " $rc
    exit $rc
fi

echo "Done"
```

### Compose app docker and db docker to run together.

Code from Andri's lecture where he shows usage of docker compose is found here:

https://github.com/KolibriDev/intro-docker

Use this as as reference as to how to do the composing. You will need to modify code in order to use environment
variables to control the postgres connection string.