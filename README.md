# learn-system-design

## 资料
- 帖子: 
    - https://blog.csdn.net/AuburnTigers/article/details/102601151
    - https://blog.csdn.net/dianxiangong2403/article/details/101879652?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-3.no_search_link&utm_relevant_index=6
- [第一步](https://www.hiredintech.com/classrooms/system-design/lesson/55)
- 第二步: Designing Data-Intensive Applications
- 第三步: https://github.com/donnemartin/system-design-primer

## Design process
1. make sure you know all the requirements the interviewer didn't tell you about in the beginning
    - Ask yourself "What does it do?"
    - Then, 
        - write the use case and 
            - `As a ... (user), I would like to do ... so that I can ...`
        - constraints
            - QPS, TPM
            - volume of data stored
            - network qos
                - throughput
                - latency
                - packet loss
            - failure rate/availibility
2. design the application service/components (follow below order)
    - data storage layer (DB)
    - service layer
        - XXX service
    - controller layer
        - /api/{...}
    - connection layer
3. List possible bottlenecks for each layer
    - need a load balancer?
    - horizontal scaling DB?
4. discuss the scalability of the design
    - Vertical scaling: 给一台机子升级配置, 但总有个极限的
    - Horizontal scaling: replicate server for the same service in order to serve more requests
    - Load balancing: decide which replicate server to serve this request (least busy? most busy? round robin?)
        - remember a large number for request executing on server X rather than maintaining sessions or storing private IP in cookie
    - Caching
        - needs to consider TTL (time to live) for GC because you can run out of thel limited space
        - Using NoSQL instead of scaling a relational database
            - Cache Database Query result
    - Database replication: making automatically copy of DB
        - Master-slave
            - a slave is a copy of the master
            - balance read requests to slaves
            - master can be single point of failure (solved by master slave switch)
            - slaves can enchance read service's availibility
        - Master-master
            - write on one master will also be execute on other masters
            - solve single point of failure for master-slave
    - Database partitioning: partition DB to different servers and work as a whole
        - Database sharding: let part of users pre-determined to be served by a small server rather than a centralised service (e.g. serve users geographically)
            |advantage||disadvantage||
            |---|---|---|---|
            |High availability|one shard down -> others still work|Rebalancing data| how to make sure data are balanced through all  shards?
            |Faster queries|small data set -> faster query speed|Joining data from multiple shards| too much small data sets -> large join times
            |More write bandwidth|you can write data in different shards -> more write bandwidth compared to master-slave|How do you partition your data in shards?|difficult to decide which field to be stored on which shard
            |
    - Being asynchronous
        - use MQ to consume requets asynchronously

## concepts
|||
|---|---|
|Reliability|The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or software faults, and even human error)
|Scalability|As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.
|Maintainability|Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.

### reliability
1. The application performs the function that the user expected.
2. It can tolerate the user making mistakes or using the software in unexpected ways.
3. Its performance is good enough for the required use case, under the expected load and data volume.
4. The system prevents any unauthorized access and abuse.

||||
|---|---|---|
|faults|one component of the system deviating from its spec| cannot decrease to 0 -> keep fault-tolerant or resilient
|failure|the system as a whole stops providing the required service to the user

#### Hardware Faults
硬盘坏了, 没电了, etc -> multi-machine redundancy
#### Software Errors
just fix bugs
#### Human Errors
1. write clear docs for API
2. different sandboxs for dev, testing, prod
3. thorough testing
4. Set up detailed and clear monitoring, such as performance metrics and error rates.

### scalability
system’s ability to cope with increased load




