# Excalidraw with a TWIST!!!
Excalidraw is a beautiful attempt at collaborative whiteboarding combining simplicity and speed of latest technologies. The attempt is to mashup Excalidraw app (ReactJS + WebSockets) with OAuth2.0 authentication deployed on a NGINX frontend proxy-layer integrated with OKTA. In addition, we use this combination of frontend (NGINX-proxy-auth-layer) plus backend (excalidraw app) to be deployed on a cloud native platform like Tanzu Application Services.

## Architectural view (made with Excalidraw)
<div align="center" style="display:flex;flex-direction:column;">
  <a href="https://excalidraw.com">
    <img src="Excalidraw-okta.png" alt="Excalidraw logo: Sketch handrawn like diagrams." />
  </a>
  <h3>Excalidraw is a whiteboard tool that lets you easily sketch diagrams with a hand-drawn feel.</h3>
  <p>
    <a href="https://twitter.com/Excalidraw">
      <img alt="Follow Excalidraw on Twitter" src="https://img.shields.io/twitter/follow/excalidraw.svg?label=follow+excalidraw&style=social&logo=twitter">
    </a>
    <a target="_blank" href="https://crowdin.com/project/excalidraw">
      <img src="https://badges.crowdin.net/excalidraw/localized.svg">
    </a>
    <a target="_blank" href="https://hub.docker.com/r/excalidraw/excalidraw">
      <img src="https://img.shields.io/docker/pulls/excalidraw/excalidraw">
    </a>
  </p>
</div>

Markup :  - - - -

### PRE-REQUISITES

* You have a Tanzu Application Services environment running with access to public docker hub registry.
* You have an Org, Space and User defined to deploy applications
* You have cf CLI v7 installed on your machine as a developer of these applications

Markup :  - - - -

### GETTING STARTED

#### STEP-1 OKTA SETUP (10 minutes)

Please read [okta-setup.md](/okta-setup.md)

* At the end of the okta setup, you will gather the dev-xxxxx.okta.com url, client id, client secret that will be required as env variables during app deployment

#### STEP-2 DEPLOY THE APPLICATIONS ON TANZU APPLICATION SERVICE PLATFORM

The Excalidraw will be deployed as an internal only application and cannot be directly accessed from sources external to the platform. This represents a classical way to approach separation of concerns, where the authentication workflows are handled via the frontend-proxy layer. The frontend-proxy layer is a NGINX reverse proxy with a LUA module that has an OAuth2.0 integration with OKTA.

* Clone this git repository. Under the parent directory, you will find a manifest.yml file that constructs the deployment schema for our application

```
---
applications:
  - name: <unique-string>-excalidraw-proxy
    docker:
      image: thecloudgarage/anh-excalidraw-nginx-frontend-okta
    env:
      okta_url: dev-xxxxx.okta.com
      okta_client_id: xxxxxxxxxxxxxxx
      okta_client_secret: xxxxxxxxxxxxxxxxxxx_
      redirect_url: "https://<unique-string>-excalidraw-proxy.cfapps.io/redirect_url"
      proxied_url: "<unique-string>-excalidraw-backend.apps.internal"
  - name: <unique-string>-excalidraw-backend
    docker:
      image: thecloudgarage/anh-excalidraw
    routes:
    - route: <unique-string>-excalidraw-backend.apps.internal
```

* Replace the "unique-string" value with something (e.g. your name) that will identify your app uniquely on the TANZU platform
* Do not change the docker image path/names as these images are readily provided on public docker hub. In case you download these images on to a private registry, then change the docker image path/names accordingly
* Change the environmental variables with the values that you collected from the OKTA setup
* Issue the command "cf push" to deploy the frontend proxy and excalidraw app via single manfiest file

#### STEP-3 ENABLE SECURE CONTAINER-TO-CONTAINER NETWORKING b/w NGINX FRONTEND & EXCALIDRAW APP

* Tanzu Application Service is high on security controls
* By default container apps cannot talk to each other on internal network
* We have to deploy networking policies to enable the same
* Issue the command 
```
cf add-network-policy <unique-string>-excalidraw-proxy <unique-string>-excalidraw-backend --protocol tcp --port 80
```
* Access the application on the NGINX app URL
