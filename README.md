# Excalidraw with a TWIST!!!
Excalidraw is a beautiful attempt at collaborative whiteboarding combining simplicity and speed of latest technologies. The attempt is to mashup Excalidraw app (ReactJS + WebSockets) with OAuth2.0 authentication deployed on a NGINX frontend proxy-layer integrated with OKTA. In addition, we use this combination of frontend (NGINX-proxy-auth-layer) plus backend (excalidraw app) to be deployed on a cloud native platform like Tanzu Application Services.

## Architectural view (made with Excalidraw)
<div align="center" style="display:flex;flex-direction:column;">
  <a href="https://excalidraw.com">
    <img src="Excalidraw-okta.png" alt="Excalidraw logo: Sketch handrawn like diagrams." />
  </a>
  <h3>Excalidraw is a whiteboard tool that lets you easily sketch diagrams with a hand-drawn feel. In this scenario, we are leveraging the simplicity of TANZU APPLICATION SERVICES platform to deploy the Excalidraw app along with an authentication workflow with OKTA as the OIDC provider</h3>
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

## PRE-REQUISITES

* You have a Tanzu Application Services environment running with access to public docker hub registry.
* You have an Org, Space and User defined to deploy applications
* You have cf CLI v7 installed on your machine as a developer of these applications
* You are logged into your Org/Space via cf-cli

## OKTA SETUP

## DEPLOY EXCALIDRAW AS AN INTERNAL ONLY OR BACKEND APP

The Excalidraw will be deployed as an internal only application and cannot be directly accessed from sources external to the platform. This represents a classical way to approach separation of concerns, where the authentication workflows are handled via the frontend-proxy layer. Navigate to excalidraw directory and open the manifest.yml file

```
---
applications:
  - name: your-unique-app-name
    docker:
      image: thecloudgarage/anh-excalidraw
    routes:
    - route: your-unique-app-name.apps.internal
```

* Edit the app name where "your-unique-app-name" is listed
* Do not change the image path/name as the excalidraw image is already hosted on public docker hub via thecloudgarage/anh-excalidraw
* Issue the command cf-push & the application will be deployed to your platform as an "INTERNALLY ONLY" app

## DEPLOY NGINX FRONTEND

* Navigate to nginx-frontend directory
* Edit the manifest.yml
* Change the app name to reflect the redirect url and initiate login url provided in the OKTA setup
* Change the environmental variables 
* Issue the command "cf push"

## ENABLE SECURE CONTAINER-TO-CONTAINER NETWORKING b/w NGINX FRONTEND & EXCALIDRAW APP

* Tanzu Application Service is high on security
* By default container apps cannot talk to each other on internal network
* We have to deploy networking policies to enable the same
* Issue the command 
```
cf add-network-policy <nginx-app-name> <excalidraw-app-name> --protocol tcp --port 80
```
* Access the application on the NGINX app URL
