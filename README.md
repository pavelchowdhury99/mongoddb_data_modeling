# MongoDB Data Modeling: Best Practices and Tips

1. Using separate db for different projects.
    - If 2 microservices are interacting each same datasets then they should write to same DB
2. Utilising collections.
    - Collections should be treated as RDBMS tables not like schemas'.
3. Understanding of all the [operators](https://www.mongodb.com/docs/manual/reference/operator/) and methods.
    - This would empower you to create succinct queries.
4. Embedding <br>
   - It can be compared with denormalised tables.
        ```
        {
            "id": 1,
            "name": "Test Test",
            "course": {
                "type": "B.Tech",
                "stream": "CSE",
                "year":3
            }
        }
        ```
5. Referencing
    - It can be compared to normalised table where you need to perform joins ([lookup](https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup/#mongodb-pipeline-pipe.-lookup)) for retreiving complete data.
6. Keeping the type of operation, documents size, performance thoughput required and future scalibility in mind, test various approached using ```.explain("executionStats")``` method.
7. General guidlines<br>
    1. For 1:1 relations
        -  Keeping the primary key (ObjectId) of the referenced document in the doc should suffice. 
        - However if reference doc is not going to queried without the primary document then only storing the ObjectId of the reference document in the primary document would save storage.
        - If the referenced document is of reasonable size and performance is not affected  post embedding then that would be a cleaner approach.
    2.  For 1:N where N is limited and in reasonable size limit
        -  Embedding would be suitable
    3.  For 1:N where N not limited or beyond reasonble size
        -  Referencing would be suitable with reference of the primary document in each of the referenced document.
    4. For 1:N where N is ever growing
        - Store each of the child document in an array inside another document and have the reference of the parent doucment in that.
8. Using [Indexes](https://www.mongodb.com/docs/manual/indexes/)
    - If your application has higher read-to-write ratio and if queries often use a field as filter creating indexes would improve execution time.
    - It comes at a cost of additional storage.
    - [References](https://www.mongodb.com/docs/manual/indexes/#additional-considerations)
9. Using [Replication](https://www.mongodb.com/docs/manual/replication/)
    - It would provide fault tolerance.
    - It would improve read performance.
    - It might come at a cost of replication lag and read disparities when writes are not replicated yet.
10. Using [Sharding](https://www.mongodb.com/docs/manual/sharding/)
    - This is MongoDB's implementation of horizontal scaling.
    - It would distributed workloads among multiple machine thus increasing the read/write performance.
    - However once sharded, there's is not native way to unshard in MongoDB, so one must carefully plan before using this.