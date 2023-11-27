# A pipline i generated for a dotnet app
## this pipline have 5 stages at the CI point you can see the build and publish
## after publishing the published dotnet DLL's will containerize and ready to deploy in kubernetes cluster
## there might be some problem in deploy stage (im working on it)
## all of these stages run at one runner based on docker.

### open ci file in gitlab source control

.gitlabci.yaml
```
stages:
  - prepare
  - build
  - publish
  - dockerize
  - deploy

variables:
  DOCKER_REGISTRY: aliakhavan # Replace with your Docker Hub username
  DOCKER_IMAGE_NAME: nopcom   # Replace with your desired Docker image name

prepare:
  stage: prepare
  tags:
  - nop
  script:
    - rm -rf /builds/nop/nopcommerce/Presentation/Nop.Web/obj
    - rm -rf /builds/nop/nopcommerce/Presentation/Nop.Web/bin
    - rm -rf /builds/nop/nopcommerce/Presentation/Nop.Web/Plugins
    - rm -rf /builds/nop/nopcommerce/Presentation/Nop.Web.Framework/obj
    - rm -rf /builds/nop/nopcommerce/Presentation/Nop.Web.Framework/bin

build:
  stage: build
  tags:
  - nop
  script:
    - dotnet build /builds/nop/nopcommerce/Presentation/Nop.Web/Nop.Web.csproj --configuration Release # Other build-related commands

publish:
  stage: publish
  tags:
  - nop
  script:
    - dotnet publish /builds/nop/nopcommerce/Presentation/Nop.Web/Nop.Web.csproj -c Release -f netcoreapp2.2 -r linux-x64 --output publish_output
    - cd /builds/nop/nopcommerce/Presentation/Nop.Web/publish_output/
    - cp -r /builds/nop/nopcommerce/copy-after-publish ./plugins # Copy Plugins folder from copy-after-publish
    - cp -r /builds/nop/nopcommerce/Presentation/Nop.Web/Themes ./Themes # Copy Themes folder from Project

dockerize:
  stage: dockerize
  image: docker:latest
  services:
    - docker:dind
  tags:
  - nop
  script:
    - echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
    - docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$CI_COMMIT_SHORT_SHA .
    - docker build -t nop-comm-v3-1-1 .
    - docker login -u $DOCKER_REGISTRY -p $DOCKER_PASSWORD # Set up Docker Hub credentials in CI/CD settings
    - docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$CI_COMMIT_SHORT_SHA
    

deploy:
  stage: deploy
  tags:
  - nop
  script:
    - kubectl apply -f path/to/deployment.yaml # Replace with the path to your deployment.yaml file
    - kubectl apply -f path/to/service.yaml # Replace with the path to your service.yaml file
```