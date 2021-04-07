# EC2 Deployment

## Dependency

1. nginx: 
   `sudo apt install nginx -y`

2. jdk-11
   `sudo apt install jdk-11 -y`

3. nodejs and npm 

   https://github.com/nodesource/distributions/blob/master/README.md



## Shells

1. `pull-from-github` 

   pulls backend and admin ui code from github.

2. `view-status` 

   checks the backend server running status.

3. `run-backend` 

   will replace the database url in backend code's `application.properties` , then detect and stop existing working process and start a new one.



Run all scripts in above order. Note that maybe need to change the database url in `application.properties` file.



## Config nginx

A example config file is in the `nginx.sites-available/default`, can use this to replace the original one `/etc/nginx/sites-available/default`.



## About backend admin page

```
cd NUS-tour-backend-adminUI
sudo npm install
sudo npm run build:prod
```

The example nginx config file will map `/admin` url to this `./dist` folder.