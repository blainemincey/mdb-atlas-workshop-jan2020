# Atlanta MongoDB Atlas Workshop at TechRise
The purpose of this repository is to support the MongoDB Atlas Workshop in Atlanta coming January 15, 2020.  If you are interested in participating, please be sure to register
for the [Atlanta MongoDB Atlas Workshop at Techrise](https://atlmdbatlasworkshoptechrise.splashthat.com/). 

![](img/Atlanta_MongoDB_Atlas_Workshop_At_TechRise.jpg)

---

## Introduction to MongoDB and MongoDB Atlas
### Hands-on Workshop
#### Overview


#### Required Prerequisites

* To use MongoDB Atlas, you **must** be able to make outgoing requests from your computer
to MongoDB Atlas services which will be running on port 27017.  Please confirm that port
27017 is not blocked by checking with [PortQuiz](http://portquiz.net:27017/).

* Complete [Getting Started with MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/). This involves creating an Atlas account, creating your first
free tier cluster, and setting up the appropriate security credentials

* Load Sample data into your cluster by following the [Insert and View Data in Your Cluster](https://docs.atlas.mongodb.com/tutorial/insert-data-into-your-cluster/)
section of the Getting Started Guide.

* Download/Install [MongoDB Compass](https://www.mongodb.com/products/compass).  At the moment,
the most recent version of MongoDB Compass is 1.20.4 (Stable).

* Verify the sample databases/collections have been successfully loaded into your
MongoDB Atlas cluster by using either MongoDB Compass or the [Data Explorer](https://docs.atlas.mongodb.com/data-explorer/index.html)
tool that is part of the MongoDB Atlas User Interface.

---

### Outline of Workshop:

1.  Deploy a MongoDB Atlas Cluster
    * Create MongoDB Atlas Account
    * Create cluster
    * Load/View data in MongoDB Sandbox Cluster
    
2.  Query and Manage Data
    * MongoDB Compass
    * Query Performance/Indexes (includes Performance Advisor Demo)
    * Aggregation Framework Example
    
3.  MongoDB Atlas Operations
    * Continuous Backup
    * Cluster Snapshots
    * Demo of Point-in-Time restore
    
4.  MongoDB Stitch
    * Webhooks
    * Query Anywhere
    * Triggers
    
5.  MongoDB Charts

6.  Code Samples

---

# MongoDB Atlas Workshop 

## 1.  Deploy a MongoDB Atlas Cluster 
### Lab 1
Complete the required prerequisites at the top of this page.

---

## 2.  Query and Manage Data 
### Lab 1 - MongoDB Compass 
#### Connect to your cluster in MongoDB Atlas
If you have completed the steps above, we can now copy the connection string and connect MongoDB Compass
to our cluster.  Within the Atlas UI, in the Clusters view, click the **Connect** button.  This will display
a pop-up window where you can select how to connect to your Atlas Cluster.  Select the option
**Connect with MongoDB Compass**.  The image below should provide an idea on what you should see:

![](img/Clusters___Atlas__MongoDB_Atlas.jpg)  

Be sure to select that you already have MongoDB Compass and select the latest version which is indicated by
**1.12 or later**.  Then, copy your connection string to paste into MongoDB Compass.  All you should have to do is enter the
password you used when you first created a MongoDB Atlas database user or Atlas Admin user.  Be sure to select this
as a favorite as this makes it much quicker to open the database when you re-open MongoDB Compass.  It should
look similar to that below:

![](img/MongoDB_Compass_-_Connect.jpg)  

Once you have successfully authenticated to MongoDB Atlas through MongoDB Compass, you should be presented
with a screen similar to that below:

![](img/MongoDB_Compass_-_atlanta-workshop-7tmzl_mongodb_net_27017.jpg)


#### Browse Documents
Click the **sample_mflix** database to open  up a series of collections.  Then, select the **theaters** collection.  It
should have a list of the documents similar to that below:  

![](img/MongoDB_Compass_-_browse.jpg)  

There is simply an **_id** field, a **theaterId** field, and a **location** object.  Wait, did we just store an object in the 
database?  We can expand the location field/object and see it is storing an address and embedding yet another object for
its geolocation.  Working with data in this way, it is much easier than having to flatten out multiple tables from a 
relational/tabular model in a single object.  

#### Analyze the Schema 
Analyze the schema??? Wait, I thought that MongoDB was a NoSQL database and was considered to be schema-less? While that
is technically true, no dataset would be of any use without the notion or concept of a schema. Although MongoDB does not
enforce a schema, your collections of documents will still always have one. The key difference with MongoDB is that the
schema can be flexible/polymorphic.  

Within MongoDB Compass, select the **Schema** tab and click **Analyze Schema**.  MongoDB Compass will sample the
documents in the collection to derive a schema.  In addition to providing field names and types, MongoDB Compass
will also provide a summary of the data values and their distribution.

After you click **Analyze Schema**, if you inspect the **location** field, you will notice that it contains a 
**geo** field with GeoJSON coordinates, i.e. a longitude and latitude coordinate.  You can drill down on the
map as it builds out a query for you.  It should resemble the image below:  

![](img/MongoDB_Compass_-_analyze.jpg)  


### Lab 2 - Query Performance/Indexes
#### Query Data with MongoDB Compass 


### Lab 3 - Aggregation Framework Example 

---

## 3.  MongoDB Atlas Operations 
### Lab 1 - Continuous Backup

### Lab 2 - Cluster Snapshots 

### Lab 3 - Point-in-Time Restore

---

## 4.  MongoDB Stitch
### Lab 1 - Webhooks

### Lab 2 - Query Anywhere 

### Lab 3 - Triggers

---

## 5.  MongoDB Charts
### Lab 1 - 

### Lab 2 - 

---

## 6.  Code Samples