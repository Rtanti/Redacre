# Task 1
## What is needed?
Docker, docker-compose, and Python2 along with pip.

## Notes on Dockerfiles
All 3 containers have their own Dockerfile in their respective directories. On the container, these directories are mounted in case changes need to be made without having to restart the container, such as the requirements.txt file.

For the backend, a simple python2.7 image was used to keep the size small and compatible. 
It comes with pip installed so requires virutally no preparations other than installing the provided requirements. Exposed the necessary ports, and commented out most of the requirements as only 3 were needed.
Some of them had issues being installed as it was trying to search for them on a container volume using the file protocol.
```
FROM python:2.7

RUN mkdir /srv/redacre
COPY ./ /srv/redacre/api/
WORKDIR /srv/redacre/api/

RUN pip install -r requirements.txt

EXPOSE 80
EXPOSE 5000

CMD ["python", "/srv/redacre/api/app.py"]`
```

For the frontend, the node docker image was used again to keep things simple. No alterations were made to existing files. 
```
FROM node

RUN mkdir -p /srv/redacre/frontend
COPY ./ /srv/redacre/frontend/
WORKDIR /srv/redacre/frontend/
RUN npm install
RUN npm run build

EXPOSE 8080
EXPOSE 3000

CMD ["npm", "start"]
```

For the proxy, we used haproxy to simply forward requests from the frontend to the backend using the official haproxy docker image. All that is needed is to edit the configration file `haproxy.cfg` to one's needs.
```
FROM haproxy:2.3

COPY haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg

CMD haproxy -c -f /usr/local/etc/haproxy/haproxy.cfg -db
```

## Notes on docker-compose
To start the containers, you can run `docker-compose up --build`. This will output to stdout so if you don't want to see the output then add `-d` to the command to run it in detached mode.
The exposed ports `5000,3000,80` from the 3 previous containers were published so that they can be accessed from the docker host.
```
version: '3'
services:
  frontend:
    container_name: "react_frontend"
    build: ./sys-stats/
    image: "redacre/frontend:latest"
    ports:
      - "3000:3000"
    volumes:
      - ./sys-stats:/srv/redacre/frontend
  backend:
    container_name: "python_backend"
    image: "redacre/backend:latest"
    build: ./api/
    ports:
      - "5000:5000"
      - "80:80"
    volumes:
      - ./api:/srv/redacre/api
  proxy:
    container_name: "haproxy"
    build: ./proxy/
    image: "redacre/proxy:latest"
    volumes:
      - ./proxy:/srv/redacre/proxy
```

# Task 2 Unavailable
Unfortunately for some reason both my AWS accounts were suspended and I heard from their support team today and still in the process of getting them enabled again. 

# Task 3
## What is needed?
Kubernetes, docker, minikube

## How to
For the frontend, a load balancer service was created along the nodejs app in order to service requests to it, and forward to the necessary ports.
For the backend, a Cluster IP to expose the port within the cluster, and a NodePort to expose it to machines outside of the cluster (such as the internet or host VM).

### Notes
Had to change the image of the frontend to include nginx and remove the haproxy container, as it seems to be the suggested way to implement it. Still in the end I was unable to get it working. Might be due to some port forwarding issues with my virtualbox as I was able to curl the python api from inside my virtual machine. 
Used the `Never` policy for the `imagePullPolicy` since the images are stored locally and not on github. Therefore if you don't set it to Never it will try to pull them from the internet.

## How to run
Run the command `kubectl apply -f .` inside the task3 directory. 
