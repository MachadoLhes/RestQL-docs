# Configuration

**restQL-http** comes with the following configuration settings

## Enviromnent variables:
- `PORT` allows you to specify wich PORT the servers will listen (default is `9000`)
- `MONGO_URL` allows you to specify the connection with restQL manager database (default is `nil`)
- `EXECUTOR_UTILIZATION` allows you to specify the server executor thread pool target utilization (default is `0.9`)
- `EXECUTOR_MAX_THREADS` allows you to specify the server executor max threads (default is `512`)
- `EXECUTOR_CONTROL_PERIOD` allows you to specify the interval, in milliseconds, between use of the controller to adjust the size of the pool (default ius `1000`)
- `POOL_CONNECTIONS_PER_HOST` allows you to specify the maximum number of simultaneous connections to any host (default is `100`)
- `POOL_TOTAL_CONNECTIONS` allows you to specify the maximum number of connections across all hosts (default is `10000`)
- `POOL_MAX_QUEUE_SIZE` the maximum number of pending acquires from the pool that are allowed before `acquire` will start to throw a Exception (default is `65536`)
- `QUERY_GLOBAL_TIMEOUT` allows you to specify the default timeout, in milliseconds, for each query (default is `30000`)
- `QUERY_RESOURCE_TIMEOUT` allows you to specify the default timeout, in milliseconds, for each resource request (default is `5000`) 
- `CACHE_TTL` allows you to specify the TTL, in milliseconds, to be used to cache Saved Queries (default is `60000`)
- `CACHE-COUNT` allows you to specify the number of items stored in cache (default is `2000`)
- `MAPPINGS_CACHE_TTL` allows you to specify the TTL, in milliseconds, to be used to cache Resources Mappings URLs (default is `60000`)
- `TENANT` allows you to choose the tenant which contains your resources
- `ALLOW-ADHOC-QUERIES` if set to `false`, blocks the execution of adhoc queries (via PUT HTTP mehtod), useful for performance (default is `true`)
- `RESTQL-CONFIG-FILE` allows you to change the name of the YAML configuration file. Note that it still needs to be located at the `src/resources/` folder (default is `restql.yml`)


## Configuration file:

Allows you to configure the resource mappings and saved queries (may be overwritten by [**restQL-manager**](https://github.com/B2W-BIT/restQL-manager) configurations).

### Location:
By default the configuration file is placed in `src/resources` path. 

## Adding Resources:

Resources can be added either through the `restql.yml` file or through connecting to a mongoDB collection.

### Files: 

`restql.yml`
``` yml
mappings:
  your-resource: "http://your-resource-url.com"

queries:
  your-namespace:
    your-query:
      - |
        from your-resource
      - |
        use max-age=900
        from your-resource
  your-namespace-2:
    your-query:
      - |
        from your-resource
      - |
        use max-age=900
        from your-resource
```

### Via a mongoDB collection:

There should be a collection called `tenant`, that shold look like that:
```json
{
    "_id" : "NINTENDO",
    "mappings" : {
        "mario-v3" : "http://mario.io/player/:name",
        "zelda" : "http://zelda.io/player/:id",
    }
}
```
Tenants are RestQL's equivalent of an environment. Each tenant has it's own resources

Then, at the moment of running the container, you should pass `tenant=$tenantId` alongside with the `-Dmongo-url` flag:
```bash
docker run -p 9000:9000 -e JAVA_OPTS="-Dmongo-url=mongodb://my-mongo-ip:27017/restql-server -Dtenant=NINTENDO" restql-server-img
```