# Python-Flask App for Stocks Analysis
This a python application that uses:

      - Flask server as rest service
      - qyandl module for fetching real-time stocks data
      - pygal to render graph data
      - boorstrap for modularization of web application
      - docker-compose for managing the container
      - yml for kubernetes configuration deployment
      - html for web development
 
 ## API
 The entrypoint for the python api is ```/app/main.py```
 The versions are : Python 2.7, flask: 0.10.1.
The app fetches realitime WIKI stocks data using python quandl module and renders pygal graph for visual appeal. 

Please note that the quandl code in `main.py` needs to be restored for the API to work. 

Extract from main.py:
```
if __name__ == '__main__':
    app.run(host = '0.0.0.0', debug = False, port=int(os.getenv('PORT', '5000')))
```

In order to expose the app to extrenal IP and enable app accessible through any machine, it is import to follow the above setup. There might be workaround, but I fould that only way its working during my project work. 

## Automation:
  A Makefile is provided for the automation of the task:

  - Build the image and deploy to kubernetes cluster already up and running:
        ``build-deploy-all``
  
  - Clean the Docker image and processes:
        ```docker-clean```

## docker-compose
Deploying the service to kubectl cluster will need docker-compose.
Here is the installation instruction for docker-compose:
```
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
 ## Kubernetes Deployment
Once the kubectl cluster is up and running. Us the following command to deploy the service to the cluster:
```
docker-compose up -d
kubectl apply -f kubernetes/flask-web-ui.yml
```

## yaml Configuration:
The configuration of the service is controlled from the `kubernetes/flask-web-ui.yml`
An extract of the file:
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: web-ui
spec:
  replicas: 3
  template:
    metadata:
      labels:
        role: web-ui
    spec:
      containers:
      - name: web-ui
        image: cloudmesh/stocks:latest
        ports:
        - containerPort: 5000
```
The noteworthy specs are `replicas` and `port`. Flask by default hits port 5000 so the same is retained in the deploment as well.

VIDEO DEMONSTARTION: 

A video demonstartion of the project is provided [video](https://www.youtube.com/watch?v=ilQ54AXBCEM).

ACKNOWLEDGEMENT:

Found this useful repo by Scott S Lowe on Flask-python api deployment to kubectl cluster. It helped me understand the delpoyment structure better. 
```
https://github.com/scottslowe/flask-web-svc
```
