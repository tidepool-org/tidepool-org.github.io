Starting up the platform locally requires a number of different pieces to make everything work.  

1. You will first need to start up Mongo, a single Mongo is sufficient.
2. Next you should start up a [hakken](http://tidepool-org.github.io/TidepoolComponents.html#hakken) coordinator.  Remember the port that you have it listening on.  An example config for this service might be:

    ```
    DISCOVERY_HOST=localhost:8000
    PORT=8000
    ANNOUNCE_HOST=localhost
    ```

3. Now you can start firing up the individual services.  Before firing up the individual services it is important to note that there are some pieces of configuration that must line up.

Specifically,

```
DISCOVERY_HOST=localhost8000
```

Should be set on all services.

Also, for services that interconnect, you will need to make sure that the `SERVICE_NAME` and `XXX_SERVICE` properties line up.  Like, if the service uses user-api, then you will need to set `USER_API_SERVICE` equal to the value of `SERVICE_NAME` set on the user-api server. Now that you have thought about how you are configuring your services, you can actually fire them up.
4. Fire up the services that you need in any order.  For services that setup watches on other services, you should notice log lines when it finds instances of those services.