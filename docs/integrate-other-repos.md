[← Publish Test Client](publish-test-client.md)

# Integrate Other Repos

Adding additional Test Clients to the mix is easy. To demonstrate, we're going to use the
`breerly/hello-server` which we can configure to act like a Test Client.

Let's add a new client to our `docker-compose.yml` now:
```yml
crossdock:
    image: yarpc/crossdock
    links:
        - client
        - newclient
    environment:
        - CROSSDOCK_CLIENTS=client,newclient
        - CROSSDOCK_AXIS_BEHAVIOR=dance,run

client:
    build: .
    ports:
        - 8080

newclient:
    image: breerly/hello-server
    environment:
        - HELLO_PORT=8080
        - HELLO_MESSAGE=ok
```

Notice how we're using `image:` when referring to a Test Client
produced by another repo, and `build` when referring to a Test Client within
the same repo? This effectively allows you to run the current commit, against other
repos `master` branch Test Client.

Now let's run Crossdock:

```
$ docker-compose run crossdock

Beginning matrix of tests...

  STATUS  |  CLIENT   |      RESPONSE       | BEHAVIOR
+---------+-----------+---------------------+----------+
  PASSED  | client    | ok                  | dance
  SKIPPED | client    | 404                 | run
  PASSED  | newclient | ok                  | dance
  PASSED  | newclient | ok                  | run

```

Great. Notice how a test was triggered for every Test Client, times every Behavior.

[Add Other Test Axis →](add-other-axis.md)