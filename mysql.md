# MySQL

## How do I delete a user?

    DROP USER <name>;

## How do I delete a database?

    DROP DATABASE <name>;

## How can I find out which users have access to a database?

    SELECT * FROM mysql.db WHERE Db = '<database>';

This will work even if you've dropped the database.
