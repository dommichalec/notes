### Chapter One: From Zero to Deploy

**1.1 Introduction**

**1.2 Up and Running**

running ```gem install rails -v 5.0.0.1```
downloads the 5.0.0.1 version of rails onto your system.

**1.3 The First Application**

running ```rails _5.0.0.1_ new hello_app``` will create a new Rails version 5.0.0.1 application called ```hello_app```.

running ```bundle install``` will ensure all necessary Ruby gems are downloaded properly before building the application.

running ```bundle install --without production``` will install all gems onto the local machine with the exception of gems used in production.

running ```rails server -p 3000``` will explicitly run a local server at port number 3000 (```0.0.0.0:3000``` or simply ```localhost:3000```).

> **MVC:** When interacting with a Rails application, a **browser sends a request**, which is **received by a webserver and passed on to a Rails controller**, which is in charge of what to do next. In some cases, the controller will immediately render a view, which is a template that gets converted to HTML and sent back to the browser. More commonly for dynamic sites, **the controller interacts with a model, which is a Ruby object that represents an element of the site (such as a user) and is in charge of communicating with the database. After invoking the model, the controller then renders the view and returns the complete web page to the browser as HTML**.

in the context of an application controller, a standard Ruby method is referred to as a **controller action**.

**1.4 Version Control with Git**

version control lets us track changes, recover from errors, and collaborate with other people on our application over time. **We'll be using Git as our SCM in the tutorial**.

running ```which git``` shows you where Git is installed on your system (if installed already).

**First time setup**
```git config --global user.name "Your Name"```
```git config --global user.email "Your Email"```
```git config --global push.default matching```
```git config --global alias.co checkout```

The first step is to navigate to the root directory of the first app and initialize a new repository:

```git init```

The next step is to add all the project files to the repository using ```git add -A```

This command adds all the files in the current directory apart from those that match the patterns in a special file called ```.gitignore```. The ```rails new``` command automatically generates a ```.gitignore``` file appropriate to a Rails project, but you can add additional patterns as well.

The added files are initially placed in a staging area, which contains pending changes to our project. We can see which files are in the staging area using the ```git status``` command.

To tell Git we want to keep the changes, we use the ```git commit -m "Add your message here"``` command.

We can see a list of the commit messages using the ```git log``` command.

Git is incredibly good at making branches, which are effectively copies of a repository where we can make (possibly experimental) changes without modifying the parent files. In most cases, the parent repository is the ```master branch```, and we can create a new topic branch by using ```checkout``` with the ```-b``` flag:

running ```git co -b name-of-branch``` creates a new branch and switches to it at the same time.

running ```git branch``` will show which branch you're currently working from:

```
git branch
master
* name-of-branch
```

Once we’ve finished making our changes, we’re ready to merge the results back into our master branch:

```
git checkout master
Switched to branch 'master'
git merge name-of-branch
Updating af72946..9dc4f64
Fast-forward
README.md | 27 +++++----------------------
1 file changed, 5 insertions(+), 22 deletions(-)
```

After you’ve merged in the changes, you can tidy up your branches by deleting the topic branch using ```git branch -d``` if you’re done with it:

```
git branch -d name-of-branch
Deleted branch modify-README (was 9dc4f64).
```

**1.5 Deployment**

Deploying early and often allows us to catch any deployment problems early in our development cycle.

Heroku uses the PostgreSQL database (pronounced “post-gres-cue-ell”, and often called “Postgres” for short), which means that we need to add the ```pg``` gem in the production environment to allow Rails to talk to Postgres.

```
group :production do
  gem 'pg', '0.18.4'
end
```

To prepare the system for deployment we'll run ```bundle install --without production``` to prevent the local installation of any production gems.

It may be necessary to install the Heroku CLI using the Heroku Toolbelt.

Once you’ve verified that the Heroku command-line interface is installed, use the heroku command to log in and add your SSH key:

```
heroku login
heroku keys:add
```
Finally, use the ```heroku create``` command to create a place on the Heroku servers for the sample app to live:

```
heroku create
Creating app... done, fathomless-beyond-39164
https://damp-fortress-5769.herokuapp.com/ |
https://git.heroku.com/damp-fortress-5769.git
```

To deploy the application, the first step is to use Git to push the master branch up to Heroku with
```git push heroku master```

Finally, use ```heroku open``` to open the application on the world wide web.
