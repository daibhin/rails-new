# Rails New

Walking through the setup documentation for popular PaaS providers in February 2023. The aim of the project is to produce rough starter templates, make a note of any gotchas and to understand general ease of use.

The specific instructions, observations and general takeaways for each platform are listed in the README of the individual directories

## Prerequisites

### DB choice
PostgreSQL seems to be the database of choice for these platforms. I have chosen, and would recommend, to initialize any new project to use it:

```
rails new app_name --database=postgresql
```

### Secret management
I am using Rails default credentials management in both development and production to keep things straightforward. By default Rails creates a single encrypted credentials file `config/credentials.yml.enc` for all environments. You can, as I have, split this into separate credentials per environment by running:

```
EDITOR="vi" rails credentials:edit --environment development
EDITOR="vi" rails credentials:edit --environment production
```

You will need to add a `secret_key_base` value to both files as it is required internally in Rails by `ActiveSupport`. A value for which can be generated using the `rails secret` command, and the files can be appended to using the same commands above, to include: 

```
secret_key_base: GENERATED_SECRET_KEY
```

You are then safe to delete the existing `config/credentials.yml.enc` and associated uncommitted key `config/master.key`.