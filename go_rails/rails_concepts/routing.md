### A Look Into Routing

```resources :books``` defines the CRUD for books in a Rails application

What if we wanted to ```patch``` a single book as to a ```published?```
attribute? We can use ```member``` action specifier as shown below:

```
resources :books do
	member do
		patch :publish  # this will produce a route for books/:id/publish (automatically routes to books#publish action)
		patch :unpublish  # this will produce a route for books/:id/unpublish (automatically routes to books#unpublish action)

	end
end
```

we can also operate on a collection of all books with ```collection```:

```
resources :books do
	collection do
		patch :publish_all  # this will produce a route for books/publish_all (automatically routes to books#publish_all action)
		post :import  # this will produce a route for books/import (automatically routes to books#publish_all action)
	end
end
```

if we want to start migrating people over from ```books``` to ```products	``` but we don't know how we're going to do that quite yet, we can use the ```path:``` option to change how the URLs look:

```
resources :books, path: "products" do
	...
end
```
The above will keep the route helper methods, verbs, and controller#action pairs intact, but will change the URL pattern to include ```/products/:id``` instead of ```/books/:id``` in the URL path.
