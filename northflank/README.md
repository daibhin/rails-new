# Northflank

Necessary setup changes in [this commit](https://github.com/daibhin/rails-new/commit/2bd1e90dc45a861ed4181f8f4d749bfee491c9ff)

## Prerequisites

### Buildpacks

If you're looking for the simplest way to host your project Northflank offers Heroku buildpacks as an alternative to Dockerfiles to automate the build process.

### Platform

Given the use of Buildpacks, the required changes are very similar to that needed from a Heroku app. The Ruby version needs to be upgraded to `3.x.x` to support the latest Buildpack stack (22 at the time of writing). Northflank will run your application on `x86_64` Linux machines and therefore requires you to bundle the platform before deploying by running:

```
bundle lock --add-platform x86_64-linux
```

## Deploying

### Setting up a server

Unlike many alternatives, the entire process is controlled through Northflank's web UI. They do not have a CLI or repository configuration files.

Within a project, after connecting your GitHub account, a 'Combined' (Build + Deploy) service can be created from a repository & branch. This will automatically deploy new builds on each change to that branch.

I found the recommended `heroku/builder-classic:22` stack did not work unless the 'Buildpack runtime mode' was changed to a custom process called `web` further on in the setup. Choosing the `heroku/builder:22` worked with no changes. All other configuration could remain unchanged.

### Secret managment

While Northflank lets you add secrets to individual services you can also create 'Secret groups'. Shared groups allow for secrets to be used across multiple services during build and/or runtime.

The `/config/credentials/production.key` can be set as `RAILS_MASTER_KEY` in this section.

### Database connection

Linking your add-ons to your app needs to be done through the above managed secret group. The connection details for the add-on (in this case a PostgreSQL database) are passed as variables to that secret group.

An alias for the `POSTGRES_URI` variable can be set to `DATABASE_URL` which will then be used by our `config/database.yml` file.

## Takeaways

### Swappable providers

Northflank hosts projects on their own infrastructure by default but allows for that to be interchanged with all of the major cloud providers (AWS, Google Cloud & Azure). Testing that was well beyond the scope of this analysis and its need is likely beyond the scale of any newly created app, but it is certainly a worthwhile consideration when mounting PaaS costs and vendor lock in from other PaaS providers is a very real concern.

### Heroku alternative

The Northflank model most closely matches that of Heroku, right down to the 'Projects' nomenclature. In an age where Heroku has canned all of its free offerings, Northflank is a good replacement to get started with as it allows a single free project per account. It even offers a Heroku migration tool that worked flawlessly for me.

One caution here is that Northflank offers more customizability than Heroku. It caters to many more frameworks and preferences Dockerfiles / user configuration which can be overwhelming to begin with. This added complexity is extremely well communicated and laid out through the UI once you become familiar with things.

### Rails guides are lacking

While the docs contain references for Ruby on Rails and the product itself actually lets you start by creating a service using a scafolded RoR GitHub project (relies on Dockerfiles), there is a distinct absence of a guide on how to host your existing RoR app on Northflank.