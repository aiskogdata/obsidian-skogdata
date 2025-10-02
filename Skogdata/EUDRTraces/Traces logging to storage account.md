
## Limits
key storage acount blob storage limits
- max 250 per region per subscriptoion
- max 5000 per dns zone enpoint per subscriptiong
- no limit to count on blob containers, blobs, directories, subdirectories, fileshares, tables, queues, entities, messages per account
- 20000 requests per second per account (this might be an issue when we look at glommen mjosen added with all others)max ingress per v2 and blob storage 60Gbps (as litle as 10 or as much as 200 in other regions)
- egress can be as low as 50 Gbps, but is 200 for norway east
- max umber of IPs 400max nr virtual network rules 400
- max nr of resource istnace rules 200
- max nubmer of private endpoint 200 per stroage account for all
- storage blobs allow for maximum of 1024 characters for a blob name https://learn.microsoft.com/en-us/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadatamax 
	- 254 path segments, 63 if hierarchical, any / will cause hierachical storing if enabled
	- https://learn.microsoft.com/en-us/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#metadata-key-and-value-namesmeta 
		- data must start wtih a letter or under score and be valid ascii
		- maybe use blobs for storage and metadata for search as metadata should be KQL queriable?
- KEY stroage table limits
	- max 500 tib per table
	- max 1 MiB per entity including all values
	- max 255 poperties per entitymax size per properity https://learn.microsoft.com/en-us/rest/api/storageservices/understanding-the-table-service-data-model  
		- key being a string max 64KiB size and 32K characters
		- and byte array of max 64KiB
		- partiction key is max 1024 characters, row key is 1024 characters
		- max 100 entities per grup transaction and max 4 MiB payload, can only update a given entity once
		- max 5 access policies per tablemax 20000 transactions per second, at 1Kib entity sizeMax 2000 1KiB entiteis input into a single table partition per second
			- https://learn.microsoft.com/en-us/rest/api/storageservices/addressing-table-service-resourceshttps://learn.microsoft.com/en-us/rest/api/storageservices/querying-tables-and-entitiescan only run 15 discrete comparison in filters
			- does not sopport the Odata 'Additional Query Options' featurequerying is often done through url https://myaccount.table.core.windows.net/Customers(PartitionKey='MyPartition',RowKey='MyRowKey1')querieing through LINQ and C# the client requires OData Data Service Version 3 
- Microsoft recommends blob storage fo logs and considers table storage of logs to be an anti pattern https://learn.microsoft.com/en-us/azure/storage/tables/table-storage-design-patterns#log-data-anti-pattern  due to hot partitions aka being blocked from accessing log data 


## Change suggestions
### Current state
As things stand right now we intend to use the devopsstorage storage account to store the data and create a new container for each of the stages:
dev, test, staging, prod with the filename being ala {client}_{orderid}_{request/response type} (where request and response types are add, update,
and remove)

### Storage account usage
First I suggest we use a new storage account. They effectively cost nothing to just have, and we don't want to tie the life times and management of two completely different business cases together. Plus if we ever need to give someone access to everything, we don't have to worry about access to other sensitive info from out AI and other logging data, nor do we risk AI doing any management on the data. This also makes any future moving or refactoring easy.
And we can still use Log analytics to monitor the storage account through Monitoring workbooks, no problem.

### Storage Structure
Secondly, I suggest we change how we store data from being a container per environment, to a container per vh_client. This lets us utilize the container-level access control features to provide our customers with access to the data through SAS token per client, instead of us having to 
export the data, run particular access control via users or similar, while still retaining the option to kill access at any time.
It also allows us to use CORS rules more sensibly (per container instead of per blob) and potentially allow SFTP access in a sensible manner.

### Naming Convention
If we enable hierarchical spaces we can now functionally seperate each environment by folder as namespaces allows for `container/directory/directory/name.log` style naming that logically sorts the blobs into folders. This comes with the limitation of max 63 directories, and each directory, including the name can have a max length of 1024 characters.
This can now allow us a naming convention of `{client}/{Environment}/{orderid}/{requestType}_{responseType}_{unix_timestamp_ms}.log"`
or 
`{client}/{Environment}/{orderid}/{requestType}_{responseType}_{transaction_guid}.log"`
THIS IS CRITICAL AS NAMES NEED TO BE UNIQUE WITHIN A BLOB, which our naming convention does not guarantee

### other notes
Ideally I think either we create a storage account for each environment for proper seperation of data and concerns, or even more ideally one per customer per environment

- Azure storage blobs uses Tags as an auto indexed and searchable metadata. Each blob can have up to 10.
This is what we want to use if we want to make the data easily searchable. This is not supported with hierarchy namespace enabled.
    -Setting index tags can be done through .SetTags(), deleting them can be done by passing in an empty dict.
- Blobs also support a normal metadata field which is a Dictionary<string,string> which is not automatically indexed but supports
    - They need to be value HTTP header values https://learn.microsoft.com/en-us/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#metadata-names
    - more reference https://learn.microsoft.com/en-us/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources
    - metadata I suggest: source client (aka which app), production environment, timestamp, 


