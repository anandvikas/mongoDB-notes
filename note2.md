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