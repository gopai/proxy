## Brief Description
<p>•	The Client prompts the user to enter an SQL statement, the beginning value of the row set, and the ending value of the row set. 
The SQL statement identifies the table the rows should be retrieved from and the beginning and ending values define the range of the 
row set the user wants displayed.</p>
<p>•	The Proxy not only fetches data from the database, but also caches the data. Metadata about the rows are retrieved by ResultCache 
which also has the ability to filter a row set and output the original row set.</p>

## Cache
<p>•	Caching is the process of storing previously requested data for use some time in the future. This is the main benefit of the 
Proxy.</p>
<p>•	Caching data ensures efficiency of the program by utilizing less storage but completing requests quickly.</p>
<p>•	Here is a representation of the Client-database communication</p>

![alt text](https://github.com/gopai/proxy/blob/master/Proxy.png)

<p>•	The Client initially requests information, entered by the user, from the database. This can be an entire row set or just a portion 
of the row set. The arrows labeled ‘A’ represent the Client-to-database route with the Client as the requester and the database as the 
supplier of the data.</p>
<p>•	From the diagram, we can see that the Proxy, given its position, acts as a literal middleman. The Proxy prevents direct 
communication between the Client and the database. Instead, when the Client first asks for some data, the Proxy takes this request and 
sends it up to the database. The database receives the Client’s request via the Proxy and hands back the entire table to the Proxy. 
This marks the end of the ‘A’ arrows and the start of the ‘B’ arrows.</p>
<p>•	The Proxy receives the data but instead of proceeding to the final ‘B’ arrow it takes a slight detour to the ‘C’ arrow pointing 
at the cache. This is where the whole table is stored. The Proxy then picks out the chunk the Client had requested and goes on to 
complete the rest of the ‘B’ journey handing the Client the correct number of rows.</p>
<p>•	When the Client requests for a different row range but from the same table, the Proxy receives the request but instead of going 
up to the database like it did before, it pulls the new row set from the cached data.</p> 

## Dealing with Cache Invalidation
<p>•	Cached data can go out of date when the original data in the database gets modified, this is known as cache invalidation.</p>
<p>•	The Proxy handles cache invalidation by setting an expiry date and time on the cached data.</p>
<p>•	When the Server is started, the current time and the expiry time are displayed.</p>
<p>•	As expected, when the current time is after the expiry time no rows are displayed. Instead, the Server prints out a message 
letting the user know that the cache has expired. Once the cached data has expired, a new expiry date and/or time needs to be set then 
the Server should to be restarted.</p>
<p>•	Restarting the Server forces the Proxy to retrieve the table from the database again ensuring that the data is current.</p>
<p>•	An advantage of setting an expiry date and time for cached data is that it gives the user flexibility. Since the expiry values 
are set manually, the user can provide a nearer expiry time for volatile tables and an expiry time further into the future for tables 
that rarely change.</p>

## RAM Storage for Cached Information vs. Permanent Storage
<p>•	RAM storage for cached information can be accessed quickly.</p>
<p>•	This speed comes at a cost, literally. RAM is expensive and as a result computers don’t have much of it. At least not enough to 
pull all records from a database table.</p> 
<p>•	Being an issue, space needs to be used wisely. Populating the entire row set and returning it to the Client would result in 
memory running out and the program failing.</p>
<p>•	Permanent storage on the other hand is cheap and so computers have a lot more of it compared to RAM.</p>
<p>•	The trade-off however, is speed.</p>
<p>•	RAM storage for cached data reduces latency associated with processing which ultimately improves user experience through faster 
performance.</p>