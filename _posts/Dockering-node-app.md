Audience: Backend/Fullstack Engineer looking to explore deployment

Pre-requisite:
 - Nodejs API

Goal: 

## What is Docker?

## Basics of Docker + Resources 

## Creating a dockerfile

## Pushing to Container Registry

// specify . at end so that it can pick dockerfile from current location
docker build -t <IMAGE_NAME>:<OPtIONAL_TAG_NAME> .

docker run -p 49160:8080 -d es-cx-migration-api

 docker ps
 
 docker container stop <container-id>
  
  -- to resolve linux kernel application security issue-
  
 --- add issue link for same--
 
 sudo aa-remove-unknown
 
 docker run -p 49160:3000 -d es-cx-migration-api

 docker logs 6c01e338efcc
 
 curl -i localhost:49160/healthcheck
 
 docker tag es-cx-migration-api:latest gcr.io/qai-bd-qa/es-cx-migration-api
 
 -- permission : Storage Admin --- 
gcloud auth login

gcloud auth configure-docker

 docker push gcr.io/qai-bd-qa/es-cx-migration-api

## What Next?

Using this docker file you can share is easily and deploy to an instance by writing a YAML file
