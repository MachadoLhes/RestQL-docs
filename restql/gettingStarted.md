# Getting Started

## Running restQL HTTP

restQL Server allows you to post ad-hoc queries and to reference resources pre-configured in the server startup.

1. Download the latest release in the [release page](https://github.com/B2W-BIT/restQL-http/releases),
2. Unzip the package,
3. Edit the file env.sh with the resources you want to invoke,
3. Run bin/run.sh.

Post to http://your-server.ip:9000/run-query the body below and content-type text/plain:

```clojure
curl -H "Content-Type: text/plain" localhost:9000/run-query -d "from planets as allPlanets" 
```

If you want to know more about the **query language**, [click here](/restql/queryLang.md)

## Your first query

Here's a basic example of what a restQL query should look like:

```
from planets
    with id = 1
```
**Note:** on this example, we use [Star Wars API](https://swapi.co) for educational purposes. The `planets` resource leads to `https://swapi.co/api/planets/:id`

from which you get the response:
```json
{
  "planets": {
    "details": {
      "success": true,
      "status": 200,
      "metadata": {}
    },
    "result": {
      "surface_water": "1",
      "climate": "arid",
      "residents": [
        "https://swapi.co/api/people/1/",
        "https://swapi.co/api/people/2/",
        "https://swapi.co/api/people/4/",
        "https://swapi.co/api/people/6/",
        "https://swapi.co/api/people/7/",
        "https://swapi.co/api/people/8/",
        "https://swapi.co/api/people/9/",
        "https://swapi.co/api/people/11/",
        "https://swapi.co/api/people/43/",
        "https://swapi.co/api/people/62/"
      ],
      "orbital_period": "304",
      "name": "Tatooine",
      "diameter": "10465",
      "created": "2014-12-09T13:50:49.641000Z",
      "gravity": "1 standard",
      "edited": "2014-12-21T20:48:04.175778Z",
      "films": [
        "https://swapi.co/api/films/5/",
        "https://swapi.co/api/films/4/",
        "https://swapi.co/api/films/6/",
        "https://swapi.co/api/films/3/",
        "https://swapi.co/api/films/1/"
      ],
      "population": "200000",
      "terrain": "desert",
      "url": "https://swapi.co/api/planets/1/",
      "rotation_period": "23"
    }
  }
}
```

## Next steps

1. Learn restQL [query language](/restql/queryLang),
2. Learn about the [manager and saved queries](/restql/manager),
3. Get involved and [contribute ツ](/restql/howToContribute)