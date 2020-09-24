The Excalidraw will be deployed as an internal only application and cannot be directly accessed from sources external to the platform. This represents a classical way to approach separation of concerns, where the authentication workflows are handled via the frontend-proxy layer. The frontend-proxy layer is a NGINX reverse proxy with a LUA module that has an OAuth2.0 integration with OKTA.

* Clone this git repository. Under the parent directory, you will find a manifest.yml file that constructs the deployment schema for our application

#### manifest.yml

```
---
applications:
  - name: unique-string-excalidraw-frontend
    docker:
      image: thecloudgarage/anh-excalidraw-nginx-frontend-okta
    env:
      okta_url: dev-xxxxx.okta.com
      okta_client_id: xxxxxxxxxxxxxxx
      okta_client_secret: xxxxxxxxxxxxxxxxxxx_
      redirect_url: "https://unique-string-excalidraw-frontend.cf-apps-domain/redirect_url"
      proxied_url: "unique-string-excalidraw-backend.apps.internal"
  - name: unique-string-excalidraw-backend
    docker:
      image: thecloudgarage/anh-excalidraw
    routes:
    - route: unique-string-excalidraw-backend.apps.internal
```

* Replace the "unique-string" value with something (e.g. your name) that will identify your app uniquely on the TANZU platform
* Do not change the docker image path/names as these images are readily provided on public docker hub. In case you download these images on to a private registry, then change the docker image path/names accordingly
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
* Access the application on the NGINX app URL on <https://unique-string-excalidraw-frontend.cfapps-domain>