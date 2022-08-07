

Insert & update records into the database.

db.pets.insert({"pet":"wolf","domestic?":false,"diet":"carnivorous","climate":["polar","equatotrial","continental","mountain"]})

db.pets.insert({"pet":"cat","domestic?":false,"diet":"carnivorous","climate":["polar","equatotrial","continental","mountain"]})

Update REcords:

Updating records sould be done in two way' 
1. $push : Add's an additonal fields to the collection.

2. $set :Overrides the collection.
Identidying a recrd thta matches pet name as wolf and updating the climate array.
db.pets.update({"pet":"wolf"},{$push:{"climate":"desert"}})


Matches and delete the record.
db.pets.deleteOne({"pet":"wolf"})

Deletes all the records that are matching pet name as wolf.
db.pets.deleteMany({"pet":"wolf"})

Drops the collection.
db.pets.drop()


Mongo DB Query Operators.

To Upate the Data across the Monog collection's, Following are the Query operator's in Mongodb.
$set
$inc
$unset

Comparison Query Operator:
$eq:Equals
$ne: Not equals
$lt :Lesser than
$gt:Greater than
$lte: Less than equal's.
$gte: Greater than equals.

Logical Operators
$and: returns data matching all the clauses.
$or : returns data matching either one of the clause.
{<operator>:[{statement1}:{statement},...{statementn}]}
$nor: returns data that match both the clauses. 
$not : negates the query requirement
{$not:{statement}}

Examples:
Find all the trips whose duration is less than 70 seconds and type is not equal to Subscriber.
db.trips.find({"tripduration":{$lte:70},"usertype":{$ne:"Subscriber"}})

find the population in zip's collection whose ppopulation count is less than 1000.
db.zips.find({"pop":{$lt:1000}}).count()

Nor Example:  
{$nor:[{"result":"No Violation Issued"},{"result":"Violation Issued"},{"result":"Fail"},{"result":"Warning"}]}
  
Find all the student id's from grades collection whosr student id's are greater than 25 and less than 100
db.grades.find({$and:[{"student_id":{$gt:25}},{"student_id":{$lt:100}}]})
Since we are performing and query on same field we can simplify it as a follows.
db.grades.find({"student_id":{$gt:25,$lt:100}})
 





