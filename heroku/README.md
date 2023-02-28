# Heroku

Necessary setup changes in [this commit](https://github.com/daibhin/rails-new/commit/8dbe85d7bb0fbda5c2cb00a0b71c6403acb080de)

## Prerequisites

### Command line interface

Heroku relies __heavily__ on the use of their command line interface (CLI). It is assumed to be installed for the purposes of this demo and left as an exercise for the reader if not.

### Platform

Heroku requires you to bundle the `x86_64` and `ruby` platforms before deploying an application:

```
bundle lock --add-platform x86_64-linux --add-platform ruby
```

## Deploying

### Ruby version

The latest Heroku stack (`heroku-22`) does not support the default Ruby version of `2.7.2` included in a Rails app. Upgrading is simply a matter of changing the value in `.ruby-version` and your `Gemfile`.

### Provisioning infrastructure

Heroku does not rely on any form of infrastructure as code. Migrating databases and scaling web servers needs to be done via the command line or web dashboard:

```
heroku run rake db:migrate
heroku ps:scale web=1
```

### Secret decryption

In order to access secrets in production you will be required to set the decryption key from `/config/credentials/production.key` as the `RAILS_MASTER_KEY` environment variable. These can be configured through the dashboard settings.

## Takeaways

### Ease of use

Heroku has always been the gold standard for ease of deployment. There are literally no changes or choices to be made beyond upgrading the Ruby version. Creating an app is cleverly encapsulates in a single command:

```
heroku apps:create --stack=heroku-22
```

Everything from the automatic creation of the database right down to the inclusion of necessary git remotes in the above command means that publishing changes is just a matter of pushing the repository using plain old git:

```
git push heroku main
```

It's worth noting that this can be automated via GitHub "on merge", which caused a great loss of confidence in recent times [when it broke](https://status.heroku.com/incidents/2413).


### Pricing

Heroku recently [removed their free tier](https://help.heroku.com/RSBRUH58/removal-of-heroku-free-product-plans-faq) which means as of now an app with the cheapest server and database is still going to cost you $12 / month.

### Console access

Heroku provides the simplest CLI that comes with shell access. As is possible to execute any `rails` command in production simply by prefixing it with `heroku run`. This means that Rails console access in your terminal is just a simple, memorable command away:

```
heroku run rails c
```