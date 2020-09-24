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

### PRE-REQUISITES

* You have a Tanzu Application Services environment running with access to public docker hub registry.
* You have an Org, Space and User defined to deploy applications
* You have cf CLI v7 installed on your machine as a developer of these applications


### GETTING STARTED

#### STEP-1 OKTA SETUP (10 minutes)

* ***SIGNUP*** Go to https://developer.okta.com/signup & provide the signup details
  * On the next page, scroll down and select "Do it Later" for customizations
  * Click on Verify your email address on the next screen
  * Upon signup, you will receive an email to verify your email address for account activation
  * Open the verification email received (note., it also contains a temporary password)
  * Once verified, you will be redirected to your OKTA developer account page
  * Note the URL in the browser bar, e.g. dev-xxxxxx.okta.com
  * Login using the email address that was used to setup the account along with the temporary password received in the email
  * Set up your new password and other security details and continue
  * You will be land up in the admin console of your OKTA developer account
  * You will be asked to customize your profile
  * You can select the appropriate role for your profile and select "Custom App" along with the languages that your prefer as a developer
  * From hereon, you can straight jump to setting up the OIDC setup for your application
  * You can ignore the other GETTING STARTED actions
* ***OIDC Setup***
  * Click on the top bar "Users" > "People" in the admin page
  * Click on "Add Persons" and add a test user with a valid email address (for the password, select your preference, set by user or admin)
  * Once done, your added user will receive a verification email
  * Next, click on "Applications" on the top admin bar
  * Then click on "Add Application" and select "Web" as the type and "next"
  * You will arrive at a page to setup your Application
  * Give it a suitable name
  * Base URIs: This url must be the same as the one that you will use to access the fronend NGINX reverse proxy
    * *Example* <https:unique-string-excalidraw-frontend.your-cf-apps-domain>
  * Login redirect URIs: Same as above but suffixed with redirect_uri path
    * *Example* <https:unique-string-excalidraw-frontend.your-cf-apps-domain/redirect_url>
* Leave everything else to defaults and click done
* On the next page, you will see a "Client ID" and "Client secret" value. Copy those for further use
* Next, click on the "Assignments" tab for your Application
* Click on "Assign" > "Assign to People" and select users that should be able to access this application
* Next, click on "API" on the top admin bar and then "Trusted Origins"
* Click on "Add Origins" and give a suitable name and in the origin URL, type the same value as in BASE URL and also select both the options in TYPE
* Example: https:<unique-string>-excalidraw-frontend.<your-cf-apps-domain> for the Origin URL
* Now you are done with the OKTA setup. At the end of this exercise, you will need the below values to proceed further
* Your OKTA URL dev-xxxxxx.okta.com
* CLIENT ID, CLIENT SECRET and the REDIRECT URL. These values are gathered during the above steps.
* We will need these values as variables while deploying our application

## DEPLOY THE APPLICATIONS ON TANZU APPLICATION SERVICE PLATFORM

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

## ENABLE SECURE CONTAINER-TO-CONTAINER NETWORKING b/w NGINX FRONTEND & EXCALIDRAW APP

* Tanzu Application Service is high on security controls
* By default container apps cannot talk to each other on internal network
* We have to deploy networking policies to enable the same
* Issue the command 
```
cf add-network-policy <unique-string>-excalidraw-proxy <unique-string>-excalidraw-backend --protocol tcp --port 80
```
* Access the application on the NGINX app URL
