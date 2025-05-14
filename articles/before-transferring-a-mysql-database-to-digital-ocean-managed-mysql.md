# Before Transferring a MySQL Database to Digital Ocean Managed MySQL

MySQL has a lot of useful features, but is also very rigid and rule-based. This means making sure your new server is setup correctly and exporting and importing your data correctly can be a handful when transferring a MySQL database from your server to DigitalOcean’s managed MySQL. Making sure the settings in your DigitalOcean server matches your current setup can be crucial as well as making sure your schema is the same.

Here’s a step-by-step guide I recently took to transfer a MySQL database to a managed DigitalOcean MySQL server.

First I matched DigitalOcean’s Global SQL Modes with the old server. You can get a list of your old SQL Modes by running the following:

```mysql
select @@sql_mode
```

Once you have a list you can make sure the match the Global SQL Modes section under your settings in your DigitalOcean database.

Be mindful that DigitalOcean requires that your tables have primary indexes. This is very important for it’s automatic backups. If for some reason you need to disable this you can run the following before migrating or importing data:

```mysql
set session sql_require_primary_key = 0;
```

Keep in mind this will have to be run before every session in case you end up running more than one.

One big gotcha, too, is your server’s timezone! This is where DigitalOcean could step up the managed part of their service. To update the timezone for your DigitalOcean MySQL database can be done with the API. Setup your token from within DigitalOcean. Then you can use a service like Postman to run a few API requests. Rajkishore J. lays out some good instruction at https://www.digitalocean.com/community/questions/how-to-set-a-timezone-for-mysql-managed-database on how to update your DigitalOcean timezone via its API.

Hopefully that’s a good start in getting your settings to match! Now you should be ready to setup your tables. To migrate my tables, I used DataGrip. There are other tools around like DBeaver and Sequel Pro for Mac. In DataGrip you can compare databases and run a migration including your views and routines. You can even customized the queries before you run them. This is where it might be helpful to set the session as mentioned earlier. Also, make sure your specific selectors reference the new database if the name is different, update or delete your definers, and make sure your engines are compatible with DigitalOcean. MyISAM is not.

Once you’ve migrated your tables and everything else, you can simply use mysqldump to export your data to a local .sql file. There are a number of ways you can install mysqldump one of which is Brew if you’re on OSX. You’ve already run your migration so the structure is there. You just need to export the data so simply use the –no-create-info flag. For example you might run the following:

```mysql
mysqldump old --no-create-info
--result-file=&quot;/Users/topher/table_data&quot;
```

Now you have an .sql file full of inserts. Simply open the file or copy and paste the queries and run the insert statements. Sit back and relax. After the inserts are done you should be all set!!!