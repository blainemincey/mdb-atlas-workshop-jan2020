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

#### Query Data with MongoDB Compass (CRUD Operations) 
Simply copy the code block for each section below and paste within the filter dialog in MongoDB Compass
as indicated below.  Please pay close attention to the database/collection you have selected.  The various options
can be expanded by clicking the **Options** button.

![](img/compass_start.jpg)

##### Find Operations
* Simple filter (specific name in sample_mflix.comments collection) 

```
{ name : "Ned Stark" }
```

* Simple filter with query operators (email that ends in fakegmail.com and comment made since 2017 in sample_flix.comments) 
```
{ email: /fakegmail.com$/ , date : {$gte : ISODate('2017-01-01')}}
``` 

* Query sub-document/array (movies with more than 10 wins in sample_mflix.movies) 
```
{"awards.wins" : {$gt : 10}} 
``` 

* Query using the $in operator (Movies in Arabic or Welsh in sample_mflix.movies) 
```
{languages : {$in : ["Arabic", "Welsh"]}} 
``` 

* Only movies in Arabic 
```
{languages : ["Arabic"] } 
``` 

* Movies in BOTH Arabic and Spanish 
```
{languages : { $all : ["Arabic", "Spanish"]}}
``` 

* Movies with IMDB Rating > 8.0 and PG Rating 
```
{"imdb.rating": {$gt : 8.0 }, rated : "PG"} 
``` 
* Movies with Title starting with "Dr. Strangelove" 
```
{ title : /^Dr. Strangelove/ } 
``` 

##### Update, Delete, Clone Operations 
Find a document in any collection and choose to update a field or fields. In fact, you can select to add a field that
does not exist. Next, clone a document. Then, delete the cloned document. MongoDB Compass provides all the CRUD 
controls you will need. Below is a sample image displaying where this capability is:

![](img/compasscrud.jpg) 


### Lab 2 - Query Performance/Indexes
#### Create Indexes to Improve Efficiency of Queries

Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a 
**collection scan**, i.e., scan every document in a collection to select those documents that match the query 
statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it 
must inspect.

In this lab, we will perform a search on a field, use the explain plan to determine if it could be improved 
with an index, and create the index...all from within MongoDB Compass.

For this lab, we will execute a query to find all movies with at least Drama as a genre.  Again, remain
in the **sample_mflix** database within the **movies** collection.

In the query box, enter:
```
{genres : "Drama" } 
```
Then, click the *Explain Plan** tab as indicated below: 
![](img/compass-explain.jpg) 

Click the **Execute Explain** button in the middle of the GUI and review the output.

![](img/compass-noindex.jpg) 

Considering this is a relatively small data size, an index may not immediately improve performance.  In our results, it 
indicates a **collection scan** took place with our filter.  It also indicates the Actual Query Execution Time was 17ms.

Click on the **Indexes** tab.  We will create an index on **genres**.  Find the field in the dropdown and select 'asc'
as the index type.  Then, click the create button as indicated below.

![](img/createidx.jpg)  

After creating the index, go back to your **Explain Plan** tab and **Execute Explain** once more.  This time, you should 
see that your index was used and instead of a collection scan, your filter performed an **index scan**.  Your results 
should look similar to that below. 

![](img/idx_used.jpg) 

