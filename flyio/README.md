# Fly

Necessary setup changes in [this commit](https://github.com/daibhin/rails-new/commit/c87cf0b7484eb0329d6d47f74c8bf9eb62581aac)

## Prerequisites

### Command line interface

Fly makes use of a command line interface (CLI) for many operations. It is assumed to be installed for the purposes of this demo and left as an exercise for the reader if not.

## Deployments

### Configuration choices

You will be asked to choose a configuration during the setup command. While it is never explained whether the difference in memory or compute pertained to the server or database, or why I would choose the "Development" option when clearly shipping to production, I just went with the default because it said that things could be changed freely down the line.

## Takeaways

### Explaining itself

Fly.io does not do a great job at explaining itself. For example, every resource (servers, databases, etc..) is referred to as an "App" on the dashboard. There is also this unexplained "Free builder" that gets created when you first launch an app. On top of all this, the resources are entirely decoupled from each other so It's not really clear what connects to what in the case of multiple apps.

### Pains of deployment

I experienced many issues when deploying my Rails app to the point that I never managed to deploy this sample app (but I did an earlier test app and can confirm that the listed changes should be all you need). The CLI caused multiple timeouts, but the app was actually created which meant when I re-ran the command I got an overage warning and could not proceed without a credit card. Unhealthy hosts and missing Docker (more below) gems were both issues during deployment, fixed simply by retrying the command.

### Magic shouldn't be explained

PaaS should make things simple. That does not mean adding a bunch of Docker files to my repository that are freely "customizable". I've never worked with Docker before in my life and find it more worrying than beneficial to have a liability in my code that could be changed despite me probably never knowing how or wanting to.

### Multi-region support

Fly heavily differentiates itself on the fact that your code is deployed close to users by offering multi-region support. Testing that out was beyond the scope of my test, but by default you are only prompted to deploy to a single region. It is also suggested that write heavy applications won't benefit because region specific read-replicas offer no advantage for the single write DB.