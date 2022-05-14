# MongoDB Notes
after installating mongoDB in the pc Write following line in terminal. This will start the shell

    C://.................................................../mongo
after that.

> command to show all databases

   

    > show dbs
    admin      0.000GB
    config     0.000GB
    local      0.000GB
    myFirstDB  0.000GB

NOTE: databases which do not contain any data are cant be seen with this command.

> creating a DB or using the existing one

    use myFirstDB

> adding data to myFirstDB

    db.data1.insertOne({fname:'Vikas',lname:'Anand',age:25,married:false})
or

    db.data1.insertMany([{fname:'Vishal',lname:'Anand',age:19,married:false},{fname:'Vivek',lname:'Anand',age:14,married:false}])

> to know the active database

    db

> to show collections of active database

    show collections

> to view the collection in detail
 

   

     > db.data1.find()
    { "_id" : ObjectId("627f744e2872c88092ca2613"), "fname" : "Vikas", "lname" : "Anand", "age" : 25, "married" : false }
    { "_id" : ObjectId("627f7a0a2872c88092ca2614"), "fname" : "Vishal", "lname" : "Anand", "age" : 19, "married" : false }
    { "_id" : ObjectId("627f7a0a2872c88092ca2615"), "fname" : "Vivek", "lname" : "Anand", "age" : 14, "married" : false }

or

    > db.data1.find().pretty()
    {
            "_id" : ObjectId("627f744e2872c88092ca2613"),
            "fname" : "Vikas",
            "lname" : "Anand",
            "age" : 25,
            "married" : false
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2614"),
            "fname" : "Vishal",
            "lname" : "Anand",
            "age" : 19,
            "married" : false
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2615"),
            "fname" : "Vivek",
            "lname" : "Anand",
            "age" : 14,
            "married" : false
    }

> to view field with the matching key

    > db.data1.find({fname:"Vikas"}).pretty()
    {
            "_id" : ObjectId("627f744e2872c88092ca2613"),
            "fname" : "Vikas",
            "lname" : "Anand",
            "age" : 25,
            "married" : false
    }

> to view the particular key of the particular field.

    > db.data1.find({age:19},{fname:1}).pretty()
    { "_id" : ObjectId("627f7a0a2872c88092ca2614"), "fname" : "Vishal" }
or

    > db.data1.find({age:19},{fname:0}).pretty()
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2614"),
            "lname" : "Anand",
            "age" : 19,
            "married" : false
    }
or

    > db.data1.find({age:19},{married:0,_id:0}).pretty()
    { "fname" : "Vishal", "lname" : "Anand", "age" : 19 }

> Limit the number of data to be shown

    > db.data1.find().pretty().limit(2)
    {
            "_id" : ObjectId("627f744e2872c88092ca2613"),
            "fname" : "Vikas",
            "lname" : "Anand",
            "age" : 25,
            "married" : false
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2614"),
            "fname" : "Vishal",
            "lname" : "Anand",
            "age" : 19,
            "married" : false
    }
or

    > db.data1.find({lname:"Anand"}).pretty().limit(2)
    {
            "_id" : ObjectId("627f744e2872c88092ca2613"),
            "fname" : "Vikas",
            "lname" : "Anand",
            "age" : 25,
            "married" : false
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2614"),
            "fname" : "Vishal",
            "lname" : "Anand",
            "age" : 19,
            "married" : false
    }

> Limit the number od data to be shown but with another method

    > db.data1.findOne({lname:"Anand"})
    {
            "_id" : ObjectId("627f744e2872c88092ca2613"),
            "fname" : "Vikas",
            "lname" : "Anand",
            "age" : 25,
            "married" : false
    }

> To skip the data to be shown

    > db.data1.find().pretty().limit(2).skip(1)
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2614"),
            "fname" : "Vishal",
            "lname" : "Anand",
            "age" : 19,
            "married" : false
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2615"),
            "fname" : "Vivek",
            "lname" : "Anand",
            "age" : 14,
            "married" : false
    }

> Updating the data (only one)
> 
this will update the first field which get matched to the query.

    > db.data1.updateOne({lname:"Anand"}, {$set:{lname:"Prajapati", age:26}})
    { "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
    > db.data1.find().pretty().limit(1)
    {
            "_id" : ObjectId("627f744e2872c88092ca2613"),
            "fname" : "Vikas",
            "lname" : "Prajapati",
            "age" : 26,
            "married" : false
    }

> Updating the data (many)

this will update the multiple fields matching to the query.

    > db.data1.updateMany({lname:"Anand"},{$set:{married:true}})
    { "acknowledged" : true, "matchedCount" : 2, "modifiedCount" : 2 }
    > db.data1.find().pretty()
    {
            "_id" : ObjectId("627f744e2872c88092ca2613"),
            "fname" : "Vikas",
            "lname" : "Prajapati",
            "age" : 26,
            "married" : false
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2614"),
            "fname" : "Vishal",
            "lname" : "Anand",
            "age" : 19,
            "married" : true
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2615"),
            "fname" : "Vivek",
            "lname" : "Anand",
            "age" : 14,
            "married" : true
    }

> Deleting the field matching to the query

    > db.data1.deleteMany({lname:"Prajapati"})
    { "acknowledged" : true, "deletedCount" : 1 }
    > db.data1.find().pretty()
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2614"),
            "fname" : "Vishal",
            "lname" : "Anand",
            "age" : 19,
            "married" : true
    }
    {
            "_id" : ObjectId("627f7a0a2872c88092ca2615"),
            "fname" : "Vivek",
            "lname" : "Anand",
            "age" : 14,
            "married" : true
    }

> Deleting the all fields

    > db.data1.deleteMany({})
    { "acknowledged" : true, "deletedCount" : 2 }
    > db.data1.find().pretty()
    >    
                                                                                                     

    
   
