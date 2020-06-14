# Data Partitioning

Data partinioning a technique to break up a big database into smaller chunk to improve manageability, performance, availability, load balancing. After a certain point, it is cheaper to scale horizontally, than scaling vertically.


## Partioning Methods

1. Horizonatal (Range based) partioning : Scheme of putting different rows in different tables. If we are sorting places, we can decide to put everything > 1000 into one table, and places with < 1000 into another table. 

2. Vertical partioninoning: Scheme of putting our data in deiffernt tables related to a specific feature. For eg, for a photo based app, put all profile related info in one, photos in one, friend list in another. 

3. Directory based partioning: A loosely coupled approach to create a lookup service which knows your current partioning scheme and abstracts it away from the DB access code. To find a particular data aentity, we query the directory server that hols the mapping to its DB server. 


## Partioning Critiera
1. Key or Hash based partioning: Aplly a hash function to some key attributes of entity we are storing. problem with this appproach is that it effectively fixes the total number of DB servers, since adding new server means changing the hash function/ 
   
2. List partioning: Each partioning is assigned a list of values, so whenever we want to insert a new record, we see which partionin contains our keu and store it there. 

3. Round robin partioning: This strategry ensures uniform data distrbution. With n partioning, i tuple is assigned to partiniotn. 

3. Composite partionng: Combining any of the above scheme to devise a new scheme. 

## Common problems
1. Joins: performing joins on a split database is not feasible. such joins will not be performant. A workaround is to combine data that previously required joins into one table. 

2. Referential integrity: Trying to enforce data integrity constraints such as foreign keys in a partioned database can be extremely difficult. Most of RDBMS do not support foreing keys constraints across DBs on different DB servers which means that apps that require referntial intgraity on partitioned database have to enforce it in application code. 

3. Rebalancing: There could be a lot of load or data in one DB and we may have to change our partioning. 


# Indexes
The goal of indexes is to make it faster to search through the table. Adding index is about improving the performance of search queries. It is useful for read heavy tables. 

# Proxies
Proxy server is an intermediate server between client and back-end server. It can be useful for filtering requests, logging requests, transforming requests, caching. 

Open proxy: it is a server that is open to public. It is of two types - annonymous proxy : one that reverals its identity as a server but does not disclose th initial IP address. It helps in hiding user's IP address and Transprent proxy: A proxy server that identifiees itself and its first IP address can be viewed. It can be useful for caching websites. 

Reverse proxy: It retrieves resources on behalf of client from one or more servers and returns to the client appearing as if they originated form the proxy server itself. 