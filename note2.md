# Mongoose
***It helps us to establish connection between mongodb and expressJS***

> install mongoose

    npm install mongoose

> importing and connecting mongoose to database

    // importing mongoose    
    const  mongoose = require("mongoose");      
    
    // this will make connection to 'firstDB' (if it exists) OR if it doesn't exists then it will be automatically created and then connection will be established    
    
    mongoose.connect("mongodb://localhost:27017/firstDB")    
    .then(() => { console.log("connection successful") })    
    .catch((err) => { console.log(err) })
## Schema and model

 - a mongoose schema defines the structure of the document, default
   values , validators etc.
   
 - A mongoose model is a wrapper on the mongoose schema. it provides an
   interface to the database for creating, quering, updating deleting
   records etc. In simple words we create our actual collection with
   help of model

> creating schema and model

    // creating a schema (object)
    const  myFirstSchema = new  mongoose.Schema({
	    fname:{
		    type:String,
		    required:true
	    },
	    lname:{
		    type:String,
		    default:"Anand"
	    },
	    age:Number,
	    adult:Boolean,
	    lastUpdate:{
		    type:Date,
		    default:Date.now
	    }
    }) 
    
    // creating a model (class)
    const  UserData = new  mongoose.model("UserData", myFirstSchema);

### Creating document

    // adding document
    const  insertDocument = () => {
	    const  person = new  UserData({
		    fname: "Vikas",
		    lname: "Anand",
		    age: 25,
		    adult: true
	    })
	    person.save().then((res)=>{console.log(res)}).catch((err)=>{console.log(err)});
    }
    insertDocument()   
OR

    const  insertDocument = (data) => {
	    const  person = new  UserData({...data})
	    person.save().then((res) => { console.log(res) }).catch((err) => { console.log(err) });
    }
    insertDocument({ fname: "Vikas", lname: "Anand", age: 25, adult: true })
    insertDocument({ fname: "Vipul", age: 8, adult: false })

> Result in mongo compass   

    {
      fname: 'Vikas',
      lname: 'Anand',
      age: 25,
      adult: true,
      _id: new ObjectId("6281b1642fc79dcebe6981b3"),
      lastUpdate: 2022-05-16T02:05:24.288Z,
      __v: 0
    }
    {
      fname: 'Vipul',
      lname: 'Anand',
      age: 8,
      adult: false,
      _id: new ObjectId("6281b1642fc79dcebe6981b4"),
      lastUpdate: 2022-05-16T02:05:24.290Z,
      __v: 0
    }

  
  **creating multiple documents at once**

    const  person1 = new  UserData({ fname: "Vishal", lname: "Anand", gender: "Male", age: 19, adult: true })
    
    const  person2 = new  UserData({ fname: "Vivek", lname: "Anand", gender: "Male", age: 12, adult: false })
    
    const  person3 = new  UserData({ fname: "Sohani", lname: "Anand", gender: "Female", age: 5, adult: false })
    
    UserData.insertMany([person1, person2, person3]).then((res) => { console.log(res) }).catch((err) => { console.log(err) });

## Reading the document

> All documents

    const  readDocument = () => {    
	    UserData.find().then((res)=>{console.log(res)}).catch((err)=>{console.log("coudn't find the data")})    
    }    
    readDocument()
***console***


    [
      {
        _id: new ObjectId("6281dce4c9e4d350195edbf6"),
        fname: 'Vikas',
        lname: 'Anand',
        gender: 'Male',
        age: 25,
        adult: true,
        lastUpdate: 2022-05-16T05:11:00.209Z,
        __v: 0
      },
      {
        _id: new ObjectId("6281dce4c9e4d350195edbf7"),
        fname: 'Vipul',
        lname: 'Anand',
        gender: 'Male',
        age: 8,
        adult: false,
        lastUpdate: 2022-05-16T05:11:00.212Z,
        __v: 0
      },
      {
        _id: new ObjectId("6281e2089393caa5f477133e"),
        fname: 'Vishal',
        lname: 'Anand',
        gender: 'Male',
        age: 19,
        adult: true,
        lastUpdate: 2022-05-16T05:32:56.149Z,
        __v: 0
      },
      {
        _id: new ObjectId("6281e2089393caa5f477133f"),
        fname: 'Vivek',
        lname: 'Anand',
        gender: 'Male',
        age: 12,
        adult: false,
        lastUpdate: 2022-05-16T05:32:56.150Z,
        __v: 0
      },
      {
        _id: new ObjectId("6281e2089393caa5f4771340"),
        fname: 'Sohani',
        lname: 'Anand',
        gender: 'Female',
        age: 5,
        adult: false,
        lastUpdate: 2022-05-16T05:32:56.150Z,
        __v: 0
      }
    ]

