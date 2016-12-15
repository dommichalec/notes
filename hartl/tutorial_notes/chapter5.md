### Chapter Five: Filling in the layout ###

**5.1 Adding some structure**

**5.2 Sass and the asset pipeline**

The asset pipeline binds all asset files (lib, public, and assets) into a single
manifest file for efficiency in production. With the asset pipeline, we don’t
have to choose between speed and convenience: we can work with multiple nicely
formatted files in development, and then use the asset pipeline to make
efficient files in production. In particular, **the asset pipeline combines all
the application stylesheets into one CSS file (application.css), combines all
the application JavaScript into one JavaScript file (application.js), and then
minifies them to remove the unnecessary spacing and indentation that bloats file
size**. The result is the best of both worlds: convenience in development and
efficiency in production.

**5.3 Layout links**

The root method arranges for the ```root``` path / to be routed to a controller
and action of our choice.

```
root_path -> '/'
root_url  -> 'http://www.example.com/'
```

**use _path unless doing a redirect; then use _url**
>the common convention of using the path form except when doing redirects,
where we’ll use the url form. (This is because the HTTP standard technically
requires a full URL after redirects, though in most browsers it will work
either way.)

to generate an integration test, we can run a script such as:

```
rails g integration_test site_layout
```


if we just want to run a single test suite (in this case, an integraton test), we'd run:

```
rails t:integration
```
otherwise, we can run the entire test suite by running:

```
rails t
```

The example below uses some of the more advanced options of the ```assert_select```
method, seen before. In this case, we use a syntax that allows us to test for
the presence of a particular link–URL combination by specifying the tag name a
and attribute ```href```, as in ```assert_select "a[href=?]", about_path```
Here Rails automatically inserts the value of ```about_path``` in place of the
question mark (escaping any special characters if necessary), thereby checking
for an HTML tag of the form

```
test/integration/site_layout_test.rb
 require 'test_helper'

class SiteLayoutTest < ActionDispatch::IntegrationTest

  test "layout links" do
    get root_path
    assert_template 'static_pages/home'
    assert_select "a[href=?]", root_path, count: 2
    assert_select "a[href=?]", help_path
    assert_select "a[href=?]", about_path
    assert_select "a[href=?]", contact_path
  end
end
```

**5.4 User Signup: A first step**
