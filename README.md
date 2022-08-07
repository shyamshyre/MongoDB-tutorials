

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

Comparison Operator:
$eq:
$neq;
$lt
$gt