#### Performance Advisor Demonstration
The [Performance Advisor](https://docs.atlas.mongodb.com/performance-advisor/) monitors queries that MongoDB considers 
slow and suggests new indexes to improve query performance.  The Performance Advisor recognizes a query as slow if it 
takes longer than **100 milliseconds** to execute.  The Performance Advisor does NOT negatively affect the 
performance of your Atlas clusters.

In order to demonstrate the Performance Advisor, we need to generate some poor performing queries against our 
sample dataset within MongoDB Atlas.  We will take advantage of a project built specifically for generating slow running
queries on our sample dataset, [https://github.com/robbertkauffman/mdb-slow-running-queries](https://github.com/robbertkauffman/mdb-slow-running-queries).  

For purposes of the demonstration, a MongoDB Stitch application was created and is utilizing a Scheduled Trigger to generate
a random poor performing query against our sample dataset.  While the Performance Advisor is scanning the logs, 
we can take a look at the [MongoDB Atlas Query Profiler](https://docs.atlas.mongodb.com/tutorial/profile-database/).  This
tool provides the ability to diagnose and monitor performance issues.  For example, the image below
was taken while the slow running queries were executed to illustrate the metrics that can be reported.

![](img/queryprofiler.jpg)  

TODO - Perf Advisor screen shot 


### Lab 3 - Aggregation Framework Example 
MongoDB's [Aggregation Framework](https://docs.mongodb.com/manual/core/aggregation-pipeline/) is modeled on the concept
of data processing pipelines.  Documents enter a multi-state pipeline that transforms the documents into an aggregated
result.  For our next exercise, we will use the aggregation pipeline builder in MongoDB Compass to create an
aggregation pipeline.

We have been tasked with determining the most popular genres of movies that have been produced in the USA, Germany, and
France since the year 2000.  Click on the **Aggregations** tab.  It should look similar to that below.

![](img/agg1.jpg)  

First, our pipeline will need to filter for movies with a country of USA, France, and Germany and have been produced since
the year 2000.  This will be done with a **$match** operation.  In the first stage of the pipeline
builder, select **$match** from the dropdown and paste the following:
```
{
  countries : ["USA", "Germany", "France"],
  year : {$gt : 2000 }
}
``` 

MongoDB Compass should look similar to that below. 

![](img/match.jpg)  

You should observe that a preview of sample documents is on the right-hand side of the builder.  Now, we want to unwind
the genres array as we want to count the most popular genres.  The next stage will be an **$unwind** operation on genres.
Paste the following in the $unwind stage.
```
{
    path: "$genres"
}
``` 

Now, we want to group by the genres and count them.  This is done with the **$group** operator and the **$sum** operator.
Copy/Paste the snippet below into the $group stage.
```
{
  _id: "$genres",
  genre: {
    $sum: 1
  }
}
``` 

Finally, we will want to sort the result in a descending fashion so the most popular genre is the first element of our result.
Can you figure out how to do that?  As a hint, the final overall pipeline will be the snippet that follows.
```
[
    {
        $match: {
            countries : ["USA", "Germany", "France"],
            year : {$gt : 2000 } }}, 
        { $unwind: { path: "$genres" }}, 
        { $group: { _id: "$genres",
                   genre: { $sum: 1 } }}, 
        { $sort: { "genre": -1 }
    }
]
```
This pipeline can then be saved if you would like to use it again at a later time.


---

## 3.  MongoDB Atlas Operations 
### Lab 1 - Continuous Backup

### Lab 2 - Cluster Snapshots 

### Lab 3 - Point-in-Time Restore

---

## 4.  MongoDB Stitch
[MongoDB Stitch](https://docs.mongodb.com/stitch/) is a serverless platform that enables developers to quickly build applications
without having to set up server infrastructure.  Stitch is built on top of MongoDB Atlas enabling it to
automatically integrate the connection to your database.

In this section of the Atlas Workshop, we will create our first MongoDB Stitch application in order to expose our
sample data through a RESTful endpoint. 

### Lab 1 - Webhooks
Our first step in exposing our sample data via a REST endpoint or rather a webhook is to create a MongoDB
Stitch application.  First, click the **Stitch** menu option within our MongoDB Atlas UI in the lower left-hand
section of the navigation.  Once this is clicked, then you will need to click the button **Create New Application**.
We will name our application MyStitchApplication and we will keep the existing default selections.  Your UI should
look like that below.  If it does, continue by clicking the **Create** button.

![](img/newstitch.jpg) 

#### Create Stitch Function 
We have performed a number of exercises with the **sample_mflix** database.  Within this database, there is a **users**
collection.  If you have inspected this collection, you may have noticed that there is a unique index on the email field.  
If we wanted to query for unique document, it would make sense that we will need to be able to retrieve a document based
on the email field.  Let's create a function to find a user by their email.

We will need to select the **Functions** option from the left-hand navigation and then click the **Create New Function**
button as indicated below.

![](img/stitchfunction.jpg) 

When the function editor opens, you should name your function **getUserByEmailFunction**.  Keep the remaining defaults.
Your function settings should look like that below.  When it does, click the **Save** button in the bottom-right corner.

![](img/getuserfunction.jpg)  

After saving, the Function Editor will open.  Copy and paste the code snippet below and click the **Save** button.
```
//
// To test in console: exports('aidan_gillen@gameofthron.es')
//
exports = async function(arg) {
  
  console.log("Calling getUserByEmail Function.");
  console.log("Find by email: " + arg);

  var usersCollection = context.services.get("mongodb-atlas").db("sample_mflix").collection("users");
  
  const query = { "email": arg };

  return usersCollection.findOne(query)
    .then(result => {
      if(result) {
        console.log(`Successfully found document: ${result}.`)
        return result;
      } 
      else {
        console.log("No document matches the provided query.")
        return {"Result" : "No document matches the provided id: " + arg}
      }
    })
  .catch(err => console.error(`Failed to find document: ${err}`))
};
``` 
Once the code has been pasted and saved, we can run a quick smoke test of our function.  In the comment section at the
top of the code snippet, you can copy and paste in the Console tab at the bottom of the screen.  Remove the 'Hello World' 
snippet in the console that is placed there by default.  It should look similar to this below after your edit.

![](img/emailtest.jpg) 

Click the **Run** button as indicated in the image above and the **Result** tab should become enabled with a result.
If your test is successful, be sure to click the **Review & Deploy Changes** button at the top of the page.  It should
look like the image below.

![](img/reviewchanges.jpg) 

The **Review & Deploy Changes** dialog will appear.  Simply click the **Deploy** button in the lower-right corner.


#### Create HTTP Service
Now that we have successfully tested our function, let's create an HTTP Service so we can interface with it via REST.
First, click the **3rd Party Services** option in the left-hand navigation then the **Add a Service** button in the
center of the page as indicated below.

![](img/createservice.jpg) 

Select **HTTP** from the Service options.  Name the service **myHttpService**.  
![](img/httpservice.jpg) 

Now, we will want to **Add Incoming Webhook**.  
![](img/addwebhook.jpg) 

Name the webhook **getUserByEmailWebhook**. Select **GET** as the HTTP Method.  Keep the remaining defaults similar
to that below.

![](img/getwebhook.jpg) 
![](img/getwebhook2.jpg) 

The Function Editor for our webhook will open.  Recall that earlier we create a function to find a user by their email.
We will now interface with that function from our webhook.  Copy the code snippet below and paste into the function
editor over the default code that is placed there.
```
// This function is the webhook's request handler.
//
// To test: exports({query: {email: 'sean_bean@gameofthron.es'}})
exports = async function(payload, response) {
  
  console.log("Executing Webhook getUserByEmail.");
  
  // Query params, e.g. '?arg1=hello&arg2=world' => {arg1: "hello", arg2: "world"}
  const {email} = payload.query;
  
  console.log("Query arg: " + email);
  
  // Calling a function:
  return await context.functions.execute("getUserByEmailFunction", email);
};
```

If you would like to quickly test this function, you can use the snippet that is in the comment section of the pasted
code.  You will test it in exactly the same manner as we tested the earlier function that was created.  If your test
is successful, let's interface with our REST API from a 3rd party tool!  Be sure to **Review & Deploy Changes** prior
to testing externally!!!

#### Test the GET Webhook
Since this is a REST-based API, we can basically use just about any tool that we are comfortable with.  For example, one
could test with a Python script, the command-line utility curl, or [Postman](https://www.getpostman.com/).

MongoDB Stitch makes this incredibly easy.  If you click on the **Settings** tab for our webhook, it will provide the
Webhook URL.
![](img/webhookurl.jpg) 

After you copy the Webhook URL, you can use your tool of choice.  Be sure to include the proper query parameter to the
end of the request.  Our parameter is named **email** as defined in our Webhook Function.  An example would be:
```
email=sean_bean@gameofthron.es
```
Once you have the parameter defined, you can now test the GET service of your webhook.  An example curl command and
result are provided below.  This exact command will NOT work for you as your Webhook URL will be entirely different!
```
curl https://webhooks.mongodb-stitch.com/api/client/v2.0/app/mystitchapplication-tlpid/service/myHttpService/incoming_webhook/getUserByEmailWebhook\?email=sean_bean@gameofthron.es
{"_id":{"$oid":"59b99db4cfa9a34dcd7885b6"},"name":"Ned Stark","email":"sean_bean@gameofthron.es","password":"$2b$12$UREFwsRUoyF0CRqGNK0LzO0HM/jLhgUCNNIJ9RJAqMUQ74crlJ1Vu"}
```
#### Create POST Webhook







### Lab 2 - Query Anywhere 

### Lab 3 - Triggers

---

## 5.  MongoDB Charts
### Lab 1 - Visualize Your Data
MongoDB Charts allows you to quickly and easily unlock the value in your data. Let's do some analysis on our NYC restaurant data:

#### Activate Charts

Click the Charts menu under SERVICES on the left on the left:

![](img/charts-001.jpg) 

Click on the "Activate MongoDB Charts" button as indicated below:

![](img/charts-002.jpg)

#### Define a Data Source for the Charts

Select **Data Sources** from the menu at the top of the page.

![](img/charts-003.jpg)

Click add a **New Data Source**. 

![](img/charts-004.jpg)

As we only have one cluster in our project, there’s only one data source to select. Select the cluster and click connect as indicated by arrows 1 (one) and 2 (two):

![](img/charts-005.jpg)

Select the database **sample_mflix** by clicking in the arrorw next to it, then select the collections "comments" and "movies". Last, click on **Set Permissions**

![](img/charts-006.jpg)

Click the **Publish Data Source** button:

![](img/charts-007.jpg)

#### Create Dashboard

![](img/charts-008.jpg)

Click on **Dashboards**:

![](img/charts-008.jpg)

Enter **Title**, **Description** and click on **Create**:

![](img/charts-010.jpg)

Click on **Add Chart**:

![](img/charts-011.jpg)

Click on **Choose a Data Source**:

![](img/charts-012.jpg)

Click on **sample_mflix.movies** source to create our first chart:

![](img/charts-013.jpg)

Click on **Chart Type**,select the **Area** chart 1 (onwe) and click on **Discrete** 2 (two):

![](img/charts-014.jpg)

Drag **runtime** field (1) to the **Y axis**, click on **Aggregate** and select **MEAN** (2) two. Drag the field **Year** 3 (three), click **Binning on** and type 10 4(four)

Filter the occurrences of year that are numeric integer fields, copy the following string into the filter section 5 (five) 

 {$or :[{"year" : { $type : 16 }},{"year" : { $type : 18 }},{"year" : { $type : 1 }}]} 

Change the title to "Average Movie Length by Decade"  6 (six) :

![](img/charts-015.jpg)

Click on customize to select a new color, if desired.

Click **Save and Close**:

XXXXXX

### Lab 2 - Embed a chart

---

## 6.  Code Samples