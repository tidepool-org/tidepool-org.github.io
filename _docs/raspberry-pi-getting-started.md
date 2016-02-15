---
layout: defaults
title: Getting started on the Raspberry Pi 
published: true
---

**NB: This document is out-of-date and deprecated. It may be deleted soon. (If you'd like to save it by updating it, feel free to [open a pull request](https://github.com/tidepool-org/tidepool-org.github.io).)**

Forays by [eurenda](https://github.com/eurenda) into installing Tidepool according to [instructions](http://developer.tidepool.io/starting-up-services/).

Intended as a proof-of-concept.  No guarantees of speed, reliability, or usability (though everything seems to indicate that it is usable speed when rendered in Chrome):

* Install Linux prerequisites that come as packages: git, bzr used apt-cache search <keyword> for exact name
* Installed Node.js using [instructions](http://joshondesign.com/2013/10/23/noderpi) (actually ended up using [package method](http://weworkweplay.com/play/raspberry-pi-nodejs/) )
* Installed `node-gyp`, `bower`, `grunt-cli`, `gulp`, `mocha` using `sudo npm install -g <package>`
* Increased swap file to 1GB according to [these instructions](http://lokir.wordpress.com/2012/10/13/raspberry-pi-debian-how-to-change-swap-size/) -- not sure this was needed
* Installed [go on raspberry pi](http://dave.cheney.net/unofficial-arm-tarballs)
* Github SSH - Installed an SSH key from my Raspberry pi for GitHub using [these instructions](https://help.github.com/articles/generating-ssh-keys)
* MongoDB was installed by [cloning ArduPi](https://github.com/brice-morin/ArduPi) -- this has precompiled mongodb binaries in a directory with instructions (avoided having to compile hopefully) and followed [these instructions](https://github.com/brice-morin/ArduPi/tree/master/mongodb-rpi/linux-test)
  * made directory `/data/db` and made sure to start `mongod` pointed to this directory since it is what tidepool expects
  * tested with web tests from point 6 -- mongodb version is 2.1.1 -- does it really matter?  NO!
* Consulted tidepool support to find out that Blip and Jellyfish required git checkouts of v0.1.2.7 and v0.2.4 respectively in their directories, followed by an npm install for each (version current as of September 17, 2014, may not apply when you read this)
* Was finally able to execute `. tools/runservers` and waited for 7-10 minutes for framework to install and run
  * tested Blip and Jellyfish by going to [http://localhost:3000](http://localhost:3000) and [http://localhost:3004](http://localhost:3004) respectively -- success!
