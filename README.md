

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
  
  
$expr:- Expression : allows the aggregation expressions within the query language.
{$expr:{<expression>}}
  
Find the totalnumber of trips where the source and destination points are same and who have reneted bike less than 100 seconds.
Approach 1 

db.trips.find({$and:[{tripduration:{$lt:100}},{"$expr":{"$eq":["$start station id","$end station id"]}}]}).count()
  
Approach 2: 

db.trips.find({$expr:{$and:[{"$gt":["$tripduration:1200]},{$eq:{$$start station id, $end station id}}]})

MQL syntax vs Aggregation Sysntax: 
  {field:{$operator:value}}  
Expression:
  {$operator:{field:value}}

## Searching an element inside the Array.
 db.companies.find({"ameneties":["Tv","Internet","Wifi"]})
 This will search the elements exact element's matching the above order.
  
 db.companies.find({"ameneties":{$all:["Tv","Internet","Wifi"]}})
 This will search all the elments without coniderig the order.
  
 ## To limit the array length
  $size:20
  
 #### The below query maps the search results to 40 + document's with amenities more than 15+ fields Searh string is specified in the array.
 ####  This is matched to the search criteria specified in the array and results multiple values.
 ####  Size maps to the length of the fields and display's
**  db.listingsAndReviews.find({amenities:{"$size":10,"$all": ['TV', 'Cable TV', 'Wifi']}}).count()
  
 Find the amenities who value is wifi and find out the only fields roomtype and address columns. 
 db.listingsAndReviews.find({"amenities":"Wifi"},{"room_type":1,adderss:1})
 field:1 -> this will act as a projection to diplay the seleced columns.

# Projection's helps you to find the data fields exclusively inside the collections.
#### Note: While using projections ensure that you are not 





