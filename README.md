skynetheremin
=============

A distributed theremin, powered by a Leap Motion controller, a user-interface device that tracks the user's hand motions.

![Screenshot](screenshot.png)

## Demo App Usage

Using Chrome, Firefox, or Safari (preferred in that general order), go to: http://skynetheremin.herokuapp.com/

Click and drag on the screen to make your own music.

During a demonstration, you should also hear music coming from the sky.


## Tech Notes

To install all libraries for this app, pull the code and run:

    npm install

The post-install hooks should also grab the bower libraries.

There are three major components: the server, the broadcaster, and the web client.

### Server

The server is a lightweight express app written in node. Basically all it does is handle websockets between the web clients, and re-broadcasts information from the broadcaster to each web client.

To run the server locally:

    node app/server.js

The server makes use of require.js's r.js optimization script to speed up file retrieval. This should run automatically when npm install is run. To re-run, issue the following command:

    node node_modules/.bin/r.js -o requirebuild.js

To turn off the use of this optimized file (i.e. while testing), comment out the appropriate line in require-main.js.

### Broadcaster

The broadcaster handles reading information from the Leap Motion, and sending that information to the server. It makes use of [Cylon.js](https://github.com/hybridgroup/cylon-leapmotion) to translate between the Leap Motion and give you callbacks that you can override in reaction to sensor events.

To run the broadcaster:

    node app/broadcaster.js [target] [--debug]

Ex:

    node app/broadcaster.js http://skynetheremin.herokuapp.com/ --debug

Press spacebar to start and stop broadcasting.

### Web Client

The web client has been tested on Chrome, Firefox, and Safari. (Tested on Mavericks.) The main features are:
 
* Accepts mouse dragging inputs from the user to create sound
* Renders sound into a pleasant-looking visualization
* Establishes a connection to the remote server to receive instructions from the broadcaster

The sound is synthesized using Web Audio.

The basic UI was inspired by (and/or reverse-engineered from) the Theremin by [Femur Designs](http://www.femurdesign.com/theremin/).


## Deployment

This node app is set up to deploy to heroku. To do so, websockets must be enabled. In addition, the buildpack should be set to the node.js pack:

    heroku config:add BUILDPACK_URL=https://github.com/heroku/heroku-buildpack-nodejs
    heroku labs:enable websockets

For more information, check out the following articles:

* https://devcenter.heroku.com/articles/getting-started-with-nodejs
* https://devcenter.heroku.com/articles/node-websockets
