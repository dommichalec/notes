### Chapter 10

Before filters use the ```before_action``` command to arrange for a particular method
to be called before the given actions. To require users to be logged in, we
define a ```logged_in_user``` method and invoke it using ```before_action``` ```:logged_in_user```.

```
Listing 10.15: Adding a logged_in_user before filter. red
app/controllers/users_controller.rb
 class UsersController < ApplicationController
  before_action :logged_in_user, only: [:edit, :update]
  .
  .
  .
  private

    def user_params
      params.require(:user).permit(:name, :email, :password,
                                   :password_confirmation)
    end

    # Before filters

    # Confirms a logged-in user.
    def logged_in_user
      unless logged_in?
        flash[:danger] = "Please log in."
        redirect_to login_url
      end
    end
end
```
You can also deploy the application and even populate the production database with sample users (using the pg:reset task to reset the production database):

```
$ rails test
$ git push heroku
$ heroku pg:reset DATABASE
$ heroku run rails db:migrate
$ heroku run rails db:seed
$ heroku restart
```

**10.5.1 What we learned in this chapter**

* Users can be updated using an edit form, which sends a ```PATCH``` request to the update action.
* Safe updating through the web is enforced using strong parameters.
* Before filters give a standard way to run methods before particular controller actions.
* We implement an authorization using before filters.
* Authorization tests use both low-level commands to submit particular HTTP requests directly to controller actions and high-level integration tests.
* Friendly forwarding redirects users where they wanted to go after logging in (saved first in the ```require_login``` method then ran in the ```create``` action in the sessions controller).
* The users index page shows all users, one page at a time.
* Rails uses the standard file db/seeds.rb to seed the database with sample data using rails db:seed.
* Running ```render @users``` automatically calls the ```_user.html.erb``` partial on each user in the collection.
* A boolean attribute called admin on the User model automatically creates an admin? boolean method on user objects.
* Admins can delete users through the web by clicking on delete links that issue DELETE requests to the Users controller destroy action.
* We can create a large number of test users using embedded Ruby inside fixtures.
