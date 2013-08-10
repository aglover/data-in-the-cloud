# Data in the Cloud

The cloud supports a vibrant "database as service" ecosystem, with myriad options ranging from traditional RDMBSs to various NoSQL implementations. These services make it easy, affordable, and quick to build legitimate data driven applications that are capable of rapid scalability without deep operational expertise. 

In this hands-on workshop, we will go over the options available for leveraging a number of NoSQL data stores including [MongoDB](http://www.mongodb.org/), [Redis](http://redis.io/), and [Amazon's DynamoDB](http://aws.amazon.com/dynamodb/). Whats more, we'll also look at more traditional relational database options available to you in the cloud. You will learn how to work with these datastores, the pros and cons of each datastore type, and what service providers are available to you. With this knowledge, you'll be armed with the information needed to build the next killer app!


## Let's get started! 

This workshop is organized into 3 labs. We don't have a lot of time, so I strongly suggest you pair with someone. People with OSX, please volunteer to be pairing partners as it'll reduce friction because I'm also on OSX. That is, if you have Windows or Linux and someone near you is on a MacBook, then you should probably ask to pair with that person as my directions are tailored to OSX. 

### Lab #1

In Lab #1, you'll become familiar with MongoDB, which is 

>a document database that provides high performance, high availability, and easy scalability.
>> [Introduction to MongoDB](http://www.mongodb.org/about/introduction/)

MongoDB is document-oriented; thus, there are no tables with rows, but simply JSON documents. There are no joins either. This might seem scary at first, however

>document-oriented (or schemaless) data is simpler and far more flexible to manage than relational data. Rather than storing data into a rigid schema of tables, rows, and columns, joined by relationships, documents are written individually, containing whatever data they require.
>> [Java development 2.0: MongoDB: A NoSQL datastore with (all the right) RDBMS moves](http://www.ibm.com/developerworks/library/j-javadev2-12/)

Like traditional RDBMSs, MongoDB supports ad-hoc queries and collections (i.e. tables) can be indexed. MongoDB supports high availability via [replica sets](http://docs.mongodb.org/manual/replication/) and easy scalability via [automatic sharding](http://docs.mongodb.org/manual/sharding/) (i.e. it's done _via MongoDB_ and [not by you](http://www.ibm.com/developerworks/library/j-javadev2-11/) or [your code](http://www.javaworld.com/community/node/4797)). 

#### Getting started with MongoDB

There's two options when getting started with MongoDB: you can download and install it (which is quite easy) or you can use an online interactive shell (that only supports _some_ of the standard features). If you are on an OSX machine, I would recommend installing MongoDB; plus, installing Mongo will give you the the `mongo` shell command, which you'll want for interacting with cloud-based MongoDB instances. 

##### Installing MongoDB

Go to the [download section of the MongoDB website](http://www.mongodb.org/downloads), grab your platform's binary, unzip it, optionally create a data directory (where MongoDB writes the contents of your datastore -- this can be an existing directory if you'd like), and then start an instance via the `mongod` command. You'll need to point the `mongod` command to where you want Mongo to write your data (that's the data directory I mentioned earlier) like so:

```
$> ./bin/mongod  --dbpath ./data/db/  -v
```

Note, I'm running the `mongod` from within the root of where I installed MongoDB. The `--dbpath` points to where I'd like the data to reside and the `-v` flag indicates I'd like verbose output -- helpful in watching what's going on.

Now MongoDB is running. 

##### Using the `mongo` shell

##### Using the online shell

Go to [try.mongodb.org/](http://try.mongodb.org/). Remember, this shell doesn't support _all_ the commands you can normally run in a local Mongo shell; what's more, a few of the features we'll cover in this workshop are not supported. 




### Lab #2

### Lab #3


## Helpful Resources
  
  * [MongoDB](http://www.mongodb.org/)
    * [Try MongoDB](http://try.mongodb.org/)
    * [MongoHQ](https://www.mongohq.com/home)
    * [MongoLab](https://mongolab.com/welcome/)
    * [Java development 2.0: MongoDB: A NoSQL datastore with (all the right) RDBMS moves](http://www.ibm.com/developerworks/library/j-javadev2-12/)
    * [The Disco Blog: MongoDB](http://thediscoblog.com/blog/categories/mongodb/)
  * [Redis](http://redis.io/)
    * [Try Redis](http://try.redis.io/)
    * [Redis Cloud](http://redis-cloud.com/)
    * [OpenRedis](https://openredis.com/)
    * [Redis To Go](http://redistogo.com/)
    * [Java development 2.0: Redis for the real world](http://www.ibm.com/developerworks/library/j-javadev2-22/)
    * [The Disco Blog: Redis](http://thediscoblog.com/blog/categories/redis/)


## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US">Creative Commons Attribution-ShareAlike 3.0 Unported License</a>.   
