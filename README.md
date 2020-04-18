# Mongoose-cheatsheet
A list of commonly used mongoose snippets.
___
## Installing mongoose
*Make sure you have MongoDB and Node.js installed*  
Navigate to your project directory and install mongoose from the command line
```
npm i mongoose
```
___
## Setting up mongoose in your application
*Assuming your entry point is* `app.js`
```javascript
const mongoose = require('mongoose');
const uri = 'mongodb://localhost:27017/mydbname'
mongoose.connect(uri, {useNewUrlParser: true});
```
___
## Making a schema
```javascript
const fruitSchema = new mongoose.Schema({
    name: String,
    rating: Number,
    review: String
})
```
___
## Making models
```javascript
const Fruit = mongoose.model('Fruit', fruitSchema);
const pineapple = new Fruit({
    name: "Pineapple",
    rating: 4,
    review: "Does not taste good on pizza."
})
```  
___  
## Saving to the database
```javascript
pineapple.save();
```
*The database name will be* `mydbname`, *taken from* `uri`. *Mongoose will create a new database if it does not already exist. The collection name will be* `fruits`*, a Mongoose generated lowercase, pluralized version of the first argument in* `mongoose.model('Fruit', fruitSchema)`.

Run ```node app.js``` and read the changes using usual mongoDB commands:
```bash
> mongo
> show dbs
> use mydbname
> show collections
> db.fruits.find()
```
___

