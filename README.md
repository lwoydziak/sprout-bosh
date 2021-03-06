# sprout-bosh

[![Build Status](https://travis-ci.org/pivotal-cf/sprout-bosh.png?branch=master)](https://travis-ci.org/pivotal-cf/sprout-bosh)

This project uses [soloist](https://github.com/mkocher/soloist) and [librarian-chef](https://github.com/applicationsonline/librarian-chef)
to run a subset of the recipes in sprout's cookbooks.

[Fork it](https://github.com/pivotal-cf/sprout-bosh/fork) to
customize its [attributes](http://docs.opscode.com/chef_overview_attributes.html) in [soloistrc](/soloistrc) and the list of recipes
you'd like to use for your team. You may also want to add other cookbooks to its [Cheffile](/Cheffile), perhaps one
of the many [community cookbooks](http://community.opscode.com/cookbooks). By default it configures an OS X
Mavericks workstation for Ruby development.

Finally, if you've never used Chef before - we highly recommend you buy &amp; watch [this excellent 17 minute screencast](http://railscasts.com/episodes/339-chef-solo-basics) by Ryan Bates.

## Installation under Mavericks (OS X 10.9)

### 1. Add an ssh key to the machines ssh-agent with access to all the repos you are cloning:
  The list of repos we clone can be found in the [soloistrc](https://github.com/pivotal-cf/sprout-bosh/blob/master/soloistrc) under the key: `node_attributes->sprout->git->projects`

    ssh-add -D
    ssh-add -t 5H [path/to/private-key]

### 2. Clone this project

    git clone https://github.com/pivotal-cf/sprout-bosh.git #note you may not have git yet, and will be prompted to install the command-line developer tools.  Go ahead and install
    cd sprout-bosh

### 3. Install soloist & and other required gems

If you're running under rvm or rbenv, you shouldn't preface the following commands with `sudo`.

    sudo gem install bundler
    bundle

If you receive errors like this:

    clang: error: unknown argument: '-multiply_definedsuppress'

then try downgrading those errors like this:

    sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future bundle

### 4. Run soloist

[The `caffeinate` command will keep your computer awake while installing; depending on your network connection, soloist can take from 10 minutes to 2 hours to complete.]

    caffeinate bundle exec soloist

#### 4.1 Re-run soloist untill it passes

some of the chef recipes may not pass the first time they are run (they download binaries from all over the internet, some time they don't get it all the first time) and you should try to re-run a couple of times.  If the recipe repeatadly fails you may need to disable the recipe in the [soloistrc](https://github.com/pivotal-cf/sprout-bosh/blob/master/soloistrc) and move on. You can test success by `echo $?` after the command finishes.  if it returns `0` that is success anything else is a failure and should be retried.

    caffeinate bundle exec soloist

### 5. Manually tweak the final set of things we don't yet have automated
1. Mirror displays
1. Configure Finder
  1. Add ~/pivotal to Finder sidenav (https://www.pivotaltracker.com/story/show/89970764)
  1. Add ~/pivotal/workspace to Finder sidenav (https://www.pivotaltracker.com/story/show/89970764)
  1. Remove 'All my files' from Finder sidenav and as start page (https://www.pivotaltracker.com/story/show/92454292)
1. Configure Rubymine
  1. Update RubyMine to use Native ssh executable for git  (https://www.pivotaltracker.com/story/show/89970316)
  1. Update RubyMine to use Darcula theme and Darcularge font profile
1. Configure Intellij
  1. Update Intellij Idea to use Native ssh executable for git  (https://www.pivotaltracker.com/story/show/89970316)
  1. Update Intellij Idea to use Default theme and Darcularge font profile
  1. Install 'idea' command line tool (https://www.pivotaltracker.com/story/show/89970526)
  1. Setup Intellij to work with Go plugin:
    1. Install Go plugin:
      1. Enter custom plugin repository:
        - go to: preferences -> Plugins -> browse repositories -> Manage repositories
        - enter 'https://plugins.jetbrains.com/plugins/alpha/list'
      1. Select only that repository in the 'Browse Repositories' window
      1. install Go
      1. restart
    1. Configure Go SDK
      1. go to: project settings (cmd+;) -> Platform Settings -> SDKs
      1. Add SDK (plus icon in top of column) -> Go SDK -> select '/usr/local/Cellar/go/1.x.x/libexec' -> 'Choose'
    1. Select SDK for each project
      1. go to: Project settings (cmd+;) -> Project Settings -> Project -> Project SDK
      1. choose Go 1.x.x
1. Gem install bundler (https://www.pivotaltracker.com/story/show/90239920)

## Discussion List

  Join [sprout-users@googlegroups.com](https://groups.google.com/forum/#!forum/sprout-users) if you use Sprout.

## References

* Slides from @hiremaga's [lightning talk on Sprout](http://sprout-talk.cfapps.io/) at Pivotal Labs in June 2013
* [Railscast on chef-solo](http://railscasts.com/episodes/339-chef-solo-basics) by Ryan Bates (PAID)
