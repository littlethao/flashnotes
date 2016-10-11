## DataBoss Command Cheat Sheet

Language: Ruby  
Framework: Rails  
ORM: Active Record  
DB: PostgreSQL  
_Test Suite other than 'Test::Unit'_

##### Install rails:

You are expected to have a current version of Ruby installed.

`$ ruby -v` to check on your Ruby version.

`$ gem install rails` to install the Rails gem.

When setting up your new Rails app tell rails that you will be using PostgreSQL as Database.

##### The basic way:

`$ rails new <your_app_name>`

##### The 'we want PostgreSQL way':

`$ rails new <your_app_name> -d postgresql -T`

* `-d` preconfigures your app for a particular type of database, in this case PostgreSQL. _(By default, rails uses SQLite – which is a useful toy database, but can't be used on Heroku.)_
* By default, Rails uses `Test::Unit` for testing. The `-T` switch turns off the built-in Rails test suite, so you can set up your chosen Test Suite for your project, e.g. RSpec.

This will create a Rails application with "your_app_name" in a directory with "your_app_name" and install the gem dependencies that are already mentioned in Gemfile using bundle install. A number of auto-generated files and folders that make up the structure of a Rails application will also be created at the same time.

Folders generated and important for your database:
* `app` – has a subfolder 'models/' to store all models/tables you require for your database and application.
* `config` – configuration information, including `database.yml` which includes database configuration details and a routes file
* `db` - to store your database schema, as well as the database migrations.

##### Build your database:
Run a `rake` task to get your database built. Make sure you are inside your application directory:

`$ bin/rake db:create`

This should have created a database for your development environment and one for your test environment.

To verify:  

`$ psql` to connect to your own database

`$ \l` to see a list of databases created on your computer

`$ \q` to disconnect from your own database


*If you still require to create a database for your application, run:*

`$ bin/rake db:create RAILS_ENV=test`

or

`$ bin/rake db:create RAILS_ENV=development`

##### Creating Models

Models contain all the logic behind the 'nouns' that make up your app. In our case, these are going to be restaurants, reviews, etc. They add constraints to on how these objects can behave and tell the app how they should be represented in the database.

`$ bin/rails g model <table_name> <property>:<data type> <property>:<data type>`

e.g. `$ bin/rails g model student first_name:string last_name:string`

This command:
* creates a new model, which tells the app what a 'student' is and what properties (first name and last name) it has.
* creates a **migration** which contains instructions for Rake ('Ruby `make`') to update the database.

Looking at db/migrate/20151231193055_create_students.rb we shall see the following file has been created:

```ruby
class CreateStudents < ActiveRecord::Migration
  def change
    create_table :students do |t|
      t.string :first_name
      t.string :last_name

      t.timestamps
    end
  end
end
```

Note:  
Two timestamp columns are automatically added so we know when a given record was created or last updated. The migration itself is timestamped based on when we ran the generate command.

Now, run:

`$ bin/rake db:migrate` to update your database with the newly created model/table.

_To run migration on specific environments:

`$ bin/rake db:migrate RAILS_ENV=test`

or

`$ bin/rake db:migrate RAILS_ENV=development`_

When you made a mistake during generation:  

`$ rails d migration MigrationName` to remove the migration (in this case, MigrationName is CreateRestaurants).

Drop the tables in the databases that were made earlier:

`$ psql` to connect to your own database

`$ \c <database_name>` to connect to a database

`$ drop table <table_name>;` to delete a table from your database

`$ \q` to disconnect from your own database

Run migration again:

`$ bin/rake db:migrate`

This will run all of your database migrations once more.

-------------------------

If you need to change something, **don't edit the schema file**. If you want to remove database tables or change the schema in any way, instead write another migration that does that.
