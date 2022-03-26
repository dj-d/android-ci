<a href="https://hub.docker.com/repository/docker/grosa1/ionic-android-ci">
        <img src="https://img.shields.io/docker/pulls/grosa1/ionic-android-ci.svg?style=flat-square&color=brightgreen&logo=docker&logoColor=white"
            alt="Pulls"></a> 

## Docker Image for Ionic Android CI
Docker image based on seanghay/android-ci for Ionic Android CI pipelines.

## Latest version:
```sh
docker pull djalba98/ionic-android-ci:latest
```

## Usage in GitLab CI

```
image: djalba98/ionic-android-ci
    
stages:
    - build

assembleDebug:
    stage: build
    script:
        - npm install
        - ionic build
        - jetify
        - cordova-res android --skip-config --copy
        - ionic cap sync --prod android
        - cp google-service.json platforms/android/app/.
        - ionic cap build android
        - cp platforms/android/app/build/outputs/apk/debug/app-debug.apk app-debug.apk
    artifacts:
        paths:
            - app-debug.apk
        expire_in: 3 day
    only:
        - master
```
