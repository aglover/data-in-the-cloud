# Data in the Cloud

The cloud supports a vibrant "database as service" ecosystem, with myriad options ranging from traditional RDMBSs to various NoSQL implementations. These services make it easy, affordable, and quick to build legitimate data driven applications that are capable of rapid scalability without deep operational expertise. 

In this hands-on workshop, we will go over the options available for leveraging a number of NoSQL data stores including [MongoDB](http://www.mongodb.org/), [Redis](http://redis.io/), and [Amazon's DynamoDB](http://aws.amazon.com/dynamodb/). Whats more, we'll also look at more traditional relational database options available to you in the cloud. You will learn how to work with these datastores, the pros and cons of each datastore type, and what service providers are available to you. With this knowledge, you'll be armed with the information needed to build the next killer app!


## Let's get started! 

This workshop is organized into 3 labs. We don't have a lot of time, so I strongly suggest you pair with someone. People with OSX, please volunteer to be pairing partners as it'll reduce friction because I'm also on OSX. That is, if you have Windows or Linux and someone near you is on a MacBook, then you should probably ask to pair with that person as my directions are tailored to OSX. 

This will become paramount in Lab #3 as you'll be installing some software packages to facilitate working with Amazon's DynamoDB.

### Lab #1

In [Lab #1](/labs/lab_1/README.md), you'll become familiar with [MongoDB](http://www.mongodb.org/).

### Lab #2

In [Lab #2](/labs/lab_2/README.md), you'll get to know [Redis](http://redis.io).

### Lab #3

In this lab, you're going to learn about Amazon's [DynamoDB](http://aws.amazon.com/dynamodb/). 

DynamoDB is part of the Amazon Web Services platform, which means you interact with Dynamo via HTTP requests -- bindings on top of Amazon's Web Service infrastructure make it possible to leverage Dynamo using the language of your choice. Under Dynamo's licensing, you pay only for the read and write throughput you consume. 

DynamoDB is a key-value store. It's

>a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability.
>> [aws.amazon.com](http://aws.amazon.com/dynamodb/)

You can think of it as essentially a persistent map (or dictionary) -- that is, there is no schema; however, there are limited data types (strings and numbers). There are also no native joins between tables; you can, of course, manage relationships via application code (much like you can do in other non-join supported NoSQL databases, like MongoDB).

The beauty of DynamoDB, of course, is that it's always on and requires almost no maintenance -- _everything_ is handled by Amazon.

#### The structure of DynamoDB

DynamoDB is logically defined as tables containing items. Items contain attributes. You can think of a DynamoDB table much like you would a table in the relational sense. Tables can have many items (which are similar to rows) and items can have 
many attributes (which are like columns in the relational world). Attributes are really just name-value pairs and the "pair" aspect isn't limited to one value. That is, an attribute name can have a list of associated values (this is slightly different than a typical column in an RDMBS, for example).

There isn't a shell-like application to work with Dynamo. You interact with Dynamo either through the Amazon Web Services (AWS) Management Console or via an API. AWS has a series of SDKs for various platforms (i.e. Java, Ruby, etc); furthermore, there are a host of open-source alternative APIs as well. 

Thus, to see how DynamoDB works, I'm going to show you an application that makes use of it. The application is a simple Node.js app that uses a nifty library for working with Dynamo; nevertheless, setup will take a few steps. Hang in there! 

Here we go...

#### Installing Node.js

First and foremost, you'll need to install Node.js -- you can do this in [three steps](http://thediscoblog.com/blog/2013/03/12/node-in-3-commands/):

Step 1: Download and install nvm.

```
$> curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```

Step 2: Reload your shell.

```
$> source .bash_profile
```

Step 2.5: Obtain a list of available node versions to install.

```
$> nvm ls-remote
```

Step 3: Install your desired version of node.

```
$> nvm install v0.10.0
```

#### Foreman

Next, you'll want to install [Foreman](https://github.com/ddollar/foreman), which is a process runner (for lack of a better definition). Foreman makes running a series of processes easier by allowing you to define process requirements in one file, known as a `Profile`. 

You can install Foreman either as a [Ruby Gem](http://rubygems.org/) (so you'll need to [install Ruby](http://www.ruby-lang.org/en/)) or if you are on OSX, there is a .dmg installer. 

Finally, you'll need to install [CoffeeScript](http://coffeescript.org/).  Type:

```
$> sudo npm install -g coffee-script
```

The above command installs CoffeeScript as a global library -- this is needed for Foreman to run the `coffee` command.

#### Run the Brew Tally app

Now that you've installed all the infrastructure to run the sample DynamoDB app, you're ready to run it. This is simple (now). Make sure you open up a terminal and change directories into the `lab_3` root directory. Type:

```
$> npm install
```

which will install a few dependencies (namely dynode, which is that nifty Dynamo library I mentioned earlier) for the app. 

##### AWS credentials! 

Access to DynamoDB (and all other AWS products) is secured via an access key and secret. These are generated for an account, you can have more than one pair, and you can reset/re-generate them anytime (and in fact, you should reset them from time to time).

I've created a temporary access key and secret for this lab that grants permission to a DynamoDB instance. You will need to set these values in your environment for the simple app to work (as, naturally, the app communicates with a live instance of DynamoDB). 

To set these environment variables, type the following in a bash shell:

```
$> export ACCESSKEY=the_key_i_provide
$> export SECRETKEY=the_secret_i_provide
``` 

Note, I'll provide the two required keys at the time of the workshop (so don't literally put `the_key_i_provide`!).

##### Fire it up 

When that's complete, type:

```
$> foreman start
```

And then open up a browser and go to [localhost:5000](http://localhost:5000/). 

![brew-tally](/docs/imgs/brew-tally.png)

You should see a simple tally of votes for 3 beers. I've purposely kept this app simple as I want you to see the basics of working with Dynamo. Accordingly, using your favorite text editor, please open the file `App.coffee` in the `src` directory.


#### A DynamoDB API



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
  * [DynamoDB](http://aws.amazon.com/dynamodb/)
    * [Let's Build Something Using Amazon's DynamoDB](http://openmymind.net/2012/2/6/Lets-Build-Something-Using-Amazons-DynamoDB/)
    * [Building Node.js app w/DynamoDB](http://public.dhe.ibm.com/software/dw/demos/jnodeamazon/index.html)


## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US">Creative Commons Attribution-ShareAlike 3.0 Unported License</a>.   
