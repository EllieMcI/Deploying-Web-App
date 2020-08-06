# Deploying Web App

## Overview 

This repo is a sample illustrating how to do Tesnroflow browser based and server side inference.

In [backend](backend), is the server side inference code written in Python and served with FastApi.
In [frontend](frontend), is the browser based inference code written in Typescript / React 

The tensorflow artifacts should be served at [here](backend/artifacts).
A sample artifact can be found in the repo [releases](https://github.com/reshamas/deploying-web-app/releases/tag/1.0.0-tfjs) 

## Demo 

Server Side Inference : [Heroku](https://manning-deploy-imagenet.herokuapp.com/)
Browse Based Inference: [Github Pages](https://reshamas.github.io/deploying-web-app/)


![Demo](assets/demo.gif)


## Setup



## Converting tensorflow model

It is strongly recommended to create a separate environment for `tesnorflowjs`

Installing tensorflowjs 
``` 
pip install tensorflowjs==2.1.0
```

Converting keras model located at `artifacts/model_tf_keras.h5` and saving to `artifacts/model_tfjs`
The `99999999` indicates that the model file should be split to 100 MB partitions

```
tensorflowjs_converter \
--input_format=keras \
--output_format=tfjs_layers_model \
--split_weights_by_layer \
--weight_shard_size_bytes=99999999 \
artifacts/model_tf_keras.h5 artifacts/model_tfjs

```


## Local Deployment

```
docker build -t app 
docker run -p 8000:8000 -t app 
```

If you want to develop outside docker,
python (backend)
```
conda create -n dl_env python=3.7 
pip install -r backend/requirments.txt
```

frontend
```
yarn 
```


## Server Deployment

This app is deployed at Heroku.
Here are the steps for mac

Setup 
``` 
brew tap heroku/brew && brew install heroku
heroku login
heroku container:login
```

Replace `APP_NAME` with something unique
```


APP_NAME="manning-deploy-imagenet"
heroku create $APP_NAME

heroku container:push web --app ${APP_NAME}

heroku container:release web --app ${APP_NAME}
heroku open --app $APP_NAME
heroku logs --tail --app ${APP_NAME}
```

## Browser Deployment
The app is also deployed as a static page on github pages. 
Forks of this repo will automatically deploy to your github pages.

Refer to [push.yaml](.github/workflows/push.yml) for the automated steps.


In [package.json](frontend/package.json), replace "homepage" with the url for the base url your app will be hosted.
If the app is deployed on github, then the url should be `https://{github user id}.github.io/{repo name}`


## Customizing
In order to prevent most frontend changes, most of the text and options are configured in this [config.yml](config.yaml).

The options that you will need to change are 
`url_server`: url for server based inference 
`url_browser`: url for where tfjs model is stored


