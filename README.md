# README
This is my take on the final project of the Ruby on Rails cirriculum on The Odin Project: https://www.theodinproject.com/lessons/ruby-on-rails-rails-final-project.

This version builds on the social network concept, but it is tailored for Mum's to share their parenting journey.

# Setup steps
## Setting up the project
Create the new rails project using postgresql as the database using this command.
```bash
rails new odin_mumsquad --database=postgresql
```
Create a new github directory and push changes to github repo
```bash
git add .
git commit -m "Initial commit"
git remote add origin git@github.com:errolleesw/odin_mumsquad.git
git push -u origin main
```
Login to Github and confirm that code has been uploaded successfully.

## Login and home page
We will use devise to manage all user authentication.
### Set up devise user model
Add devise to the Gemfile
```bash
bundle add devise
```
Run the devise installer
```bash
bundle exec rails generate devise:install
```
Generate the user model and migrate
```bash
bundle exec rails generate devise user
bundle exec rails db:migrate
```
Creatge a couple of users in `db/seeds.rb`. Add these users to the `db/seeds.rb` file.
```bash
echo "
User.create(email: 'johndoe@doe.com', password: 'johndoe')
User.create(email: 'janedoe@doe.com', password: 'janedoe')
User.create(email: 'errolleesw@gmail.com', password: 'errolleesw')
" >> db/seeds.rb
```
Create the database for this app (makesure postgresql db is running locally with `brew services start postgresql@14`)
```bash
bin/rails db:create
```
Run the seed file
```bash
bundle exec rails db:seed
```
### Create posts model and use as homepage
Create the posts scaffold. This creates a full set of RESTful resources for a given model. This includes migration, a model file, a controller with CRUD actions and views for the model's index, show, new and edit actions.
```bash
rails generate scaffold Post title:string content:text
rails db:migrate # run this to migrate the db and create the table / fields.
```
Set the root to the posts index page in `routes.rb`
```bash
root 'posts#index'
```
### Protect app from unauthorised viewing
Add the following to `app/controllers/application_controller.rb`
```bash
before_action :authenticate_user!
```
Add navbar functionality and add logout button. Ignore the css classes for now, will come back to this later with styling.

Note: because we are using the hotwire/turbo method by default, we need to add the `turbo-action="replace"` and `turbo-method="delete"` data attributes to the logout link.
```html
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-end">
    <div class="navbar-item">
      <div class="buttons">
        <% if current_user %>
          <%= link_to "Logout", destroy_user_session_path, method: :delete, data: { turbo_action: "replace", turbo_method: "delete" }, class: 'button is-link' %>
        <% else %>
          <%= link_to "Login", new_user_session_path, class: 'button is-link' %>
        <% end %>
      </div>
    </div>
  </div>
</nav>
```
We also want to redirect the user to the login page after signing out. Add this to teh `application_controller.rb` file.
```ruby
class ApplicationController < ActionController::Base
  # ...

  protected

  def after_sign_out_path_for(resource_or_scope)
    new_user_session_path
  end
end
```

### Give it all a test drive
Spin up a server `rails s`. and Open localhost in a browswer. You should need to log in to access the home page.
