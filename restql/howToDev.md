# Building Locally

## Downloading Source Code
Simply follow these steps:

1. Download the latest release in the [release page](https://github.com/B2W-BIT/restQL-server/releases),
2. Unzip the package,

## Running as a Docker container

### Building Docker image

restQL Server can also be run as a Docker container.
First, create the `restql.ymml` at the `src/resources/` folder

Then execute the build-dist script:

```shell
./scripts/build-dist.sh
```

Finally, from the source code root folder, build a Docker image with the command:

```shell
docker build -t restql-server-img .
```

### Running the container

Then run the image as a container with the command:

```shell
docker run -p 9000:9000 -e JAVA_OPTS="-Dmongo-url=mongodb://my-mongo-ip:27017/restql-server -Dplanets=http://swapi.co/api/planets/" restql-server-img
```

You can register your APIs as resources by passing them in `JAVA_OPTS` environment variable, as seen above.
The server default port is 9000 but it can be changed by passing the `PORT` environment variable with the desired port.

The MongoDB instance can also run from a container. If that's the case you can link to it and use its link name as the address:

```shell
docker run -p 27017-27017 --name mongo-docker mongo
docker run --link mongo-docker -p 9000:9000 -e JAVA_OPTS="-Dmongo-url=mongodb://mongo-docker:27017/restql-server -Dplanets=http://swapi.co/api/planets/" restql-server-img
```

The MongoDB dependency is optional and is used to store saved queries.



## Running From Source Code

### Building From Source Code
As prerequisites to build restQL-server from source we need:

- Java 11
- Leiningen

Build the server using the build script: `scripts/build-dist.sh`.  

The building script will create a folder `dist` where you can configure your resources on the file `dist/bin/env.sh` and run the server using the script `dist/bin/run.sh`.

If you want to deploy restQL-server, copy the files under the generated `dist` folder.

### Running the server
To run the restQL Server follow the bellow steps from the source code root:

1. Edit the file `dist/bin/env.sh` with the resources you want to invoke,
2. Run `dist/bin/run.sh`.

Then you can test it, since restQL Server allows you to post ad-hoc queries and to reference resources pre-configured in the server startup (in the env.sh file). Post to `http://<your-server>:9000/run-query` the following body:

```
from planets as allPlanets
```



## Running restQL Manager

restQL Manager is a Web interface (UI and REST) for the restQL Server. With restQL Manager you can easily develop and test new queries, save resources endpoints, check resources status and save queries that can be used by clients just referencing the query's name.

To learn how to run the manager check the project at [restQL manager](https://github.com/B2W-BIT/restQL-manager).

All saved queries are composed by the query name and it's version. They're also immutable, which means each query edit generates a new version. This is designed to avoid unaware impact in production system and optimize caching.

To learn how to save and run queries check the [Saved Queries](/restql/savedQueries) page.

