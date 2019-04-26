# Connecting to a REST API

To add a resource to RestQL we simply need to edit `restql.yml`, the file you created a few momments ago. Let's add Elon Musk's space business' API:

```yaml
mappings:
  launches: "https://api.spacexdata.com/v3/launches"
```

There's nothing special about this YAML file, the procedure is simple, any key you add can be consulted with a `from` statement just like this:

```
from launches
```

Any value assigned to that key should point to a RESTful API.

Now let's make a simple query to retrieve all launches
```
from launches
```
This is making a `GET` request to the endpoint `https://api.spacexdata.com/v3/launches`, you should see the following result:

```json
{
  "launches": {
    "details": {
      "success": true,
      "status": 200,
      "metadata": {}
    },
    "result": [
      {
        "launch_date_unix": 1143239400,
        "mission_name": "FalconSat",
        "launch_success": false,
        "mission_id": [],
        "is_tentative": false,
        "launch_window": 0,
        "launch_site": {
          "site_id": "kwajalein_atoll",
          "site_name": "Kwajalein Atoll",
          "site_name_long": "Kwajalein Atoll Omelek Island"
        },
        "upcoming": false,
        "tbd": false,
        "details": "Engine failure at 33 seconds and loss of vehicle",
        "launch_failure_details": {
          "time": 33,
          "altitude": null,
          "reason": "merlin engine failure"
        },
        "telemetry": {
          "flight_club": null
        },
        "static_fire_date_utc": "2006-03-17T00:00:00.000Z",
        "tentative_max_precision": "hour",
        "static_fire_date_unix": 1142553600,
        "launch_date_utc": "2006-03-24T22:30:00.000Z",
        "timeline": {
          "webcast_liftoff": 54
        },
...
```
*and it goes on all the way until line 12617*

Woah, that's a lot of data to handle! Our friends at [Apollo GraphQL]() handle it by creating a `schema` which, basically, filters the response to get only the data that you want, an **GraphQL Schema** looks somewhat like this:

```javascript
type Launch {
  id: ID!
  site: String
  mission: Mission
  rocket: Rocket
  isBooked: Boolean!
}
```

This will guarantee the response will have **only** the data that you need. Got the word? **Only**, this is how you use `only` in restQL:

```
from launches   
    only id, site, mission, rocket, isBooked
```
This will return:
```json
{
  {
  "launches": {
    "details": {
      "success": true,
      "status": 200,
      "metadata": {}
    },
    "result": [
      {
        "site": null,
        "mission": null,
        "id": null,
        "rocket": {...},
        "isBooked": null
      },
      {
        "site": null,
        "mission": null,
        "id": null,
        "rocket": {...},
        "isBooked": null
      },
      {
        "site": null,
        "mission": null,
        "id": null,
        "rocket": {...},
        "isBooked": null
      },
      ...
    ]
  }
}
```
**Note:** the `rocket` property is a huge object, we hid it with `{...}` just for keeping it simple.

Oh yeah, that's a lot more understandable, and achieving this understandability is as simple as this, no schemas, no extra coding, nada.

Now let's say you want to search for an specific launch, since it's another endpoint, you should create another resource entry at `restql.yml`:

```yaml
mappings:
    launches: "https://api.spacexdata.com/v3/launches"
    oneLaunch: "https://api.spacexdata.com/v3/launches/:id"
```

See the `:id` at the end of the endpoint? This is the name of the parameter you should use the `with` clause with:

```
from oneLaunch
	with id = 27
    only id, site, mission, rocket, isBooked
```
This query will perform a `GET` to `https://api.spacexdata.com/v3/launches/27` and you'll see the following as a response:

