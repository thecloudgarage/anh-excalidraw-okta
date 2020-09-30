# collab-server

* This is a fork of https://github.com/excalidraw/excalidraw-room.git
* The master branch seems to have an issue where the docker build fails
* On checking, the commit 35314725c4 seems to be working where the docker build is successful
* The version included in this directory is a clone of this working branch, and tested the docker build (working nicely)
* docker run --rm -dit --name excalidraw/excalidraw-room -p 5000:80 excalidraw/excalidraw-room:latest
