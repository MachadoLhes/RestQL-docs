# RestQL Manager

[restQL Manager](https://github.com/B2W-BIT/restQL-manager) allows you to easily develop and test new queries, save resources endpoints, check resources status and save queries that can be used by clients just referencing the query's name.

restQL Manager requires a [restQL-http](https://github.com/B2W-BIT/restQL-http) running instance.

## Installation

### NPM

**restQL Manager** can be installed with [`@b2wdigital/restql-manager`](https://www.npmjs.com/package/@b2wdigital/restql-manager) npm package and run it directely from the shell.

```shell
$ npm i -g @b2wdigital/restql-manager
$ restql-manager
```

### Docker

The official **restQL Manager** docker image can be pulled from `b2wdigital/restql-manager` repository.

#### Basic usage:
```shell
$ docker run -p 9000:9000 b2wdigital/restql-manager:latest
```

#### Custom configuration:
```shell
$ docker run -p 8080:8080 -e RESTQL_MANAGER_PORT=8080 ... b2wdigital/restql-manager:latest
```

## Configuration

restQL Manager uses the following environment variables for its configuration:

- `RESTQL_SERVER_URL`. This will set the [restQL-http](https://github.com/B2W-BIT/restQL-http) instance that will run the queries and **MUST** point to a running [restQL-http](https://github.com/B2W-BIT/restQL-http) instance
- `RESTQL_MANAGER_PORT`. Default is `3000`. Set this variable to change the TCP port to be bound.
- `MONGO_URL`. This should point to the same mongoDB instances used by the referenced [restQL-http](https://github.com/B2W-BIT/restQL-http).

## Development server


To install restQL manager dependecies run:

```shell
yarn install
```

To start the development server, run:

```shell
yarn server:start
```

In another shell, run:

```shell
yarn start
```

Access http://localhost:5000/.

## Production build

To build a production bundle, run:

```shell
yarn build
```

You can now start the server:

```shell
node src/server
```

restQL-manager will be available at `http://localhost:3000/`

## restQL Manager Endpoints

By default the restQL Manager run on port 3000.

#### Web Admin

`GET /` A Web interface for running and saving queries.

#### Saving Queries

`POST /ns/:namespace/query/:queryId` with the query as the POST body, where `:namespace` is the namespace you want to publish the query and `:queryId` is your query unique name inside that namespace.

#### Retrieving Available Resources

RestQL-server has a route to retrieve all the mapped resources.

The route `GET http://localhost:3000/resources/:tenant` will return the following response:

```json
{
  "resources": 
    [
      {
        "url": "https://swapi.co/api/planets/:id",
        "base-url": "https://swapi.co",
        "status": 200,
        "name": "planets"
      },
      {
        "url": "http://api.magicthegathering.io/v1/cards/:id",
        "base-url": "http://api.magicthegathering.io",
        "status": 200,
        "name": "card"
      }
    ]
}
```

Where `status = 200` means that the resource is reachable.

#### Retrieving Saved Queries

RestQL-server has a route to retrieve all the saved queries of a given namespace.

The route `GET http://localhost:3000/ns/:namespace` will return the following response:

```json
{
  "queries": [
    {
      "id": "cards",
      "revisions": "/ns/foo/query/cards",
      "last-revision": "/ns/foo/query/cards/revision/7"
    },
    {
      "id": "cardsByType",
      "revisions": "/ns/foo/query/cardsByType",
      "last-revision": "/ns/foo/query/cardsByType/revision/1"
    }
  ]
}
```

Where:

- id: the query id to be used when running the query, e.g. `GET http://localhost:9000/run-query/ns/foo/query/cards/revision/1`.
- revisions: the server route to retrieve all revisions of the given query.
- last-revision: the route to retrieve the last revision's saved query string.

#### Retrieving Saved Queries Revision

RestQL-server has a route to retrieve all revisions from a saved query.

The route `GET http://localhost:3000/ns/:namespace/query/:queryId/`, with a given `queryId = cards`, will return the following response:

```json
{
  "revisions": [
    {
      "index": 3,
      "link": "/run-query/ns/foo/query/cards/revision/3",
      "query": "/ns/foo/query/cards/revision/3"
    },
    {
      "index": 2,
      "link": "/run-query/ns/foo/query/cards/revision/2",
      "query": "/ns/foo/query/cards/revision/2"
    },
    {
      "index": 1,
      "link": "/run-query/ns/foo/query/cards/revision/1",
      "query": "/ns/foo/query/cards/revision/1"
    }
  ]
}
```

Where:

- index: the revision number
- link: the route to run the query saved for the given revision.
- query: the route to retrieve the revision's saved query string.

#### Retrieving Saved Queries String

RestQL-server has a route to retrieve the query string from a saved query, given a revision index.

The route `GET http://localhost:3000/ns/:namespace/query/:queryId/revision/:index`, given `queryId = cards` and `index = 1`, will re