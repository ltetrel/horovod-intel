#!/bin/bash

echo "Removing previous container"
yes | rm *.simg

echo "Building docker image..."
sudo docker build --no-cache -t $1 -f Dockerfile.gpu .

echo "Converting to singularity..."
sudo docker run -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd):/output --privileged -t --rm singularityware/docker2singularity --name $1 $1

echo "Deleting none images"
sudo docker rmi --force $(docker images | grep none | awk '{ print $3; }')

echo "Transferring image to the server..."
rsync -rlt --info=progress2 $1.simg stark.criugm.qc.ca:/data/cisl/CONTAINERS
rsync -rlt --info=progress2 $1.simg cedar.computecanada.ca:~/projects/rrg-pbellec/CONTAINERS
