# Association Methods in Javascript

## Objectives
+ Understand how to use associate objects with a store.
+ Understand how to use JavaScript methods to find associated objects.

### Associating Objects

So far we have seen how to construct different types of objects in JavaScript with the class syntax.  

```js
  class Hotel {
    constructor(address){
      this.address = address
    }
  }

  new Hotel('350 5th Ave New York NY')
```

Now let's say we want to also construct a class to represent rooms for our house.  

```js
  class Room {
    constructor(squareFeet, roomNumber){
      this.squareFeet = squareFeet
      this.roomNumber = roomNumber
    }
  }

  new Room(400)
```

If we have a hotel chain, with multiple hotel properties and number of rooms in each property, we would want a way to associate rooms with a particular hotel.  To do so, we would first determine how a room is associated with a hotel.  There are two types of relationships for us to choose from:

1. Many to Many - that is, a room has many hotels, and a hotel has many rooms
2. HasMany and BelongsTo - that is, a hotel has many rooms and a room belongs to a hotel.

Because a room can only be associated with one hotel, we say that a room belongsTo a hotel and a hotel has many rooms.  This is very similar to our concept of associating data with sql.  If you are unfamiliar with that, you can check it out [here](https://github.com/learn-co-curriculum/sql-table-relations-readme)].

So could imagine representing the information in the following way:

```js
  [{roomNumber: 344, hotelName: 'The Sleeper on 5th'},
  {roomNumber: 346, hotelName: 'The Sleeper on 5th'},
  {roomNumber: 444, hotelName: 'The Moonlight on 8th'},
  {roomNumber: 242, hotelName: 'The Moonlight on 8th'},
]
```

However, there is the potential that two hotels may have the same name, or that hotels would share room number, we should probably associate them by giving each hotel an id, and each room an id.   

```js
  let store = {rooms: [
    {id: 1, roomNumber: 242, squareFeet: 400, hotelId: 1},
    {id: 2, roomNumber: 244, squareFeet: 300, hotelId: 1},
    {id: 3, roomNumber: 244, squareFeet: 300, hotelId: 2}
    ],
  hotels: [
    {id: 1, name: 'The Sleeper on 5th'},
    {id: 2, name: 'The Moonlight on 8th'},
    {id: 3, name: 'The PillowHead in Brooklyn'}
  ]}
```

Let's try to see what's going on in the data structure above.  We assign a variable called store to a JavaScript object.  The store object will represent all of the objects that are initialized.  The store object has two keys each of which points to an array: one to represent the collection of rooms and one to represent the collection of hotels.  

Let's see if we can answer some questions with our data structured like this.  For example, if we want to see the name of the hotel that is associated with our first room, just take a look at the hotelId which is 1, and then go find the hotel with id 1, and see that the name is The Sleeper on 5th.  We can also go find all of the rooms associated with the Sleeper on 5th.  To do so, we see that its id is 1, and then find all of the rooms with a hotelId of 1: the first and second rooms.  

So this is the structure we are aiming for.  How do we hook this up to our classes?

### Linking Instances to a Store

1. Assign an id each time we make a new instance

We need to assign each room object an id, and that id should increment each time we make a new room.  I bet if you close your eyes and rub your forehead with your index finger, you can think of the solution yourself.


```javascript
  let roomId = 0
  class Room {
    constructor(roomNumber, squareFeet){
      this.id = ++roomId
      // increment roomId, then assign the roomId as the instance's id
      this.squareFeet = squareFeet
      this.roomNumber = roomNumber
    }
  }

  let room = new Room(250, 242)
  // {id: 1, squareFeet: 250, roomNumber: 242}
  let secondRoom = new Room(200, 240)
  // {id: 2, squareFeet: 200, roomNumber: 240}
```

2. Our second task is to insert these new objects to the store

```javascript
let store = {rooms: []}
// initialize store with key of rooms that points to an empty array

let roomId = 0

class Room {
  constructor(roomNumber, squareFeet){
    this.id = ++roomId
    this.squareFeet = squareFeet
    this.roomNumber = roomNumber

    // insert in the room to the store
    store.rooms.push(this)
  }
}

let room = new Room(250, 242)
let secondRoom = new Room(200, 240)

store.rooms[0]
// {id: 1, squareFeet: 250, roomNumber: 240}
```
Ok, let's do the same thing with hotels.

```javascript
let store = {rooms: [], hotels: []}
// initialize store with key of rooms and hotels that each point to an empty array

let hotelId = 0

class Hotel {
  constructor(name){
    this.id = ++hotelId
    this.name = name

    // insert in the hotel to the store
    store.hotels.push(this)
  }
}
```

Finally, let's allow the ability to associate a hotel with a room.

```js
let roomId = 0

class Room {
  constructor(roomNumber, squareFeet, hotel){
    this.id = ++roomId
    this.squareFeet = squareFeet
    this.roomNumber = roomNumber
    if(hotel){
      this.hotelId = hotel.id
    }

    // insert in the room to the store
    store.rooms.push(this)
  }
  setHotel(hotel){
    this.hotelId = hotel.id
  }
}

let forest = new Hotel("The Forest Hotel")
let forestRoom = new Room(213, 240, forest)

store
// {hotels: [{id: 1, name: "The Forest Hotel"}], rooms: [{id: 1, number: 213, squareFeet: 240, hotelId: 1}]}
```

So from the code above, you can see that we can associate a room with a hotel either by passing through a room to a hotel upon initialization or by by calling a the `setHotel` setter method that we wrote.  

## Summary

In this lesson, we saw how we can use a plain javascript object to store and associate our data.  We showed how we can assign each new instance an id by modifying our `constructor` method.  We also saw that we can write setters or modify our constructor methods to provide an interface to associate a two objects.     

## Resources

+ [Sql Relations](https://github.com/learn-co-curriculum/sql-table-relations-readme)

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/js-classes-readme'>Classes in JS</a> on Learn.co and start learning to code for free.</p>
