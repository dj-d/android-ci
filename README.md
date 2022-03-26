<a href="https://hub.docker.com/repository/docker/djalba98/ionic-android-ci">
        <img src="https://img.shields.io/docker/pulls/djalba98/ionic-android-ci.svg?style=flat-square&color=brightgreen&logo=docker&logoColor=white"
            alt="Pulls"></a> 

## Docker Image for Ionic Android CI
Docker image based on seanghay/android-ci for Ionic Android CI pipelines.

## Latest version:
```sh
docker pull djalba98/ionic-android-ci:latest
```

## Usage in GitLab CI

```
stages:
  - dependencies
  - build

install_dependencies:
  stage: dependencies
  
  image: node:16-alpine
  
  script:
    - npm install
  
  only:
    - master
  
  cache:
    key:
      files:
        - package-lock.json
    
    paths:
      - node_modules

build_image:
  stage: build
  
  image: djalba98/ionic-android-ci
  
  script:
    - ionic cap add android
    - jetify
    - cordova-res android --skip-config --copy
    - ionic cap sync --prod android
    - cp google-services.json android/app/.
    - cd android
    - ./gradlew assembleDebug
    - cd ..
    - cp android/app/build/outputs/apk/debug/app-debug.apk .
  
  only:
    - master
  
  cache:
    key:
      files:
        - package-lock.json
    
    paths:
      - node_modules
    
    policy: pull
  
  artifacts:
    paths:
      - app-debug.apk
    
    expire_in: 3 day

```
