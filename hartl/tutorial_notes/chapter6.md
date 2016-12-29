###Chapter Six: The User Model

**6.1 Modeling Users**

**Our goal in this section is to create a model for users that *persists* in
a database**

To store data Rails uses a relational database by default, which consists of
tables composed of data rows, where each row has columns of data attributes.

Much like creating a users controller, the analogous command for making a model
is ```rails generate model```, which we can use to generate a User model with
name and email attributes:

```
rails g model User first_name last_name email
```

One of the results of the generate command in Listing 6.1 is a new file called a
**migration**. Migrations provide a way to alter the structure of the database
incrementally, so that our data model can adapt to changing requirements.

```
db/migrate/[timestamp]_create_users.rb
 class CreateUsers < ActiveRecord::Migration[5.0]
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end
```

The migration itself consists of a ```change``` method that determines the
change to be made to the database.

We can run the migration, known as “migrating up”, using the db:migrate command
as follows:

```
rails db:migrate
```

Rails uses a file called ```schema.rb``` in the ```db```/ directory to keep
track of the structure of the database.

The ```update_attributes``` method accepts a hash of attributes, and on success
performs both the ```update``` and the ```save``` in one step
(returning true to indicate that the save went through). Note that if any of
the validations fail, such as when a password is required to save a record,
the call to ```update_attributes``` will fail. If we need to update only a
single attribute, using the singular ```update_attribute``` bypasses this
restriction.

Again, especially when testing for model validity, we can use
```rails test:models``` to run just the model tests
(compare to ```rails test:integration```).

A callback is a method that gets invoked at a particular point in the lifecycle
of an Active Record object.

In this chapter, we’ll take the first critical step by creating a *data model* for users of our site, together with a way to store that data.

> In Rails, the default data structure for a data model is called, naturally enough, a *model* (the M in MVC). The default Rails solution to the problem of persistence is to use a *database* for long-term data storage, and the default library for interacting with the database is called *Active Record*. Active Record comes with a host of methods for creating, saving, and finding data objects, all without having to use the *structured query language (SQL)* used by relational databases.

use ```rails console --sandbox``` to test out modeling and ```rails console``` when you actually want to make changes to the development database.

>> user.update_attributes(name: "The Dude", email: "dude@abides.org")
=> true
>> user.name
=> "The Dude"
>> user.email
=> "dude@abides.org"

The ```update_attributes``` method accepts a hash of attributes, and on success performs both the ```update``` and the ```save``` in one step (returning true to indicate that the save went through). Note that if any of the validations fail, such as when a password is required to save a record, the call to ```update_attributes``` will fail. **If we need to update only a single attribute, using the singular ```update_attribute``` bypasses this restriction.**

We should enforce certain constraints on object values. Active Record allows us to impose such constraints using *validations*.
Akin to integration tests, if we simply want to run the model test suite we can do so by running ```rails test:models```.
