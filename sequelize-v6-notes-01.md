# Sequelize v6 Notes (01)

### Installation

```
npm i sequelize
```

If you are using Typescript, we also include the corresponding types package

```
npm i @types/sequelize
```

### Setup

```
import {Sequelize} from 'sequelize';

const sequelize = new Sequelize({
    dialect: 'sqlite',
    storage: 'path/to/database.sqlite'
})

try {
    await sequelize.authenticate();

    // Do stuff

    sequelize.close();
} catch (e) {
    // Error handling
}
```

### Model

A model is a ES6 class that extends `sequelize.Model` class. It represents a table in your database.

You can use `sequelize.define(...)` to define your model. The models are stored in `sequelize.models`.

```
const User = sequelize.define(
  'User',
  {
    firstName: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    lastName: {
      type: DataTypes.STRING,
    },
  },
  {
    // freezeTableName: false,
    tableName: 'Users',
    timestamps: false,
    //createdAt: false,
    //updatedAt: 'updateTimestamp'
  },
);

// Will output true
console.log(User === sequelize.models.User);
```

_(Above code snippet taken from sequelize documentation)_

### Sync

We can sync a table with a sequelize model in three ways

- `User.snyc()` will create a table if the Users table does not exist and will do nothing otherwise
- `User.sync({ force: true })` will drop any existing Users table if it exists and create a new User table.
- `User.sync({ alter: true })` will create a Users table if it does not already exist, and will alter an existing User table to match the model if the table exists but the columns do not match with the sequelize model attributes

We can call `sequelize.sync({ ... })` to sync all tables in the database to all sequelize models - with the same options described above.

### Drop

`await User.drop()` will drop Users table.

`await sequelize.drop()` will drop all tables.

### Creation

`const jane = User.build({ firstName: 'Jane' })` will create a sequelize model instance.

`await jane.save();` will save the model instance to the database.

`const jane = User.create({ firstName: 'Jane' })` will create a sequelize model instance and save it to the database (`build` + `save`)

### Update

Once you have a sequelize instance, you can modify it directly and then use `save()` method to save to a database.

```
const jane = await User.create({ name: 'Jane' });
jane.name = 'Ada';
await jane.save();

```

_(Above code snippet taken from sequelize documentation)_

You can also use `.set(...)` method to change several fields at once.

```
const jane = await User.create({ name: 'Jane' });

jane.set({
  name: 'Ada',
  favoriteColor: 'blue',
});

await jane.save();

```

_(Above code snippet taken from sequelize documentation)_

You can use `.update(...)` to `.set()` and `.save()` at the same time - but update only saves to database the changes specified in update function call. The following example shows this

```
const jane = await User.create({ name: 'Jane' });

jane.favoriteColor = 'blue';

await jane.update({ name: 'Ada' });
// The database now has "Ada" for name,
//   but still has the default "green" for favorite color

await jane.save();
// Now the database has "Ada" for name
//   and "blue" for favorite color
```

_(Above code snippet taken from sequelize documentation)_

### Delete

`await jane.destroy()` will delete user instance.

### Reload

`await jane.reload()` will sync user instance to database information.

### Increment and Decrement

`await jane.increment('age', { by: 2 })` will increment numerical properties of model instance without concurrency issues.

`await jane.decrement('cash')` will decrement numerical properties of model instance without concurrency issues.

_Author: Gautam Ramasubramanian_

_Last Updated: May 4, 2024_
