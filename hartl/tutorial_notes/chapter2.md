### Chapter Two: A Toy App

In this chapter, we’ll develop a toy demo application to show off some of the power of Rails. The purpose is to get a high-level overview of Ruby on Rails programming (and web development in general) by rapidly generating an application using scaffold generators, which create a large amount of functionality automatically.

**2.1 Planning the Application**

**The typical first step when making a web application is to create a data model**, which is a representation of the structures needed by our application. In our case, the toy app will be a Twitter-style microblog, with only users and short (micro)posts. Thus, we’ll begin with a model for users of the app, and then we’ll add a model for microposts.

Users of our toy app will have a unique identifier called ```id``` (of type integer), a publicly viewable ```name``` (of type string), and an ```email address``` (also of type string) that will double as a unique username. The label ```users``` corresponds to a table in a database, and the ```id```, ```name```, and ```email``` attributes are columns in that table.

The core of the micropost data model is even simpler than the one for users: a micropost has only an ```id``` and a ```content``` field for the micropost’s text (of type text). There’s an additional complication, though: **we want to associate each micropost with a particular user**. We’ll accomplish this by recording the ```user_id``` of the owner of the post.

**2.2 The Users resource**

The combination of commands and subsequent code will constitute a Users resource, which will allow us to **think of users as objects that can be created, read, updated, and deleted through the web via the HTTP protocol.**

Rails scaffolding is generated by passing the ```scaffold``` command to the rails generate script. The argument of the scaffold command is the singular version of the resource name (in this case, User), together with optional parameters for the data model’s attributes:

```
rails generate scaffold User name:string email:string
      invoke  active_record
      create    db/migrate/20160515001017_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/models/user_test.rb
      create      test/fixtures/users.yml
      invoke  resource_route
       route    resources :users
      invoke  scaffold_controller
      create    app/controllers/users_controller.rb
      invoke    erb
      create      app/views/users
      create      app/views/users/index.html.erb
      create      app/views/users/edit.html.erb
      create      app/views/users/show.html.erb
      create      app/views/users/new.html.erb
      create      app/views/users/_form.html.erb
      invoke    test_unit
      create      test/controllers/users_controller_test.rb
      invoke    helper
      create      app/helpers/users_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/users/index.json.jbuilder
      create      app/views/users/show.json.jbuilder
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/users.coffee
      invoke    scss
      create      app/assets/stylesheets/users.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.scss
```

To proceed with the toy application, we first need to migrate the database using ```rails db:migrate```

> Here is a summary of the steps to highlight MVC using the User resource:

> 1. The browser issues a request for the /users URL.
2. Rails routes /users to the index action in the Users controller.
3. The index action asks the User model to retrieve all users (User.all).
4. The User model pulls all the users from the database.
5. The User model returns the list of users to the controller.
6. The controller captures the users in the @users variable, which is passed to the index view.
7. The view uses embedded Ruby to render the page as HTML.
8. The controller passes the HTML back to the browser.

You may notice that there are more controller actions than there are pages; **the index, show, new, and edit actions all correspond to pages, but there are additional create, update, and destroy actions as well.** These actions don’t typically render pages (although they can); instead, their main purpose is to modify information about users in the database. **This full suite of controller actions represents the implementation of the REST architecture in Rails, which is based on the ideas of representational state transfer**.

Though good for getting a general overview of Rails, the scaffold Users resource suffers from a number of severe weaknesses:

1. **No data validations**. Our User model accepts data such as blank names and invalid email addresses without complaint.
2. **No authentication**. We have no notion of logging in or out, and no way to prevent any user from performing any operation.
3. **No tests**. This isn’t technically true—the scaffolding includes rudimentary tests—but the generated tests don’t test for data validation, authentication, or any other custom requirements.
4. **No style or layout**. There is no consistent site styling or navigation.
5. **No real understanding**. If you understand the scaffold code, you probably shouldn’t be reading this book.

**2.3 The Micropost Resource**

One of the most powerful features of Rails is the ability to form associations between different data models. In the case of our User model, each user potentially has many microposts.

```
app/models/user.rb
 class User < ApplicationRecord
  has_many :microposts
end
```
**Inheritance hierarchies**
> We start with the inheritance structure for models. Both the User model and the Micropost model inherit from ApplicationRecord, which in turn inherits from ActiveRecord::Base, which is the base class for models provided by Active Record. It is by inheriting from ActiveRecord::Base that our model objects gain the ability to communicate with the database, treat the database columns as Ruby attributes, and so on.

> As with model inheritance, both the Users and Microposts controllers gain a large amount of functionality by inheriting from a base class (in this case, ActionController::Base), including the ability to manipulate model objects, filter inbound HTTP requests, and render views as HTML. Since all Rails controllers inherit from ApplicationController, rules defined in the Application controller automatically apply to every action in the application.