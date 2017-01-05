### Chapter Seven: Sign Up

**7.1 Showing Users**

When following REST principles, resources are typically referenced using the resource name and a unique identifier. What this means in the context of users—which we’re now thinking of as a Users resource—is that we should view the user with id ```1``` by issuing a ```GET``` request to the URL ```/users/1```. Here the show action is implicit in the type of request—when Rails’ REST features are activated, ```GET``` requests are automatically handled by the show action.

**7.2 Sign Up Form**

To get a better picture of how Rails handles the submission, let’s take a closer look at the user part of the parameters hash from the debug information

```
"user" => { "name" => "Foo Bar",
            "email" => "foo@invalid",
            "password" => "[FILTERED]",
            "password_confirmation" => "[FILTERED]"
          }
```

This hash gets passed to the Users controller as part of ```params```, and **the ```params``` hash contains information about each request**. In the case of a URL like ```/users/1```, the value of ```params[:id]``` is the id of the corresponding user (1 in this example). In the case of posting to the signup form, params instead contains a hash of hashes, which introduced the strategically named ```params``` variable in a console session.

**7.5 Professional-grade deployment**

When submitting the signup form developed in this chapter, the name, email address, and password get sent over the network, and hence are vulnerable to being intercepted by malicious users. This is a potentially serious security flaw in our application, and the way **to fix it is to use Secure Sockets Layer (SSL) to encrypt all relevant information before it leaves the local browser**.

Enabling SSL is as easy as uncommenting a single line in ```production.rb```, the configuration file for production applications. All we need to do is set the config variable to force the use of SSL in production.

```
config/environments/production.rb
 Rails.application.configure do
  .
  .
  .
  # Force all access to the app over SSL, use Strict-Transport-Security,
  # and use secure cookies.
  config.force_ssl = true
  .
  .
  .
end
```

At this stage, we need to set up SSL on the remote server. Setting up a production site to use SSL involves purchasing and configuring an SSL certificate for your domain. That’s a lot of work, though, and luckily we won’t need it here: for an application running on a Heroku domain (such as the sample application), we can piggyback on Heroku’s SSL certificate. As a result, when we deploy the application, SSL will automatically be enabled. (If you want to run SSL on a custom domain, such as www.example.com, refer to [Heroku’s documentation on SSL](http://devcenter.heroku.com/articles/ssl).)

**7.6 What we learned in this chapter**

* Rails displays useful debug information via the debug method.
* Sass mixins allow a group of CSS rules to be bundled and reused in multiple places.
* Rails comes with three standard environments: development, test, and production.
* We can interact with users as a resource through a standard set of REST URLs.
* Gravatars provide a convenient way of displaying images to represent users.
* The form_for helper is used to generate forms for interacting with Active Record objects.
* Signup failure renders the new user page and displays error messages automatically determined by Active Record.
* Rails provides the flash as a standard way to display temporary messages.
* Signup success creates a user in the database and redirects to the user show page, and displays a welcome message.
* We can use integration tests to verify form submission behavior and catch regressions.
* We can configure our production application to use SSL for secure communications and Puma for high performance.
