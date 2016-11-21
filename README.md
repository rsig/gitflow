HubFlow
=======

#### Installing & Updating

```sh
$ git clone git@github.com:rsig/gitflow.git
$ cd gitflow
$ sudo ./install.sh
```

Add the following line to the ~/.bash_profile file
```sh
source /usr/local/bin/hubflow-shortcuts
```

In each of the git project directories (EG: RS and Ember-RS) you will need to
run. If you delete your repository and reclone you will have to do this every
time.

```sh
$ cd /Volumes/Source/rs
$ git hf init

$ cd /Volumes/Source/ember-rs
$ git hf init
```
*Note* Once the initialization is done, create a new shell for the settings to take effect.

#### Features

Features are meant for bugfixes, features, and enhancements

```sh
$ feature start my-feature
```

This will start a new feature branch `feature/my-feature`. From here on out you
can use git as you would normally. Create commits and push to the remote. Once
you're happy with your work and wish to submit a pull request you can do the
following:

```sh
$ feature submit my-feature
```

This will submit a pull request opening up a chrome browser with the pull
request page pre-filled for this bit of work. The pull request should be opened
up against the `develop` long lived branch.  *note* this currently only works
on OSX.

* Mark the pull request with the correct tag: *pending reviewal* or *wip*
  (work in progress)
* Mark the pull request with the correct milestone (for the next release)
* Assign the pull request to a relevant developer
* Submit the pull request
* Wait for approval from the reviewer
* Once approved merge the pull request on github and make sure to choose the
  *Squash Commit* option after clicking merge.
* Delete the feature branch on github after merging

Now that you've completed a bit of work you can clean up your local machine

```sh
$ git checkout develop
$ git pull
```

Make sure your changes have made it into `develop` with:

```sh
$ git log
```

You now can delete your local branch since you're done with it.
Make sure you're on the `develop` branch and do the following

```sh
$ git branch -d feature/my-feature
```

#### Releases

Releases are done one to two times a week. A release contains pending features
that have been merged into the `develop` branch.  As a release captain you are
tasked of starting, validating,
finishing and publishing a release.

We release every Tuesday and Thursday. If for some reason we find a regression
during Tuesdays validation process it is okay for the release captain to take
Wednesday to rally the team and get a fix out, revalidate and release on
Thursday. That being said, if the offending pull request can be reverted prefer
that over delaying the release.

* In preperation for a release a milestone in github should be created with the
  release name.
* Ensure all pull requests that you wish to release have been added to the
  current milestone

```sh
$ git checkout develop
$ git pull
$ release start my-release
```

This will create the release feature branch `release/my-release`. From here
you'll deploy the release branch on staging for the verification process.

Once deployed let other team members know that the release is out on staging and
ask for help for verification. Each person should mark all pull requests for
this release as `verified` or `failed-verification`. During this time automated
tests should be run as well.

*remember* If a pull request has `failed-verification`, that is the time to
decide if the release should be delayed or not.

##### A production ready release
* Must have all pull requests verified
* Must not contain regressions
* Must pass all automated tests (unless they were written to validate a future
  bugfix)

After the release is ready for production do the following

* Close the milestone on github
* Finish the release
```sh
$ git checkout release/my-release
$ git pull
$ release finish my-release
```

This will merge the release into `master`, `develop` and create a tag with the
prefix `rss-` so the tag will be something like `rss-my-release`.

The final step is deploying the release to production. You made it!

Changelog
---------

To see what's new in each release, see our [Changelog](http://datasift.github.com/gitflow/ChangeLog.html).

License Terms
-------------
HubFlow is published under the liberal terms of the BSD License, see the
[LICENSE](LICENSE) file. Although the BSD License does not require you to share
any modifications you make to the source code, you are very much encouraged and
invited to contribute back your modifications to the community, preferably
in a Github fork, of course.
