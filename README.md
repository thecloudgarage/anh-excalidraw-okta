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

* You have a Tanzu Application Services environment running
* You have an Org, Space and User defined to deploy applications
* You have cf CLI v7 installed on your machine as a developer of these applications
* You are logged into your Org/Space via cf-cli

## OKTA SETUP

## DEPLOY EXCALIDRAW

* Navigate to excalidraw directory and open the manifest.yml file
* Edit the app name and also the apps.internal route
* Issue the command cf-push
* The application will be deployed to your platform as an "INTERNALLY ONLY" app

## DEPLOY NGINX FRONTEND

* Navigate to nginx-frontend directory
* Edit the manifest.yml
* Change the app name to reflect the redirect url and initiate login url provided in the OKTA setup
* Change the 



* Install nodejs
* sudo npm install
* npm run-script build
* cd build
* touch Staticfile
* cf push

## Push to Pivotal Web Services
* Register on run.pivotal.io
* Install cf cli on windows or linux
* clone or download this git repo
* cd into the repo directory
* cf login
* cf push <unique-app-name>
* Accessing the application over https will also enable collaborative whiteboarding via rooms

## Push to Pivotal Web Services with a docker image

```
cf login
cf push <unique-app-name> --docker-image thecloudgarage/anh-new-whiteboard
```
* Accessing the application over https will also enable collaborative whiteboarding via rooms

Have fun!!!
  
## Try it now

Go to https://excalidraw.com to start sketching.

Read our [blog](https://blog.excalidraw.com) and follow the [guides](https://howto.excalidraw.com) to learn more about Excalidraw and how to use it effectively.

## Run the code

### Code Sandbox

- Go to https://codesandbox.io/s/github/excalidraw/excalidraw
  - You may need to sign in with Github and reload the page
- You can start coding instantly, and even send PRs from there!

### Local Installation

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

#### Clone the repo

```bash
git clone https://github.com/excalidraw/excalidraw.git
```
