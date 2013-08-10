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

Now open up another terminal and change directories into where you installed Mongo. Now type:

```
$> ./bin/mongo
```

##### Using the online shell (if you did not install Mongo locally)

Go to [try.mongodb.org](http://try.mongodb.org/). Remember, this shell doesn't support _all_ the commands you can normally run in a local Mongo shell; what's more, a few of the features we'll cover in this workshop are not supported. 

##### Working with Mongo

As indicated earlier, MongoDB is a document-oriented database; what's more, those documents are JSON documents. Accordingly, MongoDB works directly with JSON (which makes this database a _great_ fit with JavaScript). Thus, to define a document, you simply define it in JSON like so: 

```
{
 some_key: "some value",
 some_list: ["a", "b", "c"]
}
```

This document can be directly saved into Mongo -- note the usage of a list as a value. 

For this workshop, we're going to work with _business cards_; that is, these are often great examples of a direct fit for document-orientation because it's somewhat challenging to manage their data in an RDBMS (as card formats can vary; thus, leading to a lot of `null` column values, for example)

###### Inserts

In the `mongo` shell, type (or copy and paste!) the following: 

```
card = {
  name: "Andrew Glover",
  mobile: "703-555-5555",
  twitter: "aglover",
  email: "ajglover@acme.com"
}
```

Now, you can insert the document into what Mongo dubs a _collection_, which is like a table. We'll call this collection `business_cards`. To insert the `card`  document, type (or copy & paste) the following:

```
db.business_cards.insert(card)
```

That just "persisted" a document. I put persisted in quotes because in fact, for a few moments after you insert, the document is actually _in memory_ and later will be written to disk. You can control this behavior, however, via some flags; that is, you can force a write to disk on insert as well via various language drivers.

Now create another three cards in the same manner as above. Here are the cards to insert:

```
{
  name: "Lee Glover",
  mobile: "321-555-5555",
  twitter: "lglve",
  email: "lee@acme.com",
  blog: "thediscoblog.com"
}

{
  name: "Emily Smith",
  mobile: "301-555-5555",
  email: "esmith@acme.com"
}

{
  name: "Mike Smith",
  mobile: "301-555-5555",
  home: "801-555-5555",
  email: "esmith@acme.com",
  linkedin: "http://linkedin.com/msmtih",
  twitter: "smith"
}
```

Notice how each business card in this case is slightly different -- some have `twitter` as an field, some have `linkedin`, etc. 

Now that you've inserted a few documents, type the following command to see how many are in the `business_cards` collection:

```
db.business_cards.count()  //FYI, this won't work on try.mongodb.org
```

You should see at least 4 if you inserted the documents I suggested. (Feel free to insert more, however)

###### Querying

Querying for data is what a database is all about, right? To find a document, you simply fashion an ad-hoc query. Supply an attribute name and value. For example, to find a document with a `name` attribute whose value is "Andrew Glover" you'd type:

```
db.business_cards.find({name:"Andrew Glover"})
```

__Question__: How would you find a document (or business card) whose `twitter` value was "smith"? 

Notice how the documents we inserted don't all have a `twitter` attribute defined. You might wonder how you'd then find only those documents that have a `twitter` attribute. Mongo's query language supports some interesting operators; in this case, you can use the `$exists` operator like so:

```
db.business_cards.find({twitter: {$exists:true}})
```

__Question__: How would you find the documents that _do not_ have a `linkedin` attribute? 


Are you a bit tired of mentally parsing that JSON? Wouldn't it be easier if that JSON was pretty printed? Not a problem! Use the `forEach` function, which iterates over a collection of returned documents and pass in the `printjson` value like so:

```
db.business_cards.find({twitter: {$exists:true}}).forEach(printjson) //for each won't work in try.mongodb.org
```

Now isn't that nice? 

But wait, you're tired of seeing the entire document. Not a problem either -- just use _projections_ in Mongo. These allow you to specify what or what you'd not like to see. For example, if you want just the `name` values for documents that have a `twitter` attribute, you can type:

```
db.business_cards.find({twitter: {$exists:true}}, {name:1})
```

The 1 in this case represents what you'd like to see. A 0 means leave the `attribute` out. You can, of course, select multiple attributes too:

```
db.business_cards.find({twitter: {$exists:true}}, {name:1, twitter:1})
```

###### Updates

Want to update a particular document? You can do this as well via the `update` command. This command takes two JSON documents as parameters: the selector, which is what document you wish to update, and an operator. There are a lot of operator options, however, the easiest one is the `$set` operator. This allows you to create a new field. 

For example, to add a `linked` attribute to the document whose `name` field is "Andrew Glover", you can type:

```
db.business_cards.update({name:"Andrew Glover"},{$set: {linkedin:"http://www.linkedin.com/in/ajglover/l"}}) 
```

You can verify the changes like so:

```
db.business_cards.find({name:"Andrew Glover"}).forEach(printjson)
```

__Question__: If the `$unset` operator _removes_ a field, how would you remove the `home` field from the document whose `name` is "Mike Smith"?


##### Using MongoDB in the cloud

There are a couple MongoDB-as-a-service options available to you; I happen to favor [MongoHQ](https://www.mongohq.com/home) as I've had a good experience with them, especially their support. Accordingly, for this workshop, we'll use MongoHQ; moreover, I've created a free database with a user dubbed `modev` with a password of `123456`. 

Using the `mongo` command, you can connect to remote MongoDB instances; for example, if the URL to the database is `dharma.mongohq.com`, it's listening on port 10044, and the database name is `seattle` you can connect to it as follows:

```
$> mongo dharma.mongohq.com:10044/seattle -u <user> -p<password>
```

__Question__: Given the credentials above, can you connect to the MongoHQ `seattle` database? 

__Question__: How many business cards are in the `business_cards` collection? 

__Question__: How many people have a mobile number of "301-555-5555"? Hint: you can always attach a `.count()` to a find query.

__Question__: What are their names of the people who have a mobile number of "301-555-5555"?


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
