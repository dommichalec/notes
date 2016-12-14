### Chapter Three: Mostly Static Pages

In this chapter, we will begin developing the professional-grade sample application that will serve as our example throughout the rest of this tutorial. Although the sample app will eventually have users, microposts, and a full login and authentication framework, we will begin with a seemingly limited topic: the creation of static pages.

**3.1 Sample App Setup**

**3.2 Static Pages**

In this section, we’ll take a first step toward making dynamic pages by creating a set of Rails actions and views containing only static HTML. Rails actions come bundled together inside controllers (the C in MVC), which contain sets of actions related by a common purpose.

If you’re using Git for version control, you should run the following command to checkout a topic branch for static pages:

```
$ git checkout -b static-pages
```

pushing to remote repository branch can be achieved by running:

```
git push -u origin name-of-topic-branch
```
which can later be merged to master branch

**controller actions always try to render the corresponding view template at the end of execution (even empty contoller actions as in the StaticPages controller**

to generate the StaticPages controller (along with home and help actions), we'll run:

```
rails g controller StaticPages home help
```

if we wanted to undo a generation of code, we'd run the inverse ```rails destroy``` command.

another technique related to models involves undoing migrations, which are finalized by running ```rails db:migrate```. To undo a single migration we can run ```rails db:rollback``` or, ```rails db:migrate VERSION=0``` to go back to the very beginning. As you might guess, substituting any other number for ```0``` migrates to that version number, where the version numbers come from listing the migrations sequentially.

**HTTP**

The hypertext transfer protocol **(HTTP)** defines the basic operations ```GET```, ```POST```, ```PATCH```, and ```DELETE```. These refer to operations between a **client** computer (typically running a web browser such as Chrome, Firefox, or Safari) and a **server** (typically running a webserver such as Apache or Nginx)
> ```GET``` is the most common HTTP operation, used for reading data on the web; it just means “get a page”. ```POST``` is the next most common operation; it is the request sent by your browser when you submit a form. In Rails applications, ```POST``` requests are typically used for creating things (although HTTP also allows ```POST``` to perform updates). The other two verbs, ```PATCH``` and ```DELETE```, are designed for updating and destroying things on the remote server.

**3.3 Getting Started With Testing**

>
When deciding when and how to test, it’s helpful to understand why to test. Writing automated tests has three main benefits:

1. Tests protect against **regressions**, where a functioning feature stops working for some reason.
2. Tests allow code to be **refactored** (i.e., changing its form without changing its function) with greater confidence.
3. Tests act as a **client** for the application code, thereby helping determine its design and its interface with other parts of the system.

Our main testing tools will be *controller tests*, *model tests*, and *integration tests* (starting in Chapter 7). **Integration tests are especially powerful, as they allow us to simulate the actions of a user interacting with our application using a web browser.** Integration tests will eventually be our primary testing technique, but controller tests give us an easier place to start.

taking a look at the file
```test/controllers/static_pages_controller_test.rb```
we see:

```
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest

  test "should get home" do
    get static_pages_home_url
    assert_response :success
  end

  test "should get help" do
    get static_pages_help_url
    assert_response :success
  end
end
```
Each test simply gets a URL and verifies (via an *assertion*) that the result is a success. Here the use of ```get``` indicates that our tests expect the Home and Help pages to be ordinary web pages, accessed using a ```GET``` request. The response ```:success``` is an abstract representation of the underlying HTTP status code (in this case, 200 OK).

To begin our testing cycle, we need to run our test suite to verify that the tests currently pass. We can do this with the ```rails test``` command.

When developing an application, **often code will start to “smell”, meaning that it gets ugly, bloated, or filled with repetition.** The computer doesn’t care what the code looks like, of course, but humans do, so it is important to keep the code base clean by refactoring frequently.

**Adding titles to each template with <% provide() %>**

```
app/views/static_pages/home.html.erb

<% provide(:title, "Help") %>
<!DOCTYPE html>
<html>
  <head>
    <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
  </head>
```

> **provide** stores a block of markup in an identifier for later use. In our case, 'Help' in the symbol :title. The provide is enclosed in <% %> to indicate it is executing this code and not printing out in the view. Yield in this case just spits that block back out. The yield is enclosed in <%= %> to indicate it is being printed out into the view. Yield is responsible for inserting the contents of each page into the layout.
