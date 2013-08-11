# Data in the Cloud

The cloud supports a vibrant "database as service" ecosystem, with myriad options ranging from traditional RDMBSs to various NoSQL implementations. These services make it easy, affordable, and quick to build legitimate data driven applications that are capable of rapid scalability without deep operational expertise. 

In this hands-on workshop, we will go over the options available for leveraging a number of NoSQL data stores including [MongoDB](http://www.mongodb.org/), [Redis](http://redis.io/), and [Amazon's DynamoDB](http://aws.amazon.com/dynamodb/). Whats more, we'll also look at more traditional relational database options available to you in the cloud. You will learn how to work with these datastores, the pros and cons of each datastore type, and what service providers are available to you. With this knowledge, you'll be armed with the information needed to build the next killer app!


## Let's get started! 

This workshop is organized into 3 labs. We don't have a lot of time, so I strongly suggest you pair with someone. People with OSX, please volunteer to be pairing partners as it'll reduce friction because I'm also on OSX. That is, if you have Windows or Linux and someone near you is on a MacBook, then you should probably ask to pair with that person as my directions are tailored to OSX. 

### Lab #1

In [Lab #1](/labs/lab_1/README.md), you'll become familiar with [MongoDB](http://www.mongodb.org/).

### Lab #2

In this lab, you'll become familiar with Redis, which is

>an open source, BSD licensed, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets.
>> [redis.io](http://redis.io/)

If you've ever worked with or are familiar with [Memcached](http://memcached.org/), then you'll quickly see the benefits of Redis as it is quite similar to Memcached in that all values are stored in memory. This makes data access _extremely_ fast. Where Redis is different is that the values of keys can be _more than_ just strings (as alluded to in the description of Redis!). Finally, Redis can persist its data to disk, supports master-slave relationships, and [clustering is due to come out soon](http://vimeo.com/63672368).

Redis makes a great solution for caching; however, it can also be used as a primary data store (provided your data can _fit_ into main memory). Because Redis allows you to store data structures, you can model out objects (via Hashes, usually), etc and there are a number of ORM-like libraries available to do just this. 

#### Getting started with Redis

There's two options when getting started with Redis: you can download and install it (which is quite easy) or you can use an online interactive shell (that only supports _some_ of the standard features). If you are on an OSX machine, I would recommend installing Redis; plus, installing Redis will give you the the `redis-cli` shell command, which you'll want for interacting with cloud-based Redis instances. 

### Installing Redis

Go to the [download section of the Redis website](http://redis.io/download), grab your platform's binary, unzip it, change directories into the root install directory and type:

```
$> make
```

Next, to start Redis, within the root directory of your Redis installation type:

```
$>  src/redis-server
```


Now Redis is up & running. 

##### Using the `redis-cli` shell

Now open up another terminal and change directories into where you installed Redis. Now type:

```
$> src/redis-cli
```

### Using the online shell (if you did not install Redis locally)

Go to [try.redis.io](http://try.redis.io/). Remember, this shell doesn't support _all_ the commands you can normally run in a local Redis shell; what's more, a few of the features we'll cover in this workshop *might* not be supported. 

![try redis](/docs/imgs/try-redis.png)

### Working with Redis

As Redis is a data structure server, there are a host of ways to store data -- you can store things as simple strings and associate them via a key. Or you can store lists, sets, and even hashes, just to name the most common ones. In this workshop, you'll see these basic data structures in action. 

If you don't have a terminal fired up and haven't already run `redis-cli`, please do so. Or you can go to [try.redis.io](http://try.redis.io/) as well. Either way, let's start simple. 

##### `GET` & `SET`

First type the following: 

```
set test "some value"
```

In this case, you've associated the string "some value" to the key `test` using the `set` command. Want to get the value for a simple string key? Watch:

```
get test
```

What do you see? 

`set` doesn't have to set a string value and keys can be any string you'd like. 

Redis is really good for real-time analytics because everything is stored in memory; plus, it works really well with numerical values. Watch:

```
set "60 Minute IPA" 1
```

What does the above command do? 

Now, I'd like to increment the integer value -- I could `set` 2 or I could:

```
incr "60 Minute IPA"
```

What's the value of  "60 Minute IPA" now? 

__Question__: How do you think you can decrement the value of "60 Minute IPA"?

Ponder this: why would you want to associate an integer with a string and then continually increment it or decrement it? What good does that do you? 

For more information on [Strings](http://redis.io/commands#string), see the Redis [documentation](http://redis.io/commands#string).

##### Lists

Keeping with the beer theme, I'd like to start to store the beers I'm drinking (from day to day, mind you). For example, I can keep a _list_ of beers I drank for a particular week and then I can find out what were the first three, etc. I can do this by using a Redis list. 

Run the following command:

```
lpush "recent::beers" "Guinness"
```

__Question__: What is the key in this case? 

`lpush` is a list command and it means push a value onto the list from the left -- i.e. put the value _on top_ like a stack. 

__Question__: How do you think you add a value to the _tail_ of a list? 

Let's add a few more: 

```
lpush "recent::beers" "60 Minute IPA"
```

and 

```
lpush "recent::beers" "Chimay Blue"
```

and 

```
lpush "recent::beers" "Fat Tire"
```

Lists support _ranges_ -- that is, you can select a subset of the elements in them. List indexes are 0 based. Thus, the item on top (i.e. the last item pushed via a `lpush`) is index 0. 

`lrange` means list range and takes a start value and end value. For example, if you wanted to get the first item from a list, you would pass in a 0 for the start value and a 0 for the end value. 

Thus, the _most recent beer_ can be obtained like so:

```
lrange "recent::beers" 0 0
```

__Question__: How would you obtain the first beer added (i.e. Guinness)?


__Question__: How would you get the most recent three beers I drank? 

__Question__: How would you get the first two?


For more information on [Lists](http://redis.io/commands#list), see the Redis [documentation](http://redis.io/commands#stringhttp://redis.io/commands#list).


##### Keys

By now, you've got at least two keys in Redis. You can always see your keys via the command: 

```
keys *
```

And you can see the  type of a key like so:

```
type key_name
```

Keys can have a TTL (time-to-live). Just set it via the `expire` command like so:

```
expire "recent::beers" 60 
```

`expire` takes an integer value representing seconds. 

```
keys *
1) "60 min IPA"
2) "recent::beers"
3) "test"
4) "favorite::beers"
5) "favorite_beers"
```

Wait about a minute and then:

```
keys *
1) "60 min IPA"
2) "test"
3) "favorite::beers"
4) "favorite_beers"
```

Notice one is missing? You can always obtain the TTL for a key via the `ttl` command.

sets:

redis 127.0.0.1:6379> sadd "favorite::beers" "Guinness"
(integer) 1
redis 127.0.0.1:6379> sadd "favorite::beers" "Guinness"
(integer) 0

edis 127.0.0.1:6379> sadd "favorite::beers" "60 Minute IPA"
(integer) 1
redis 127.0.0.1:6379> sadd "favorite::beers" "Sierra Nevada Pale Ale"
(integer) 1
redis 127.0.0.1:6379> sadd "favorite::beers" "Dead Guy Ale"
(integer) 1
redis 127.0.0.1:6379> smembers "favorite::beers"
1) "60 Minute IPA"
2) "Guinness"
3) "Sierra Nevada Pale Ale"
4) "Dead Guy Ale"



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
    * [What is Redis and what do I use it for?](http://stackoverflow.com/questions/7888880/what-is-redis-and-what-do-i-use-it-for)
    * [Java development 2.0: Redis for the real world](http://www.ibm.com/developerworks/library/j-javadev2-22/)
    * [Redis Tips](https://developer.mozilla.org/en-US/docs/Mozilla/Redis_Tips)
    * [The Disco Blog: Redis](http://thediscoblog.com/blog/categories/redis/)


## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US">Creative Commons Attribution-ShareAlike 3.0 Unported License</a>.   
