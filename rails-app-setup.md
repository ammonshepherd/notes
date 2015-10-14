## Creating the Project Deployment App from scratch

# Basic Rails setup

1. Start a new rails application.
  
    Start by making a new rails application in your project directory (or
    wherever you want it)
 
   `rails new bqpd`


2. Update Gemfile and get gems

    Add the ruby version and ruby gemset to the file 'bqpd/Gemfile'

    ```
    #ruby=ruby-2.2.2
    #ruby-gemset=rails
    ```

    Install all of the needed Gems.

    ```
    cd bqpd
    bundle install
    ```

3. Use version control

    Use git to manage version control

    ```
    git init .
    git add .
    git commit -m "Brand new Rails app"
    ```

4. Generate the rails scaffolding

    ```
    rails generate scaffold Project title:string description:string default_task_id:integer
    rails generate scaffold Task title:string description:string command:string project:references
    rails generate scaffold Job task_name:string log:text task:references
    ```

    - NOTE: The name used in the scaffold should be singular

    Add two migrations for database changes

    ```
    rails generate migration AddDefaultTaskToProjects
    ```

    ```
    class AddDefaultTaskToProjects < ActiveRecord::Migration
      def change
        add_column :projects, :default_task_id, :integer
        add_foreign_key :projects, :tasks, column: :default_task_id
        add_index :projects, :default_task_id
      end
    end
    ```

    and then run

    ```
    rails generate migration AddTaskRefToJobs
    ```

    ```
    class AddTaskRefToJobs < ActiveRecord::Migration
      def change
        add_reference :jobs, :task, index: true, foreign_key: true
      end
    end
    ```

    Now we can run the migrations to set up the database.

    ```
    rake db:migrate
    ```

5. Edit models for associations

    All of these files are in app/models/

    project.rb
    ```
    class Project < ActiveRecord::Base

      has_many :jobs

    end
    ```

    job.rb
    ```
    class Job < ActiveRecord::Base

      belongs_to :projects

    end
    ```

6. 
