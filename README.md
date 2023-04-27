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
Create a simple one page app
