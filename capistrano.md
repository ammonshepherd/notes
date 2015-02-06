# The Basics of Capistrano

Here's the workflow that capistrano helps with, and where it fits:

1. Make changes on some project on your laptop/development server.
2. Update the project's Git repository available publicly
3. Run capistrano on laptop/development server that will take the
   updates from the Git repo and push them to the production/staging/other
   server, and subsequently run any other arbitrary commands (like restarting
   the webserver).


 
# "Remote" Server Setup 
Use the steps in this tutorial to set up a testing environment:
https://github.com/mossiso/notes/blob/master/basic-lamp-server.md



# Set up Capistrano 3

Capistrano requires Ruby > 1.9.3. It is highly recommended to use a Ruby
environment manager usch as rvm, or rbenv (a list of many more:
https://github.com/wayneeseguin/rvm/blob/master/docs/alt.md).

If you have a project already started, put it under version control using git
and github.

Once all of that is set up enter the project directory and run the capistrano
command to turn it into a capistrano project:

    cap install

Next up, edit the staging.rb

http://capistranorb.com/documentation/getting-started/preparing-your-application/

Follow this turorial:

https://www.digitalocean.com/community/tutorials/how-to-automate-php-app-deployment-process-using-capistrano-on-ubuntu-13

Working on this tutorial - 2/6/15 - at step Configuring Production With Capistrano



