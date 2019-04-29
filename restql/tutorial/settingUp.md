# Setting Up

## Connecting to a REST API

To add a resource to RestQL we simply need to edit `restql.yml`, the file you created a few momments ago. Let's add Elon Musk's space business' API:

```yaml
mappings:
  launches: "https://api.spacexdata.com/v3/launches"
```

Any value assigned to that key should point to a RESTful API.

## Building Docker image

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

## Running the container

Then run the image as a container with the command:

```shell
docker run -p 9000:9000 restql-server-img
```

Nice! Now `restql-http` is being served at `localhost:9000`!