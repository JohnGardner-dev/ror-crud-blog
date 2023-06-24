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


## Generating a Model and Database Migrations

### Steps:

* To create a model, we'll use a generator commnand again:
  * `bin/rails generate model Article title:string body:text`
    * `bin/rails` is the configuration directory
    * `generate model` is what generates the model
    * `Article` is the name of our model
    * `title:string` tells our db migrations to create a db column called `title` with a type `string`
    * `body:text` tells our db migrations to create a db column called `body` with a type `text`
  * This command creates a "migration file" and a "model file"
    * migration file: `db/migrate/<timestamp>_create_articles.rb`
    * model file: `app/models/article.rb`

### Migration File

```
class CreateArticles < ActiveRecord::Migration[7.0]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :body

      t.timestamps
    end
  end
end
```
* the call to `create_table` specifies how the `articles` table should be constructed
* `t.string :title` creates a column of type `string` named "title"
* `t.text :body` creates a column of type `text` named "body"
* `t.timestamps` is created automatically and creates two additional columns:
  * `created_at` and
  * `updated_at`
  * Rails will manage these columns by itself

Next, run the migration: `bin/rails db:migrate`

### Using a Model to Interact w/ the DB

To create an instance of our model and interact with it, we need to use the Rails Console:
* `bin/rails console`
Now that we're in the console, we can create initialize a new `Article` object:
* `article = Article.new(title: "Hello Rails", body: "I am on Rails!")`
* `article` is the variable we're assigning our new `Article` object to
* `Article.new(...)` creates a new instance of the `Article` object
* `title: "Hello Rails"` sets the `title` column to the string `"Hello Rails"`
* `body: "I am on Rails!"` sets the `body` column to the text `"I am on Rails!"`
IMPORTANT: All we did was initialized the object. At this point, it is only available within the console. We need to call `save` in order for it to be saved to our db:
* `article.save`
Now, when we type `article` in our console, it will return the Article object we created and assigned to the `article` variable.
To fetch this Article from the db, we can call `find` on the model and pass the `id` as an arguement:
* `Article.find(1)`
And, when we want to fetch ALL Articles from the db, we can call `all` on the model:
* `Article.all`
* This method returns an `ActiveRecord::Relation` object which we can think of as an superpowered `array`
  * These return objects look very similar when there's only one instance of the model, however the `ActiveRecord::Relation` object is surrounded by brackets `[]` to denote it's an array
