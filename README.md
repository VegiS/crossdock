# Crossdock [![Build Status](https://travis-ci.org/crossdock/crossdock.svg)](https://travis-ci.org/crossdock/crossdock) [![](https://badge.imagelayers.io/crossdock/crossdock:latest.svg)](https://imagelayers.io/?images=crossdock/crossdock:latest 'Get your own badge on imagelayers.io')

A tiny Docker appliance for running cross-repo integration tests; Crossdock is:

* Portable - runs anywhere Docker is installed, eg Travis & locally.
* General - can be used to test sets of libraries and microservices.
* Flexible - test all combinations of behaviors using custom matrix axis.
* Decentralized - each repo can configure and run Crossdock independently from the others.
* Light - run Crossdock for every commit on every repo in parallel.
* Easy - run integration tests on a large project without installing every component.

## How It Works

Crossdock is [published in Docker Hub](https://hub.docker.com/r/crossdock/crossdock/) and is
meant to be used with [Docker Compose](https://docs.docker.com/compose/) directly from your repos.

Given the following `docker-compose.yml`:

```yml
crossdock:
    image: crossdock/crossdock
    dns_search: .  # Ensures unified DNS config.
    links:
        - alpha
        - omega
    environment:
        - WAIT_FOR=alpha,omega

        - AXIS_CLIENTS=alpha,omega
        - AXIS_SPEED=fast,slow

        - BEHAVIOR_DANCE=clients,speed
        - BENAVIOR_RUN=clients,speed

alpha:
    image: breerly/hello-server
    dns_search: .
    ports:
        - 8080
    environment:
        - HELLO_PORT=8080
        - HELLO_MESSAGE=ok

omega:
    image: breerly/hello-server
    dns_search: .
    ports:
        - 8080
    environment:
        - HELLO_PORT=8080
        - HELLO_MESSAGE=ok
```

Running Crossdock will initiate tests for clients `alpha` and `omega` for
every combination of `behavior` and `speed`:

```
$ docker-compose run crossdock

Beginning matrix of tests...

  STATUS | CLIENT | RESPONSE | SPEED | BEHAVIOR
+--------+--------+----------+-------+----------+
  PASSED | alpha  | ok       | fast  | dance
  PASSED | alpha  | ok       | fast  | run
  PASSED | alpha  | ok       | slow  | dance
  PASSED | alpha  | ok       | slow  | run
  PASSED | omega  | ok       | fast  | dance
  PASSED | omega  | ok       | fast  | run
  PASSED | omega  | ok       | slow  | dance
  PASSED | omega  | ok       | slow  | run
```

## How To Use

1. [Write Test Client](docs/write-test-client.md)
2. [Run Crossdock](docs/run-crossdock.md)
3. [Publish Test Client](docs/publish-test-client.md)
4. [Integrate Other Repos](docs/integrate-other-repos.md)
5. [Add Other Test Axis](docs/add-other-axis.md)
6. [Run During Continuous Integration](docs/add-to-ci.md)

Cheers.
