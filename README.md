Converting Cloud9 sqlite3 to postgresql</br>
</br>
Here are instructions for converting a Cloud9 Rails application from sqlite3 to postgresql. The main reason for this is the deployment of the app to Heroku, which requires a postgresql database.</br>
</br>
After logging into Cloud9, create a new rails app. Cloud9 will provision and set up the server. By default, Cloud9 creates a role/user called ubuntu, which we will recall for later.</br>
</br>
Once in the rails app, open up your gem file and replace gem 'sqlite3' with gem 'pg'.</br>
</br>
Install the gem pg via command line: gem install pg.</br>
Now run: bundle install and then bundle update</br>

In the  Config folder, open database.yml</br>
Copy/paste the below code, changing just the password to match the password for your database</br>
default: &default</br>
  adapter: postgresql</br>
  encoding: SQL_ASCII</br>
  host: localhost</br>
  pool: 5</br>
  username: ubuntu</br>
  password: password</br>
</br>
development:</br>
  <<: *default</br>
  database: app_development</br>
</br>
test:</br>
  <<: *default</br>
  database: app_test</br>
</br>
production:</br>
  <<: *default</br>
  database: app_production</br>
</br>
</br>
Ctrl+s to save the file.</br>

Remeber the default cloud9 user 'ubuntu'? Now we're going to create the username ubuntu in postgres</br>
Start the postgresql server by running the following: sudo service postgresql start</br>
Access the postgres terminal, run: sudo sudo -u postgres psql</br>
Create the user: postgres=# CREATE USER ubuntu SUPERUSER PASSWORD 'yourpassword';</br>
Passwords of 'ubuntu' should match in postgres and the database.yml file</br>
Create a new database via: CREATE DATABASE “database_name”;</br>
Run list command to verify the database is created using: \list</br>
Your database will be first listed and postgres will be identified as the owner.</br>
We will now set 'ubuntu' as the owner of the databases. </br>
Run this command for each database (should be 4 databases at minimum): ALTER DATABASE (db name) OWNER TO ubuntu;</br>
If successful, you'll see a ALTER DATABASE message populate below the command you just ran</br>
If not, ensure you have the ; at the end of the ALTER DATABASE command.</br>
HINT: you can use the “up” arrow to populate the previous command.</br>
Run this command to quit the postgres command line: \q</br>
</br>
Now back at the terminal run: rake db:create</br>
Run: rake db:migrate</br>
</br>
Configuration of postgresql for Cloud9 IDE using Ruby on Rails is now complete. Have fun!</br>
</br>
Visit http://docs.c9.io for support, or to learn more about using Cloud9 IDE. </br>
To watch some training videos, visit http://www.youtube.com/user/c9ide</br>
