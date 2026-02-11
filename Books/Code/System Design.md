


1. Users access websites through domain names, such as api.mysite.com. Usually, the Domain Name System (DNS) is a paid service provided by 3rd parties and not hosted by our servers. 
2. Internet Protocol (IP) address is returned to the browser or mobile app. In the example, IP address 15.125.23.214 is returned. 
3. Once the IP address is obtained, Hypertext Transfer Protocol (HTTP) [1] requests are sent directly to your web server. 
4. The web server returns HTML pages or JSON response for rendering


When to use nosql 
• Your application requires super-low latency.

• Your data are unstructured, or you do not have any relational data.

• You only need to serialize and deserialize data (JSON, XML, YAML, etc.).

• You need to store a massive amount of data.


master-slave model, all writes and updates happen in master
nodes; whereas, read operations are distributed across slave nodes.
it allows more queries to be processed in parallel.


In production systems, promoting a new master is more complicated as the data in a slave
database might not be up to date. The missing data needs to be updated by running data
recovery scripts. Although some other replication methods like multi-masters and circular
replication could help, those setups are more complicated


	sidenote: i shoudl talk about hwo my postmortem comments to github was essentailly split brain task 


Consider using cache when data is read frequently but modified infrequently. Since cached data is stored in volatile memory, a cache server is not ideal for persisting data


write through cache
	(e.g., financial transactions where every write must be durable).

write behind cache
	- **Risk of Data Loss:** If the cache fails _before_ the data is written to the primary data source, the data residing only in the cache will be lost. This is the biggest drawback.
	- - Applications with very high write volumes where absolute real-time durability isn't strictly necessary for every single write (e.g., logging, analytics, social media feeds


stateful
	shopping cart is in servers memmory
stateless
	everything is persisted 
		requests are always self contained

State data is stored in a shared data store and kept out of web servers. A stateless system is simpler, more robust, and scalable.


geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user.

Users from different regions could use different local databases or caches. In failover cases, traffic might be routed to a data center where data is unavailable. A common strategy is to replicate data across multiple data centers. A previous study shows how Netflix implements asynchronous multi-data center replicatio![[Screenshot 2026-01-04 at 11.52.25 PM.png]]


On Sharding
Celebrity problem: This is also called a hotspot key problem. Excessive access to a specific
shard could cause server overload. Imagine data for Katy Perry, Justin Bieber, and Lady
Gaga all end up on the same shard. For social applications, that shard will be overwhelmed
with read operations. To solve this problem, we may need to allocate a shard for each
celebrity. Each shard might even require further partition