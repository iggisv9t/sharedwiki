# Memgraph
## Data import
The only way I found for data import from csv is a standard `LOAD CSV` cypher clause. It import 200K nodes for a couple of minutes, but I failed to upload the edges. For ~1M it was estimated for a few dates.  
I used a docker to run Memgraph. And don't forget to create index on nodes IDs.

It does not persist data between runs, so every rerun you have to copy data into container. There is also `.cypherl` import, so you can load previously dumped database from Memgraph lab UI.

## Overall impressions
It's look promising. Cypher runs much faster then in neo4j and lab looks more feature rich then UI in neo4j, but I had no chance to try it in work.  
Docker installation is super easy.
