# Jelass (jĕl′əs), a Linearly Scalable, Searchable, NoSQL and Graph Database
## Jelass = JanusGraph + Elassandra (Elastic Search + Cassandra) 

Elassandra stores Elastic data on Cassandra. So there's no double up on this system. Cassandra is the boss. Elastic runs on top of it and allows it to be useful (searchable, querying etc.). Janus comes to town and adds all the graph functionality LinkedIn could ever need. All under the one roof.

How is this different from straight Janus? Janus' elastic data isn't stored in Cassandra. This has all 3 together. Bullet-proof. 

## Download Docker Image

https://hub.docker.com/repository/docker/sfproductlabs/jelass

## Running & Ready for Production
- [x] Docker with SSL by default
- [x] Nginx SSL for elastic search (Available on port 443 & port 9343, using nginx reverse proxy)
- [x] Cassandra client and server keystores by default
- [ ] TODO: add nginx streaming SSL for tinkerpop on 8182

## Connecting
- `cqlsh --ssl`
- Remotely: `./bin/gremlin.sh` then `:remote connect tinkerpop.server conf/remote.yaml`
- Or locally: `./bin/gremlin.sh` then `graph = JanusGraphFactory.open('conf/gremlin-server/janusgraph-cql-es-server.properties')` 
- etc.

## Starting out
Then try the basic demo:

On the console hosting docker run:
```bash
docker ps
#then replace [container_number] with your docker container hash
docker exec -it [container_number] bash
```
Then inside the docker container:
```bash
cd /app/ela/janusgraph-full-0.5.2
./bin/gremlin.sh
```

Then inside `gremlin>`:

```gremlin

GraphOfTheGodsFactory.load(graph)
g = graph.traversal()
saturn = g.V().has('name', 'saturn').next()
g.V(saturn).valueMap()
g.V(saturn).in('father').in('father').values('name')
```
or access the data **remotely** in a remote gremlin console `./bin/gremlin.sh` (you may need to change the ip):
```
:remote connect tinkerpop.server conf/remote.yaml
:> saturn = g.V(g.V().has('name', 'saturn').next()).valueMap()
```
