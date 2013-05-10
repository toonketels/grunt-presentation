GRUNT PRESENTATION
==================

## Meta

* Introduction
* About us
* Overview


## What?

> Grunt in a nutshell

A JavaScript task runner.

Runs on the command line.

    grunt

    grunt build:dev

Runs with node.

Uses npm for distributing plugins (and itself).


## Why?

> Why use task runners and, why grunt?


### Why a taskrunner?

*   Invest time once up front to do the right thing, consistently do 
    the right thing without any time cost.

    This results in consistent quality, less bugs.

*   Create machine-readable documentation that make your workflow 
    reproducible [Why use make - Mike Bostock](http://bost.ocks.org/mike/make/).


### Why a JavaScript taskrunner?

*   Front-end needs build-systems with their client side MVC-frameworks,
    templating languages to compile to html, css preprocessors to be 
    compiled to css, JavaScript files to be linted, concatenated, 
    minified...

    JS makes it easier for front-end developers to use. This way a front
    and back end developer can use the same build system. Making the
    project easier to understand. 

    We can do `grunt -h` to see all available tasks.

    Back ender can create advanced task for front-end developer if needed.

*   So we can leverage npm.


### Why Grunt?

#### Requirements for using an existing task runner

1.  Easy to use (great API, docs)
2.  Future proof
3.  Don't want to write all code myself (plugin system)
4.  Everybody else uses it


#### Grunt aces all requirements

1.  Easy to use

    Great (documented) API, great docs, great plugins to read the
    source from, posts, presentations...

    Did I mention the [website is great](http://gruntjs.com)?

2.  Future proof

    *   The project has some momentum going and no signs of slowing down.    
    *   They have a [roadmap](https://github.com/gruntjs/grunt/wiki/Roadmap).
    *   Great [people are behind it](https://github.com/gruntjs?tab=members).
    *   Adopted by [open-source projects and organizations](http://gruntjs.com/who-uses-grunt).

3.  Don't want to write all code myself (plugin system)

    Offers a rich [plugin eco system](http://gruntjs.com/plugins) powered by npm.

    npm makes it easy for anyone to contribute plugins.

4.  Everybody else uses it

    *   It is gaining traction.
    *   If not using, mostly welcomed once Grunt gets introduced.


## How?

> How to get started with grunt?


### Installing Grunt

#### Dependencies

We only need [node.js](http://nodejs.org) and [npm](https://npmjs.org) to use Grunt.js.


#### grunt & grunt-cli

Grunt actually is `grunt` and `grunt-cli` npm packages.

Grunt-cli is supposed to be installed once globally.

    npm install -g grunt-cli

_Grunt-cli_ basically makes the `grunt` command available on your command
line, pointing to the correct `grunt` package.

_Grunt_ is the real deal. It contains the API.

By dividing it into two modules the problem of having different
versions of grunt (and hence the API) by different team members 
(installing grunt on different points in time).

Now, the "exact" version of grunt travels with your project in your
`package.json`.


### Using Grunt

#### Package.json

For grunt to work it expects 2 files in project root:

    package.json
    Gruntfile.js

Start by creating an new package.json.

    {
      "name": "grunt-presentation",
      "version": "0.1.0"
    }

Install grunt via npm.

    npm install grunt --save-dev

`--save-dev` updates `package.json` with the installed version of grunt.
Next time someone checks out your project they'll do:

    npm install

to download "your version" (or a later one) of grunt. You can always
edit `package.json` to lock it down for an exact version of grunt in 
case of issues.

The `package.json` is now updated like this:

    {
      "name": "grunt-presentation",
      "version": "0.1.0",
      "devDependencies": {
        "grunt": "~0.4.1"
      }
    }

Download `grunt-contrib-sass` and `grunt-jade` so we have some grunt 
plugins to play with.

    npm install grunt-contrib-sass --save-dev
    npm install grunt-jade --save-dev

The `package.json` now looks like:

    {
      "name": "grunt-presentation",
      "version": "0.1.0",
      "devDependencies": {
        "grunt": "~0.4.1",
        "grunt-contrib-sass": "~0.3.0",
        "grunt-jade": "~0.4.0"
      }
    }


#### Gruntfile.js

The `Gruntfile.js` consists of three parts:
1.   Configuring tasks
2.   Loading plugins and tasks
3.   Defining task aliases and custom tasks

Gruntfile.js:

    module.exports = function(grunt) {
      "use strict";
    gr
      // Config...
      grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        jade: {
          dev: {
            files: {
              'www/': ['jade/*.jade']
            },
            options: {
              client: false,
              pretty: true
            }
          }
          prod: {
            files: {
              'www/': ['jade/*.jade']
            },
            options: {
              client: false,
              pretty: false
            }
          }
        },
        sass: {
          dev: {
            options: {
              style: 'expanded'
            },
            files: {
              'www/css/main.css': 'sass/main.sass' 
            }
          }
        },
      });
    
    
      // Load tasks...
      grunt.loadNpmTasks('grunt-jade');
      grunt.loadNpmTasks('grunt-contrib-sass');
    
      // Task aliases and tasks
      grunt.registerTask('default', 'Compile jade/sass into html/css', ['jade:dev', 'sass:dev']);
    };


##### Module

The `Gruntfile.js` is wrapped within a `module.exports` to which the
`grunt` object is passed.


##### Configuration basics

Configuration is done by

    grunt.initConfig({})

to which a JSON configuration object is passed.

The first key of that object is the `package.json` made available
as `pkg` like so:

    pkg: grunt.file.readJSON('package.json')

This allows us to access the name and version as properties of the
configuration object:

    pkg.name
    pkg.version

The other keys are configuration objects specified by the contrib plugins
we load. Their github repo's should have good documentation.

We can also add our own custom data if we want to.


##### Loading tasks

Contrib plugins are loaded via

    grunt.loadNpmTasks('grunt-jade');

which is a shortcut for

    grunt.task.loadNpmTasks('grunt-jade');

This makes available all the tasks provided by the `grunt-jade` module.

Note that some modules require some configuration before the tasks can 
be run.


##### Custom tasks

Besides the tasks loaded via plugins, we define our own tasks:

    grunt.registerTask('name', 'description', [taskList]);

This is known as a _alias task_ as it will just run a set of other
tasks in sequence. Those tasks are specified as an array in taskslist.

If we really want to go crazy, we can specify _function tasks_ which
will... execute a function.

    grunt.registerTask('name', 'description', function);

We'll dive a bit deeper into the API a bit further but that's all there
is to know to set up Grunt.


##### Executing tasks

Okay, now everything is set up execute tasks on the command line.

To execute the default task:

    grunt

To execute any other task do:

    grunt jade:dev
    grunt sass

This will execute the `jade` and `sass` task. For jade it will execute 
the `dev` target (later more about that).

That's all we need to know to execute tasks. We can now use Grunt.



### Scaffolding - a special task


## Meta
