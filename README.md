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
});
```
[Schema Types](https://mongoosejs.com/docs/api/schema.html#schema_Schema.Types)
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
## Saving to the database (`C`RUD)
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

## Saving multiple documents
```javascript
const kiwi = new Fruit({
    name: 'Kiwi',
    score: 9,
    review: 'Tasty!'
});
const banana = new Fruit({
    name: 'Banana',
    score: 8,
    review: 'Yum!'
});
const orange = new Fruit({
    name: 'Orange',
    score: 6,
    review: 'Sour.'
});

Fruit.insertMany([kiwi, banana, orange], function(err) {
    if (err) {
        console.log(err);
    }
    else {
        console.log("Successfully saved");
    }
})
```
[Model.insertMany()](https://mongoosejs.com/docs/api/model.html#model_Model.insertMany)
___

## Reading from your database (C`R`UD)
```javascript
Fruit.find(function(err, fruits) {
    if (err) {
        console.log(err);
    }
    else {
        console.log(fruits);
        fruits.forEach(function(fruit){
            console.log(fruit.name);
        mongoose.connection.close(); //Good practice to close the connection.
        });
    }
});
```
[Model.find()](https://mongoosejs.com/docs/api/model.html#model_Model.find)  
[Mongoose connection events](https://mongoosejs.com/docs/connections.html#connection-events)
___
## Updating the database (CR`U`D)
##### We create a model instance with a missing name field and save it first.
```javascript
const strawberry = new Fruit({
    score: 9,
    review: 'Delicious, what fruit is this?'
});
strawberry.save();
```

##### Update the entry
```javascript
Fruit.updateOne({review: 'Delicious, what fruit is this?'}, {name: 'Strawberry'}, function(err){
    if (err) {
        console.log(err);
    }
    else {
        console.log("Successfully updated an entry");
    }
});
```
[Model.update()](https://mongoosejs.com/docs/api/model.html#model_Model.update)  
[Model.updateMany()](https://mongoosejs.com/docs/api/model.html#model_Model.updateMany)  
[Model.updateOne()](https://mongoosejs.com/docs/api/model.html#model_Model.updateOne)  
___
## Data Validation with Mongoose
Modify your `fruitSchema` to only accept ratings between 1 and 10, and set the name field as required.
```javascript
const fruitSchema = new mongoose.Schema({
    name: {
        type: String,
        required: [true, "Why no name?"]
    }
    rating: {
        type: Number,
        min: 1,
        max: 10
    }
    review: String
});
```
[Mongoose Validation Docs](https://mongoosejs.com/docs/validation.html)
___
