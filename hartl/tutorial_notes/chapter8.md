### Chapter 8

**Basic login**

Now that new users can sign up for our site, it’s time to give them the ability to log in and log out. In this chapter, we’ll implement a basic but still fully functional login system: the application will maintain the logged-in state until the browser is closed by the user. The resulting authentication system will allow us to customize the site and implement an authorization model based on login status and identity of the current user.

**8.1 Sessions**

HTTP is a stateless protocol, treating each request as an independent transaction that is unable to use information from any previous requests. This means there is no way within the hypertext transfer protocol to remember a user’s identity from page to page; instead, web applications requiring user login must use a session, which is a semi-permanent connection between two computers (such as a client computer running a web browser and a server running Rails).

The most common techniques for implementing sessions in Rails involve using cookies, which are small pieces of text placed on the user’s browser.

**8.2 Logging in**

Implementing sessions will involve defining a large number of related functions for use across multiple controllers and views. Ruby provides a module facility for packaging such functions in one place. Conveniently, a Sessions helper module was generated automatically when generating the Sessions controller.

Moreover, such helpers are automatically included in Rails views; by including the module into the base class of all controllers (the Application controller), we arrange to make them available in our controllers as well.

Including the Sessions helper module into the Application controller.

```
app/controllers/application_controller.rb
 class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception
  include SessionsHelper
end
```

To find the current user, one possibility is to use the find method, as on the user profile page:
```User.find(session[:user_id])```

But recall that ```find``` raises an exception if the user id doesn’t exist. This behavior is appropriate on the user profile page because it will only happen if the ```id``` is invalid, but in the present case ```session[:user_id]``` will often be ```nil``` (i.e., for non-logged-in users). To handle this possibility, we’ll use the same ```find_by``` method used to find by email address in the create method, with ```id``` in place of ```email```:

```User.find_by(id: session[:user_id])```

**Rather than raising an exception, this method returns nil (indicating no such user) if the id is invalid**.

This is why developers tend to use ```find_by``` over ```find```.

We could now define the current_user method as follows:

```
def current_user
  User.find_by(id: session[:user_id])
end
```

This would work fine, but it would hit the database multiple times if, e.g., current_user appeared multiple times on a page. Instead, we’ll follow a common Ruby convention by storing the result of ```User.find_by``` in an instance variable, which hits the database the first time but returns the instance variable immediately on subsequent invocations:

```
if @current_user.nil?
  @current_user = User.find_by(id: session[:user_id])
else
  @current_user
end
```
Recalling the or operator ```||``` we can rewrite this as follows:

```
@current_user = @current_user || User.find_by(id: session[:user_id])
```
or simply:

```
@current_user ||= User.find_by(id: session[:user_id])
```

Because a ```User``` object is true in a boolean context, the call to ```find_by``` only gets executed if ```@current_user``` hasn’t yet been assigned.

With the working ```current_user``` method in Listing 8.16, we’re now in a position to make changes to our application based on user login status.

A user is logged in if there is a current user in the session, i.e., if current_user is not nil. Checking for this requires the use of the “not” operator, written using an exclamation point ! and usually read as “bang”. The resulting ```logged_in?``` method appears in:

```
app/helpers/sessions_helper.rb
 module SessionsHelper

  # Logs in the given user.
  def log_in(user)
    session[:user_id] = user.id
  end

  # Returns the current logged-in user (if any).
  def current_user
    @current_user ||= User.find_by(id: session[:user_id])
  end

  # Returns true if the user is logged in, false otherwise.
  def logged_in?
    !current_user.nil?
  end
end
```

**What we learned in this chapter**

* Rails can maintain state from one page to the next using temporary cookies via the ```session``` method.
* The login form is designed to create a new session to log a user in.
* The ```flash.now``` method is used for flash messages on rendered pages.
* Test-driven development is useful when debugging by reproducing the bug in a test.
* Using the session method, we can securely place a ```user id``` on the browser to create a temporary session.
* We can change features such as links on the layouts based on login status.
* Integration tests can verify correct routes, database updates, and proper changes to the layout.
