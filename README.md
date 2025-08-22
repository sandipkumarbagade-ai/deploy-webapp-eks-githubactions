```
CREATE BELOW MENTIONED FILES 

```

```
.dockerignore
```
```
node_modules
npm-debug.log
```

```
Dockerfile
```

```
FROM node:18

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --omit=dev

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]

```



```
package.json
```

```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}

```

```
server.js
```

```
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, HOST, () => {
  console.log(`Running on http://${HOST}:${PORT}`);
});

```

```
deployment.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodejs
  labels:
    app: nodejs
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nodejs
  template:
    metadata:
      labels:
        app: nodejs
    spec:
      containers:
        - name: mycontainer
          image: 048648750621.dkr.ecr.us-east-1.amazonaws.com/github-sample:sample_image
          imagePullPolicy: Always
          ports:
            - containerPort: 8080

```

```
service.yaml
```

```
apiVersion: v1
kind: Service
metadata:
  name: nodejs
spec:
  selector:
    app: nodejs
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer

```

```
FIRST MAKE SURE YOU CREATE EKS CLUSTER NAMED "my-demo-cluster"

CREATE USER WITH ADMINISTRATORACCESS - demouser  
```



```

Create Repository under ECR Registry 

name : github-sample

```


GENERATE 

 AWS_ACCESS_KEY_ID 
 AWS_SECRET_ACCESS_KEY 
 ```


```
add these keys with values in docker project repository

UNDER SECRETS AND VALUES 

```

```
Create Github action flow file 
```

```
 600  git clone <YOUR REPO>>
  601  cd deploy-webapp-eks-githubactions/
  602  git init
  603  git add .
  604  git commit -m "commit 1"
  605  git push
  606  history

```


```
CROSS CHECK DEPLOYMENT AND SERVICE OBJECTS ARE CREATED USING AWS CLI 

kubectl get deplo
kubectl get deploy
kubectl get deploy -o wide
kubectl get po -o wide
kubectl get svc
```

```
CLEAN UP 

kubectl delete deploy nodejs
kubectl delete svc nodejs
kubectl get po
kubectl delete pod test-ecr

```

