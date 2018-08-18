---
layout: post
title:      "I've Got a Crush on You - Sinatra"
date:       2018-08-18 16:55:06 +0000
permalink:  ive_got_a_crush_on_you_-_sinatra
---


Actually, I have a crush on Sinatra and ActiveRecord. Programming my Sinatra Portfolio Project made me really appreciate how awesome the two are when working in tandem. Sinatra's MVC structure keeps things simple and organized so it's easy to go back and forth between the controllers and views for all the different parts of the database, and copy or edit similar chunks of code. Instead of getting bogged down writing a ton of code to define relationships and map objects to my database, ActiveRecord makes it easy to store, retrieve and manipulate data behind the scenes. After the initial setup of the structure, how to present and interact with my Recipe Database becomes the main focus.


**SETTING UP THE STRUCTURE**

The beauty of the MVC structure is that you work from the outside in. For the most part, this is the order I followed:

* Set up all the files needed to work on or run my app: *config.ru*, *Gemfile*, *Rakefile* and *environment.rb*
* Create the database by writing migrations
* Set up the MVC folders within the *app* folder
* In the *models* folder, create classes that match the database tables: *User*, *Recipe*, *Ingredient* and *RecipeIngredient*

```
|__ config.ru
|__ Gemfile
|__ Rakefile
|__ config
|   |_ environment.rb 
|
|__ db
|   |_ migrate (migrations go in here)
|
|__ app
    |_ models
    |  |_ user.rb
    |  |_ recipe.rb
    |  |_ ingredient.rb
    |  |_ recipe_ingredient.rb
    |
    |_ views
    |_ controllers
```
						

If these are all set up properly, there shouldn't be any need to go back an touch any of these files. Which means that all the rest of your time can be spent working on the *controllers* and *views* files!


**WORKING ON THE GUTS**

```

|__ app
     |__ layout.erb
     |__ index.erb
     |
     |__ models
     |
     |__ controllers
     |   |__ application_controller.rb
     |   |__ ingredients_controller.rb
     |   |__ recipes_controller.rb
     |   |__ users_controller.rb
     |
     |__ views
		 |__ ingredients
		 |   |__ index.erb
		 |   |__ show.erb
		 |
		 |__ recipes
         |   |__ index.erb
         |   |__ show.erb
         |   |__ new.erb
         |   |__ edit.erb
         |
         |__ users
             |__ show.erb
	         |__ create_user.erb
	         |__ login.erb
					
```

It might be easier to think of the controllers as distinct categories and the views as being related to those categories. In this case the /app/index file would belong to the application_controller category. Looking at it this way, each category shares many similar views to other categories. Index pages function to show an index for everything in that category. Show pages will show details for the specific item in that category. Due to the functional similarities, the views and controller routes can be copied from one category to another with minor modifications to match that category.

```
  get '/recipes/:id' do
    if logged_in?
      @recipe = Recipe.find_by_id(params[:id])
      erb :'/recipes/show'
    else
      redirect to 'login', locals: {message: "Please login:"}
    end
  end
```

becomes

```
  get '/ingredients/:id' do
    if logged_in?
      @ingredient = Ingredient.find_by_id(params[:id])
      erb :'/ingredients/show'
    else
      redirect to 'login', locals: {message: "Please login:"}
    end
  end
```

Once you have your basic controller routes and view set up, then you can have fun playing with adding more features and functionality for users to interact with on your pages!



