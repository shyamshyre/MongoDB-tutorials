

# Insert & update records into the database.

db.pets.insert({"pet":"wolf","domestic?":false,"diet":"carnivorous","climate":["polar","equatotrial","continental","mountain"]})

db.pets.insert({"pet":"cat","domestic?":false,"diet":"carnivorous","climate":["polar","equatotrial","continental","mountain"]})

# How to Update Records inside the collections:

#### Updating records sould be done in two way' 
1. $push : Add's an additonal fields to the collection.

2. $set :Overrides the collection.
#### Identidying a recrd thta matches pet name as wolf and updating the climate array.
db.pets.update({"pet":"wolf"},{$push:{"climate":"desert"}})


#### Matches and delete the record.
db.pets.deleteOne({"pet":"wolf"})

#### Deletes all the records that are matching pet name as wolf.
db.pets.deleteMany({"pet":"wolf"})

#### Drops the collection.
db.pets.drop()


# Mongo DB Query Operators.

## To Upate the Data across the Monog collection's, Following are the Query operator's in Mongodb.
###  $set
###  $inc
###  $unset

# Comparison Query Operator:
####  $eq:Equals
####  $ne: Not equals
#### $lt :Lesser than
#### $gt:Greater than
#### $lte: Less than equal's.
#### $gte: Greater than equals.

# Logical Operators
####  $and: returns data matching all the clauses.
####  $or : returns data matching either one of the clause.
#### {<operator>:[{statement1}:{statement},...{statementn}]}
#### $nor: returns data that match both the clauses. 
#### $not : negates the query requirement
{$not:{statement}}

# Examples:
#### Find all the trips whose duration is less than 70 seconds and type is not equal to Subscriber.
#### db.trips.find({"tripduration":{$lte:70},"usertype":{$ne:"Subscriber"}})

#### find the population in zip's collection whose ppopulation count is less than 1000.
db.zips.find({"pop":{$lt:1000}}).count()

#### Nor Example:  
{$nor:[{"result":"No Violation Issued"},{"result":"Violation Issued"},{"result":"Fail"},{"result":"Warning"}]}
  
###### Find all the student id's from grades collection whosr student id's are greater than 25 and less than 100
db.grades.find({$and:[{"student_id":{$gt:25}},{"student_id":{$lt:100}}]})
#### Since we are performing and query on same field we can simplify it as a follows.
db.grades.find({"student_id":{$gt:25,$lt:100}})
 
#### Dislplay airplanes CR2,A81 fly against src or desitination airports of KZN 
{$and:[{$or:[{"src_airport":"KZN"},{"dst_airport":"KZN"}]},{$or:[{"airplane":"CR2"},{"airplane":"A81"}]}]}
  
{"result":"Out of Business","sector":"Home Improvement Contractor - 100 sector"}
  
## Insteresting use case of 'and' and 'or'.
How many companies in the sample_training.companies dataset were either founded in 2004
[and] either have the social category_code [or] web category_code,
[or] were founded in the month of October
[and] also either have the social category_code [or] web category_code?
 
db.companies.find({$or:[{$and:[{"founded_year":2004},{$or:[{"category_code":"web"},{"category_code":"social"}]}]},{$and:[{"founded_month":10},{$or:[{"category_code":"web"},{"category_code":"social"}]}]}]})
db.companies.find({$or:[{"$and":[{"founded_year":2004},{"$or":[{"category_code":"social"},{"category_code":"web"}]}]},{"$and":[{"founded_year":2004},{"$or":[{"category_code":"social"},{"category_code":"web"}]}]}]}).count() 
  
$expr:- Expression : allows the aggregation expressions within the query language.
{$expr:{<expression>}}
  
Find the totalnumber of trips where the source and destination points are same and who have reneted bike less than 100 seconds.
Approach 1 

db.trips.find({$and:[{tripduration:{$lt:100}},{"$expr":{"$eq":["$start station id","$end station id"]}}]}).count()
  
Approach 2: 

db.trips.find({$expr:{$and:[{"$gt":["$tripduration:1200]},{$eq:{$$start station id, $end station id}}]})

# MQL syntax vs Aggregation Sysntax: 
####  {field:{$operator:value}}  
####   Expression:
####  {$operator:{field:value}}

# Searching an element inside the Array.

#### db.listingsAndReviews.find({"ameneties":["TV","Internet","Wifi"]})
#### Above String will return search results exactly matching criteria and it's case sensitive.

####  $all : Default search criteria only mathches the search string following the same order, where as when we declare the order:all it matches across all the elements irrespective of the order.
  ## db.listingsAndReviews.find({"amenities":{$all:["TV","Internet","Wifi"]}})
#### This will search the elements exact element's matching the above order provided in the array.
#### This will search all the elments without coniderig the order.
## $size: this is used to restrict or limit the size of the array.

$size:20 This will limit the fields to only 20 fields.
  
 #### The below query maps the search results to 40 + document's with amenities more than 15+ fields Searh string is specified in the array.
 ####  This is matched to the search criteria specified in the array and results multiple values.
 ####  Size maps to the length of the fields and display's
  db.listingsAndReviews.find({amenities:{"$size":10,"$all": ['TV', 'Cable TV', 'Wifi']}}).count()
  
#### Find the amenities whose value is wifi and find out the only fields roomtype and address columns. 
 db.listingsAndReviews.find({"amenities":"Wifi"},{"room_type":1,adderss:1})
 field:1 -> this will act as a projection to diplay the seleced columns.

# Projection's helps you to find the data fields exclusively inside the collections.
#### Note: While using projections ensure that you are not 
## 0- this will exclude the field appearing in the result.
## 1- this will inlcude the field as apart of the result.
## 0,1-> We cannout use both 0 and 1 at the same time, We can only use this if we are using id column.

Examples:
1. db.collection.find({amenities:{''},{field1:1,field2:1,field3:1}})
2. db.collection.find({amenities:{''},{field1:0,field2:0,field3:0}})
Exception only in case of id, else we cannot have both 0's and 1 in the same collection.
3. db.collection.find({amenities:{''},{field1:1,_id:0)

## $elemMatch:- This is used to result or match the sub document's inside the array.
Example we have a class, and we have students inside them and they have scores of various exam's and if we want to filter out the scores of students who marks are greater than 80.

### The below is the usage of the elemMatch as projection.
db.grades.find({"class_id":431},{"scores":{"$elemMatch":{"score":{"$gt":80}}}})
 
### The below is the usage of the elemMatch as projection.
db.grades.find({"class_id":431},{"scores":{"$elemMatch":{"score":{"$gt":80}}}})
  
## The below query will return the data of matching fiels and their respective values.(Column 1 =Column2)
### How many companies in the sample_training.companies collection have the same permalink as their twitter_username?
db.companies.find({"$expr":{"$eq":["$permalink","$twitter_username"]}}).count()
  
## Array Operators:
### {arrayfield:{$size:{10}} : returns value's with all the fields matching exactly length
  
### {arrayfield:{$size:{10}} : returns value's with all the fields matching exactly length
  
### Example: Using the sample_airbnb.listingsAndReviews collection find out how many documents have the "property_type" "House", and include "Changing table" as one of the "amenities"?
#### db.listingsAndReviews.find({$and:[{"property_type":"House"},{"amenities":{"$all":["Changing table"]}}]}).count() ->11
  

 ## Arrays & Projections
 ### How many companies in the sample_training.companies collection have offices in the city of Seattle?
 #### db.companies.find({ "offices": { "$elemMatch": { "city": "Seattle" } } }).count() ->117 
  
 

  
  
  
  



