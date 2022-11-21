# How to load huge csv files to Neo4j database
Example from [official docs](https://neo4j.com/docs/operations-manual/current/tutorial/neo4j-admin-import/):  
`bin/neo4j-admin database import full --nodes=import/movies.csv --nodes=import/actors.csv --relationships=import/roles.csv neo4j`  
Call it from installation directory of your neo4j. It works only for empty database.

# What does not work (at least for me)
## Works, but takes too long
There are a lot of obsolete and misleading instructions. 

First thing you probably found is `LOAD CSV` clause. Well, it works, but if your graph is somewhat large it will take eternity to finish.

Then you might found `USING PERIODICAL COMMIT` -- it's obsolete. Database will hint you to use `CALL {...} IN TRANSCTIONS`. It's may be not quite clear how to use it properly. Full example should look like this:
```cypher
:auto LOAD csv with headers from 'file:///nodes.csv' as row
CALL {
WITH row
MERGE (:Channel {label:row.Label, id:toInteger(row.id)})
} IN TRANSACTIONS;
```
It's much faster. But for graph with 200K nodes it's about 10 hours. And still eternity for edges.

## Does not work complitely
Then. If you are insistent enough you may find `apoc.import.csv`. Well, it does not work out of the box. First you need to install apoc plugin. Then create `conf/apoc.conf` add this lines:
```
apoc.import.file.use_neo4j_config=true
apoc.import.file.enabled=true
apoc.import.file.use_neo4j_config=true
```
Also add this lines to `conf/neo4j.conf`:
```
dbms.security.allow_csv_import_from_file_urls=true
```
and maybe change `dbms.directories.import` to absolute path. But It still does not work and I also found a lot of people asking on SO and forums how to configure it properly. Somebody says that absolute import path helps, but other complain that it doesn't help.