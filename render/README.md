# Render

Necessary setup changes in [this commit](https://github.com/daibhin/rails-new/commit/07988de040f8bfb51383f4e71b2bb5f2be3f6b34)

## Prerequisites

### Platform

Render will run your application on `x86_64` Linux machines and therefore requires you to bundle the platform before deploying by running:

```
bundle lock --add-platform x86_64-linux
```

### Instance Type

Despite not being mentioned in their [docs](https://render.com/docs/blueprint-spec#instance-types) the key for instance types is actually called `plan`.

## Deploying


### Secret decryption

Because we are taking advantage of the secret management setup offered by Rails for both development and production, you will be prompted to enter the `RAILS_MASTER_KEY` when creating a Blueprint for your project.

This is the value in `/config/credentials/production.key` (which should be in your .gitignore)

## Takeaways

### Infrastructure as code

I really like the approach Render takes here where it is clear in the YAML file which resources are associated with a project.

### Blueprints vs projects

Add-ons are not as tightly coupled in Render as they traditionally are in the likes of Heroku. The main tab of your dashboard will show resources across all of your projects, if you want to see them "by project" you need to visit the Blueprints tab.

### Slow deploys

Perhaps it was when I tested but the time taken to deploy when using free instances was significantly longer than I'm used to. Owing to the fact that have a banner saying that free builds take longer, I'm going to assume it's intentionally slow.
