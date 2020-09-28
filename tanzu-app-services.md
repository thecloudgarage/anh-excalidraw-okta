### Deploying Excalidraw (frontend & backend) on TANZU Application Services

The Excalidraw will be deployed as a combination of actual app (docker image) along with a nginx based proxy-layer (docker image) that is integrated with OKTA for OAuth athentication.

* Clone this git repository and edit the manifest.yml under the root directory
* Enable docker support on your Cloud Foundry platform 

```
cf enable-feature-flag diego_docker
```

#### manifest.yml

```
---
applications:
  - name: unique-string-excalidraw-backend
    docker:
      image: thecloudgarage/anh-excalidraw
    routes:
    - route: unique-string-excalidraw-backend.apps.internal
  - name: unique-string-excalidraw
    docker:
      image: thecloudgarage/anh-excalidraw-nginx-frontend-okta
    env:
      okta_url: dev-xxxxx.okta.com
      okta_client_id: xxxxxxxxxxxxxxx
      okta_client_secret: xxxxxxxxxxxxxxxxxxx_
      redirect_url: "https://unique-string-excalidraw.your-cf-apps-domain/redirect_url"
      proxied_url: "unique-string-excalidraw-backend.apps.internal"
```

* Replace the ***unique-string*** value with something (e.g. your name)
* Do not change the docker image path/names as these images unless you have copied them to a private registry
* Change the environmental variables with the values that you collected from the OKTA setup
* Issue the command "cf push" to deploy the frontend proxy and excalidraw app via single manfiest file

#### ENABLE SECURE CONTAINER-TO-CONTAINER NETWORKING b/w NGINX FRONTEND & EXCALIDRAW APP

* Tanzu Application Service is high on security controls
* By default container apps cannot talk to each other on internal network
* We have to deploy networking policies to enable the same
* Issue the command 
```
cf add-network-policy unique-string-excalidraw-frontend unique-string-excalidraw-backend --protocol tcp --port 80
```

* Access the application on the NGINX app URL on <https://unique-string-excalidraw.cf-apps-domain>
* You will be redirected to OKTA for sign-in. Upon successful authentication, you will be able to access your self-hosted excalidraw app