> Filtered read

    const  readDocument = () => {
	    UserData.find({ gender: "Male" }).select({ fname: 1, _id: 0, age: 1 })
	    .then((res) => { console.log(res) })
	    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    [
      { fname: 'Vishal', age: 19 },
      { fname: 'Vivek', age: 12 },
      { fname: 'Vikas', age: 25 },
      { fname: 'Vipul', age: 8 }
    ]

> Limit data
> 

    const  readDocument = () => {
	    UserData.find({ gender: "Male" }).select({ fname: 1, _id: 0, age: 1 }).limit(2)
	    .then((res) => { console.log(res) })
	    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()

***console***

    [ { fname: 'Vishal', age: 19 }, { fname: 'Vivek', age: 12 } ]
    
### Comparision query operators
|Name| Description |
|--|--|
|  $eq| matches equal to |
|  $gt|  matches greater then|
|  $gte|  matches greater then equal to|
|  $in|  matches any of the value specified in an array|
|  $lt|  matches less then|
|  $lte|  matches less then equal to|
|  $ne|  matches not equal to|
|  $nin| matches none of the value specified in an array |

> $gte ; $gt ; $lte ; $lt 

    const  readDocument = () => {
	    UserData.find({ age : {$gte : 10} }).select({ fname: 1, _id: 0, age: 1 })
	    .then((res) => { console.log(res) })
	    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    [
      { fname: 'Vishal', age: 19 },
      { fname: 'Vivek', age: 12 },
      { fname: 'Vikas', age: 25 }
    ]

> $ne : $eq

    const  readDocument = () => {
	    UserData.find({ fname : {$ne : "Vikas"} }).select({ fname: 1, _id: 0, age: 1 })
	    .then((res) => { console.log(res) })
	    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    [
      { fname: 'Vishal', age: 19 },
      { fname: 'Vivek', age: 12 },
      { fname: 'Sohani', age: 5 },
      { fname: 'Vipul', age: 8 }
    ]

> $in : $nin

    const  readDocument = () => {
	    UserData.find({ gender : {$in : "Male"} }).select({ fname: 1, _id: 0, gender: 1 })
	    .then((res) => { console.log(res) })
	    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    [
      { fname: 'Vishal', gender: 'Male' },
      { fname: 'Vivek', gender: 'Male' },
      { fname: 'Vikas', gender: 'Male' },
      { fname: 'Vipul', gender: 'Male' }
    ]
    
####  Logical query operators
|Name| Description |
|--|--|
|  $and| return documents which matches both queries |
|  $not|  return documents which fail to matches given query|
|  $nor|  return documents which fail to match both queries|
|  $or|  return documents which matches any one query|

> $and

    const  readDocument = () => {
	    UserData.find({$and : [{gender:"Male"},{fname:"Vikas"}]})
	    .select({ fname: 1, _id: 0, gender: 1 })
	    .then((res) => { console.log(res) })
	    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    [ { fname: 'Vikas', gender: 'Male' } ]

> $or

    const  readDocument = () => {
	    UserData.find({$or: [{gender:"Male"},{fname:"Vikas"}]})
	    .select({ fname: 1, _id: 0, gender: 1 })
	    .then((res) => { console.log(res) })
	    .catch((err) => { console.log("coudn't find the data") })
    }

***console***

    [
      { fname: 'Vishal', gender: 'Male' },
      { fname: 'Vivek', gender: 'Male' },
      { fname: 'Vikas', gender: 'Male' },
      { fname: 'Vipul', gender: 'Male' }
    ]

> $nor

    const  readDocument = () => {
    	    UserData.find({$nor: [{gender:"Male"},{fname:"Vikas"}]})
    	    .select({ fname: 1, _id: 0, gender: 1 })
    	    .then((res) => { console.log(res) })
    	    .catch((err) => { console.log("coudn't find the data") })
        }
***console***

    [ { fname: 'Sohani', gender: 'Female' } ]
### Sorting and count query methods

> "countDocuments()" method gives the count of documents which are
> satisfying the find query.

example : 

    const  readDocument = () => {
    UserData.find({gender:"Male"})
    .select({ fname: 1, _id: 0, gender: 1 })
    .countDocuments()
    .then((res) => { console.log(res) })
    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    4

> "sort()" can be used to sort in ascending(1) and descending(-1) order

Example1

    const  readDocument = () => {
	    UserData.find()
		    .select({ fname: 1, _id: 0, gender: 1 })
		    .sort({fname:1})
		    .then((res) => { console.log(res) })
		    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    [
      { fname: 'Sohani', gender: 'Female' },
      { fname: 'Vikas', gender: 'Male' },
      { fname: 'Vipul', gender: 'Male' },
      { fname: 'Vishal', gender: 'Male' },
      { fname: 'Vivek', gender: 'Male' }
    ]
Example 2:

    const  readDocument = () => {
	    UserData.find()
		    .select({ fname: 1, _id: 0, gender: 1 })
		    .sort({age:-1})
		    .then((res) => { console.log(res) })
		    .catch((err) => { console.log("coudn't find the data") })
    }
    readDocument()
***console***

    [
      { fname: 'Vikas', gender: 'Male' },  
      { fname: 'Vishal', gender: 'Male' }, 
      { fname: 'Vivek', gender: 'Male' },  
      { fname: 'Vipul', gender: 'Male' },  
      { fname: 'Sohani', gender: 'Female' }
    ]
## Updating the document
Example 1 :

    const  updateDocument = (fname) => {
	    UserData.updateOne({fname:fname},{
		    $set:{
		    lname:"Prajapati",
		    }
		    }).then((res)=>{console.log(res)})
    }
    updateDocument("Vikas")
***console***

    {
      acknowledged: true,
      modifiedCount: 1,
      upsertedId: null,
      upsertedCount: 0,
      matchedCount: 1
    }

Example 2 : 

    const  updateDocument = (lname) => {
	    UserData.updateMany({lname:lname},{
		    $set:{
		    lname:"Prajapati",
		    }
		    }).then((res)=>{console.log(res)})
    }
    updateDocument("Anand")
***console***

    {
      acknowledged: true,
      modifiedCount: 4,
      upsertedId: null,
      upsertedCount: 0,
      matchedCount: 4
    }
## Deleting the document

Example 1 :

    const  deleteDocument = (lname) => {
	    UserData.deleteOne({age:{$gte:30}}).then((res)=>{console.log(res)})
    }
    deleteDocument()
***console***

    { acknowledged: true, deletedCount: 1 }
Example 2 :

    const  deleteDocument = () => {
	    UserData.deleteMany({age:{$gte:30}}).then((res)=>{console.log(res)})
    }
    deleteDocument()
***console***

    { acknowledged: true, deletedCount: 3 }

## Inbuilt validations in mongoose

 - Mongoose inbuilt validators are applied in schema as key-value pair.
 - https://mongoosejs.com/docs/schematypes.html

// SCHEMA >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

    const  myFirstSchema = new  mongoose.Schema({
	    fname: {
		    type: String,
		    required: true, //this will make the field required
		    unique:true, // all the fname will be unique
		    // uppercase:true, // string will be saved in uppercase (VIKAS)
		    // lowercase:true, // string will be saved in lowercase (vikas)
		    trim:true, // this will trim the empty spaces form starting and ending (____v_ik__as__)=>(v_ik__as)
		    minlength:3, // min acceptable length of the string
		    maxlength:20  // max acceptable length of the string.
	    },
	    lname: {
		    type: String,
		    default: "Anand"
	    },
	    gender: {
		    type:String,
		    required:true,
		    enum:["Male", "Female"] // this defines the possible inputs
	    },
	    age: {
		    type:Number,
		    required:true,
		    min:1, //min possible age
		    max:120  //max possible age
	    },
	    adult: Boolean,
	    lastUpdate: {
		    type: Date,
		    default: Date.now
	    }
    })
## Writing Custom validators in mongoose
- Mongoose inbuilt validators are applied in schema as key-value pair.
 - https://mongoosejs.com/docs/schematypes.html

Example 1 :
// SCHEMA >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

    const  myFirstSchema = new  mongoose.Schema({
    ...,
    age: {
	    type:Number,
	    // applying custom validation for age
	    validate(value){
		    if(value<1 || value>120){
			    throw  new  Error('1>=age>=120')
		    }
	    }
    },
    ...
    })
## Using NPM based validator

    npm i validator
Example 1 :

    ...
    const  validator = require("validator")
    ...
    const  myFirstSchema = new  mongoose.Schema({
	    ...,
	    email:{
		    type:String,
		    require:true,
		    unique:true,
		    // using npm validator
		    validate(value){
			    if(!validator.isEmail(value)){
				    throw  new  Error("Enter valid email")
			    }
		    }
	    }
    })
    ...
