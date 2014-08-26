---
layout: defaults
title: Starting Up Services
published: true
---

#Getting started

Welcome to the technical side of Tidepool!

If you have questions, you can email us, or join us in the #tidepool IRC channel on freenode.net.

We generally track bugs in [the issues list of Tidepool's "hub" repository](https://github.com/tidepool-org/hub/issues?state=open).

##Quickstart

Starting up the platform locally requires a number of different pieces to make everything work.  

##Prerequisites

You need a command line development environment; you need a bash-compatible shell and git, bzr, npm, bower, node, grunt, mocha, and mongod installed and on your path to support JavaScript, and the Go language installed for supporting Go.

Some of the node libraries we use have native (C/C++) code in them, so you also need a C++ compiler. To get one on a mac, you'll need to install XCode, and the XCode command line tools.

##Installing everything

The easiest thing to do is to clone the [tools repository](https://github.com/tidepool-org/tools) and run ```get_current_tidepool_repos.sh```. (Apologies to Windows users; none of us in Tidepool are, so our scripts are built around bash.) ```get_current_tidepool_repos.sh``` will clone the other necessary Tidepool repositories alongside the tools repository. It also checks to make sure you have the requirements installed.

After you've completed these steps, you can start up everything with the following command:

```
$ . tools/runservers
```

Please note that runservers is NOT a bash script -- it needs to be run with "." (aka "source"). This is why it is not marked as executable.

See the comments at the top of the `runservers` file for more information on using it.

##The details

If you want to do it the hard way:

* You will first need to start up Mongo; a single Mongo is sufficient.
* Next you should start up a [hakken](/tidepool-components#hakken) coordinator.  Remember the port that you have it listening on.  An example config for this service might be:

    ```
    DISCOVERY_HOST=localhost:8000
    PORT=8000
    ANNOUNCE_HOST=localhost
    ```

* Now you can start firing up the individual services. A list of all current services can be found in the tools repository, in the file ```required_repos.txt```. Before firing up the individual services it is important to note that there are some pieces of configuration that must line up.

  Specifically,

  ```
  DISCOVERY_HOST=localhost:8000
  ```

  Should be set on all services.

  Also, for services that interconnect, you will need to make sure that the `SERVICE_NAME` and `XXX_SERVICE` properties line up.  Like, if the service uses user-api, then you will need to set `USER_API_SERVICE` equal to the value of `SERVICE_NAME` set on the user-api server. Now that you have thought about how you are configuring your services, you can actually fire them up.

* Fire up the services that you need in any order.  For services that setup watches on other services, you should notice log lines when it finds instances of those services.
