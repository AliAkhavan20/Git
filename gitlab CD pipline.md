# pipline cd sample 
```
stages:
  - build
  - deploy

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t my-html-app .
  artifacts:
    paths:
      - Dockerfile
      - index.html
  only:
    - main

deploy:
  stage: deploy
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker stop my-html-container || true
    - docker rm my-html-container || true
    - docker run -d -p 8080:80 --name my-html-container my-html-app
  only:
    - main
```