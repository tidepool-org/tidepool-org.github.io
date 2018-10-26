---
layout: defaults
title: Starting Up Services
published: true
---

# Getting started

Welcome to the technical side of Tidepool!

If you have any questions, please join us in our [Gitter channel](https://gitter.im/tidepool-org/public) or [e-mail us](mailto:info@tidepool.org).

Note: we *don't* use GitHub Issues to track issues on our platform and application repositories. Open source contributors are welcome to file issues on GitHub, but our process will be to ingest them into our project management process (currently mainly on Trello) and close them.

## Quickstart

Starting up the platform locally requires a number of different pieces to make everything work.

## Prerequisites

You need a command line development environment; you need a bash-compatible shell and git, mercurial (hg) bzr, npm, node, grunt, mocha, and mongod installed and on your path to support JavaScript, and the Go language installed for supporting Go.

If you're performing a new MongoDB installation on a Mac (using `brew install mongodb`), you also need to do the following:

- Run `sudo mkdir -p /data/db`
- Set permissions for `/data/db` to the local user using `sudo chown <your username> /data/db`

Some of the node libraries we use have native (C/C++) code in them, so you also need a C++ compiler. To get one on a Mac, you'll need to install XCode, and the XCode command-line tools. (After opening XCode once to accept this license agreement, you can install the command-line tools from the Terminal with `xcode-select --install`.)

## Installing everything

The easiest thing to do is to clone the [tools repository](https://github.com/tidepool-org/tools) and run ```get_current_tidepool_repos.sh```. (Apologies to Windows users; none of us in Tidepool are, so our scripts are built around bash; people have successfully set up the Tidepool stack on Windows using bash emulators.) ```get_current_tidepool_repos.sh``` will clone the other necessary Tidepool repositories alongside the tools repository. It also checks to make sure you have the requirements installed.

After you've completed these steps, you can start up everything with the following command:

```
$ . tools/runservers
```

Please note that runservers is NOT a bash script -- it needs to be run with "." (aka "source"). This is why it is not marked as executable.

See the comments at the top of the `runservers` file for more information on using it.

## The details

If you want to do it the hard way:

* You will first need to start up Mongo; a single Mongo is sufficient.
* Next you should start up a hakken coordinator.  Remember the port that you have it listening on.  An example config for this service might be:

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

## Using Tidepool

Congratulations! You should have a local copy of the Tidepool platform running now.  If everything worked, it will be waiting for a browser to connect at [http://localhost:3000](http://localhost:3000).  Opening that link should load a login page, with a 'Sign Up' button in the top-right.

Creating a new account on a local Tidepool platform is a little different than using the public system.  You will need to pick an email address that contains the text `+skip` (e.g., `testUser+skip@example.com`). Don't worry, no email will actually be sent there, so you can make up any email address you want. You will need to remember it, though, because that is the username you will use to log in.

Click `Sign up` and your account will be created. You can ignore the screen that tells you to check your email because you used a `+skip` address. You can go straight to the [Log in](http://localhost:3000/#/login) screen and log in with the email address and password you set in the sign up step.

## Uploading Data

The [chrome-uploader](https://github.com/tidepool-org/chrome-uploader) is the simplest way to upload data to Tidepool.  Again, you will need to do things a little differently to make use of your local platform. (The extension in the Chrome store won't work with a locally-running version of the platform.)

If you used the `get_current_tidepool_repos.sh` script above, then you should have a local copy of the `chrome-uploader` in your Tidepool directory.  Using the same shell where you started the Tidepool services (with `tools/runservers`), change into the `chrome-uploader` directory and follow the directions for building and loading the uploader in the [README.md](https://github.com/tidepool-org/chrome-uploader/blob/master/README.md#how-to-set-it-up)

