## Build the application

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
