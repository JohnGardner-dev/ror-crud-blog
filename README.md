# Rails CRUD App

This is my first app using Ruby on Rails. Excited to give it a try.

## Steps I Took for Setup:

* Had to brew install Ruby to update to a new version that was compatible with Rails
* Had to change my ~/.zshrc to use the Homebrew version of Ruby and not the pre-installed version
* Started following the ["Getting Started" tutorial](https://guides.rubyonrails.org/getting_started.html) from rubyonrails.org
* after creating a blog with `rails new blog` I had to remove the .git directory from blog/
  * after cd'ing into blog
  * `rm -rf blog/.git`
  * used [this stackoverflow as a guide](https://stackoverflow.com/questions/56873278/how-to-fix-error-filename-does-not-have-a-commit-checked-out-fatal-adding)

## Saying Hello
* for this, we need to create at minimum:
  * a **route**
    * maps a request to a controller action
    * routes are "rules" written in a Ruby Domain-Specific Language
  * a **controller** with an action
    * performs the necessary work to handle the request **AND** prepares data for the view
  * and a **view**
    * displays data in a desired format

### Steps:
* add route to routes file `config/routes.rb`
* create `ArticlesController` and `index` action
  * `bin/rails generate controller Articles index --skip-routes`
    * `bin/rails` is the configuration directory
    * `generate controller` is what generates the controller
    * `Articles` is the name of the controller we made
    * `index` is controller action
    * `--skip-routes` tells the generator to not generate routes as we've already done that ourselves
  * the new controller we made is located in `app/controllers/articles_controller.rb`
    * inside the controller is the `index` action we defined
      * the `index` action is empty by default
      * because it's empty, Rails automatically renders a view that matches the name of the controller and the action
  * the new view created by the controller is in `app/views/articles/index.html.erb`
    * any html we put in this file will be rendered at `localhost:3000/articles
  
## Setting the App Home Page

### Steps:
* Now, we'll be displaying the same text from '/articles' to the root path `localhost:3000`
* In `config/routes.rb` we added a new route:
  * `root "articles#index"
* This routes the `root` route to our `index` action of our `ArticlesController`
  * `root` handles the root route
  * `"articles#index"` specifies the `ArticlesController` and the `index` action. Changing either of these would route to a different `controller` or a different `action`

## Autoloading
Our `ArticlesController` inherits from `ApplicationController`
* `class ArticlesController < ApplicationController`
  * the class after the `<` denotes inheritance
We do not have to have something like `require "application_controller"` because Application Classes and Modules are available everywhere.
We only need `require` calls for two cases:
* To load files under the `lib/` directory
* To load gem dependencies that have `require: false` in the `Gemfile`
