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

- - - -

### GETTING STARTED

#### OKTA SETUP (10 minutes)

Please read [okta-setup.md](/okta-setup.md)

* At the end of the okta setup, you will gather the dev-xxxxx.okta.com url, client id, client secret that will be required as env variables during app deployment

#### DEPLOY ON TANZU APPLICATION SERVICE PLATFORM

Please read [tanzu-app-services.md](/tanzu-app-services.md)
