#creating a table with Sequelize
Tomorrow, we're going to dive deep into managing the structure of the database.  Below is a quick script to use if you don't have the Countries database on your LEARN computer.

This Script creates a new table called 'my_countries' using Sequelize.  We first connect to the database, then define the model as normal.  Finally, we call ```MyCountry.sync()``` which creates the table, and sets up the fields according to the model definition.

```Javascript
let Sequelize = require('sequelize')

let sequelize = new Sequelize( 'countries', '','', {
  host: 'localhost',
  dialect: 'postgres',
  pool: {
    max: 5,
    min: 0,
    idel: 10000
  }
})


let MyCountry = sequelize.define('my_countries',{
  code: {
    type: Sequelize.STRING
  
  },
  name: {
    type: Sequelize.STRING 
  },
  continent: {
    type: Sequelize.STRING
  },
  region: {
    type: Sequelize.STRING
  }
})

MyCountry.sync({force: true})
  .then(function(){
    console.log('success!')
  })
  .catch(function(error){
    console.log(error)
  })


// Now that the Database is setup, we add some records
let countries = [
  {
    code: 'FLR',
    name: 'Florin',
    continent: 'Europe',
    region: 'Make Believe'
  },
  {
    code: 'GEN',
    name: 'Genovia',
    continent: 'Europe',
    region: 'Make Believe'
  }
]

function createCountries(){
  let promises = countries.map(function(country){
    return Promise.new(function(resolve,reject){
      return MyCountry.create(country)
    })
  })

  return Promise.all(promises)
}
```
