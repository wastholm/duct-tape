# Duct-Tape

Tools to patch up your leaky continuous delivery pipelines.

More precisely, a single tool. At least for now.


## clone-pipeline

I often find myself using [Jenkins](http://www.jenkins-ci.org/) to implement continuous delivery with multiple pipelines, and setting up all the stages and tasks almost identically for each pipeline gets old pretty quickly. Sure, there's the [Template Project plugin](https://wiki.jenkins-ci.org/display/JENKINS/Template+Project+Plugin), but it only reduces repetitive work; it doesn't eliminate it. A shell script to the rescue!


### Usage

Example:

```
$ clone-pipeline --host=jenkins.example.org --port=2222 foo bar
```

Connect via SSH to a running Jenkins instance, read the configuration for every job in a view named `foo`, create a new view called `bar` and populate it with new jobs corresponding to those in `foo`.


### Assumptions

* All jobs in the `foo` view have names beginning with `foo_`. (`foo_build`, `foo_unit-test`, `foo_deploy-dev`... you get the idea.)
* You want all jobs in the `bar` view to have names beginning with `bar_` (`bar_build`, etc.).
* All occurrances of `foo` in the existing job configurations are to be replaced with `bar` in the new ones. The intention is to preserve internal relationships between, for example, builds and unit tests, but the substitutions are made naïvely and you may encounter problems if your jobs have names that are likely to appear in the configuration XML itself, like `hudson` or `git` or `project`.


### Bells and Whistles

`clone-pipeline` can connect via either SSH or HTTP.

* To connect via SSH, make sure Jenkins' built-in SSHD is enabled and that you know what port it's listening on (see "Manage Jenkins" → "Configure System"); then use the `--host` and `--port` arguments to tell `clone-pipeline` how to connect.
* To connect via HTTP, use the `--jar` argument to give `clone-pipeline` the location of your `jenkins-cli.jar` file (to download it, see "Manage Jenkins" → "Jenkins CLI"). You will probably also need to use the `--uri` argument to tell the script the URI to your Jenkins instance.


### Bugs

Yes, probably, though none that I am aware of (unless you consider the simplistic nature of the job name substitution a bug). If you find any, let me know, or fix them yourself and shoot me a pull request.


### (Possible) Future Improvements

* Less naïve job name substitutions.
* Ability to clone pipelines from one Jenkins server to another.
* Some sort of configurable filtering mechanism (ability to pass job name configurations through some other script).
