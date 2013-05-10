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


### Why a task runner?

*  Invest time once up front to do the right thing, consistently do 
   the right thing without any time cost.

   This results in consistent quality, less bugs.

*  Create machine-readable documentation that make your workflow 
   reproducible [Why use make - Mike Bostock](http://bost.ocks.org/mike/make/).


### Why a JavaScript task runner?

*  Front-end needs build-systems with their client side MVC-frameworks,
   templating languages to compile to html, css preprocessors to be 
   compiled to css, JavaScript files to be linted, concatenated, 
   minified...
  
   JS makes it easier for front-end developers to use. This way a front
   and back end developer can use the same build system. Making the
   project easier to understand. 
  
   We can do `grunt -h` to see all available tasks.
  
   Back ender can create advanced task for front-end developer if needed.
  
*  So we can leverage npm.


### Why Grunt?

#### Requirements for using an existing task runner

1.  Easy to use (great API, docs)
2.  Future proof
3.  Don't want to write all code myself (plugin system)
4.  Everybody else uses it


#### Grunt aces all requirements

1. Easy to use

   Great (documented) API, great docs, great plugins to read the
   source from, posts, presentations...

   Did I mention the [website is great](http://gruntjs.com)?

2. Future proof

   *  The project has some momentum going and no signs of slowing down.    
   *  They have a [roadmap](https://github.com/gruntjs/grunt/wiki/Roadmap).
   *  Great [people are behind it](https://github.com/gruntjs?tab=members).
   *  Adopted by [open-source projects and organizations](http://gruntjs.com/who-uses-grunt).

3. Don't want to write all code myself (plugin system)

   Offers a rich [plugin eco system](http://gruntjs.com/plugins) powered by npm.

   npm makes it easy for anyone to contribute plugins.

4. Everybody else uses it

   *  It is gaining traction.
   *  If not using, mostly welcomed once Grunt gets introduced.


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


#### The Gruntfile

The `Gruntfile.js` consists of three parts:
1. Configuring tasks
2. Loading plugins and tasks
3. Defining task aliases and custom tasks

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

We can also execute multiple tasks at once

    grunt jade:prod sass

Or get help

    grunt --help
    grunt -h

That's all we need to know to execute tasks. We can now use Grunt.


### Tasks deep dive

> Maybe not so deep...

#### Different types of tasks

We've seen _alias tasks_. There are actually three types:
*  alias task
*  function task
*  multi task

_Function tasks_ are similar to `alias tasks` except they... execute
a function when called.

    grunt.registerTask('name', 'description', function);

A _multi task_ is the most common task when dealing with plugins. You
can define your own via:

    grunt.task.registerMultiTask('name', 'description', function)

This kind of task's execution is tied to a _target_. We have to possibility
to define multiple targets like 'dev', 'prod' and specify the configuration
for each one.

In our example, both jade and sass are multitasks. The plugin's documentation
should specify if multiple targets are possible.

For jade we've specified two targets `dev` and `prod`.

`grunt jade` will execute both targets one after the other
`grunt jade:dev` will execute only the dev task


#### Access data from within tasks

In multi tasks and functions we'll execute a function we've defined.

There are several ways of accessing data from within the function to do
something useful.


##### Arguments

We pass arguments to functions when executing a task:

    grunt introspect:one:two

`one` and `two` are passed to the function. We can access them via arguments
object or as named parameters, just like any other function.

    grunt.registerTask('introspect', function(argOne, argTwo) {
      arguments;          // { '0': 'one', '1': 'two' }

      argOne;             // one
      argTwo;             // two
    }

Other options is to get them from the "rich" `this` object.

    this.nameArgs
    this.args
    this.flags


##### "Rich" this object

Inside a task function, `this` is augmented with several objects and
parameters.

    { 
      nameArgs: 'introspect:one:two',
      name: 'introspect',
      args: [ 'one', 'two' ],
      flags: { one: true, two: true },
      async: [Function],
      errorCount: [Getter],
      requires: [Function],
      requiresConfig: [Function],
      options: [Function] 
    }

`target` is key we access withing a multi task.


##### Access configuration

Access the configuration object:

    grunt.config.get(this.name)

Will give:

    { 
      configOne: 'config one',
      configTwo: [ 1, 2 ]
    }


##### Access package information

Since we made the package part of the config object we do:

    grunt.config.get('pkg');

Will give:

    { 
      name: 'grunt-presentation',
      version: '0.1.0',
      devDependencies:{ 
        grunt: '~0.4.1',
        'grunt-contrib-sass': '~0.3.0',
        'grunt-jade': '~0.4.0'
      }
    }


##### Options

We can pass options when invoking a task:

    grunt introspect:one:two --env=demo

This can be accessed in two ways:

    grunt.option('env')         // demo
    grunt.option.flags()        // ['--env=demo']


#### Pass info to tasks

The other way arount we could say there are four ways to pass 
information to tasks:

*  Make it part of _configuration object_, configure per _target_
*  Add it to _package.json_
*  Pass as parameters
*  Pass as options


#### Async tasks

For async tasks, we need to indicate when we're done to prevent the
task from terminating too early.

Below we configure and register a multi task to perform a get request
and download the response in the downloads folder.

    grunt.initConfig({
      pkg: grunt.file.readJSON('package.json'),
      get: {
        'node-json': {
          'url': "http://nodejs.org/api/all.json",
          'name': 'node-docs.json'
        },
        'node-html': {
          'url': "http://nodejs.org/api/all.html",
          'name': 'node-docs.html'
        }
      }
    });


    grunt.registerMultiTask('get', 'Perform get request', function() {
  
      var http = require('http'),
          fs = require('fs'),
          config = grunt.config.get(this.name)[this.target],
          url =  config.url,
          name = config.name,
          prefix =  grunt.template.today('yyyy-mm-dd-hh-MM-ss'),
          dir =  'downloads',
          file = grunt.template.process("<%= dir %>/<%= prefix %>-<%= name %>",{ data: {dir: dir, prefix: prefix, name: name }}),
          done = this.async(),
          fd;
  
      http.get(url, function(res) {
  
        grunt.log.writeln("Got response: " + res.statusCode);
        grunt.log.writeln("Downloading...");
  
        res.setEncoding('utf8');
        fd = fs.openSync(file, 'w');
        
        res.on('data', function (chunk) {
          fs.writeSync(fd, chunk);
          grunt.verbose.writeln("Wrote chunk...");
        });
        
        res.on('end', function() {
            grunt.log.ok("Loaded");
            fs.closeSync(fd);
            done();
        });
      }).on('error', function(e) {
        grunt.fail.fatal("Got error: " + e.message);
      });
    });

The only thing required to successfully work with async tasks is to

1. Get the `done` function.

    `var done = this.async();`

2. Call it when... we're done.

    `done();`


#### Refactor and load tasks from own files

The `Gruntfile.js` can become too big when defining lots of custom tasks.

We have two options to separate our tasks into separate files:

1. Create Grunt contrib plugin (and share on npm)
2. Wrap tasks as modules and move to folder


##### Move tasks to separate folder

First, create a file per task in a different folder.

    mkdir tasks
    touch tasks/get.js

Wrap each task as a module:

    module.exports = function(grunt) {
      grunt.registerMultiTask('get', 'Perform get request', function() {
        // ...
      }
    }

Finally, load the files into the `Gruntfile.js`:

      grunt.task.loadTasks('tasks');

Done, everything works like before except your `Gruntfile.js` is less
cluttered.


### Magic config object keys

We've seen and used the config JSON object already a handful of times.
We know plugins `function tasks` and `multi tasks` specify which keys
to use.

Now, not all keys are equal. Some are magic.

Magic means they do some stuff behind the scenes are defined by Grunt 
to be used by us.

Let's look into some of these magic object keys.


#### Options

The `options` key is particularly useful for plugin authors. It allows
the author to specify a configuration in its plugin and allow the `options`
key to override his defaults.

We see this happening in the `jade` plugin.

Our config with `options` key:

    grunt.initConfig({
      jade: {
        dev: {
          options: {
            client: false,
            pretty: true
          }
        }
      }
    }

Inside the jade plugins `jade.js`:

    grunt.registerMultiTask('jade', 'Compile your Jade templates', function() {
   
      var defaults = {
        client: true,
        runtime: true,
        compileDebug: false,
        extension: null,
        wrap: null,
        locals: null,
        basePath: null
      };
  
      // Options object for jade
      var options = this.options(defaults);
  
      // ...
    }

The author activates the options by calling
    
    this.options({})

from within his task.


#### Files

The other area to introduce "lots" of "magic keys" is files.

Since a task runner as grunt has to deal a lot with files, there are quite
some configuration keys for that.


##### Formats

Specifying source/destination mapping can happen on three different ways.


###### Compact format

    grunt.initConfig({
      'cat: {
        one: {
          src: ['src/bb.js', 'src/bbb.js'],
          dest: 'dest/b.js',
        },
      },
    });

Pro
+ Succinct
+ We can add other file attributes too (more magic keys)

Con
- Only one mapping possible


###### File object format

    grunt.initConfig({
      cat: {
        one: {
          files: {
            'dest/a.js': ['src/aa.js', 'src/aaa.js'],
            'dest/a1.js': ['src/aa1.js', 'src/aaa1.js'],
          }
        }
      }
    });

Pro
+ Multiple mappings

Con
- We cannot add file attributes


###### File array format

    grunt.initConfig({
      cat: {
        one: {
          files: [
            {src: ['src/aa.js', 'src/aaa.js'], dest: 'dest/a.js'},
            {src: ['src/aa1.js', 'src/aaa1.js'], dest: 'dest/a1.js'},
          ],
        }
      }
    });

Pro
+ Multiple mappings
+ We can add other file attributes too (more magic keys)


##### Other file attributes

Compact and file array format allow for other file attributes like:

* `filter`
* `expand`
* `dot`
* ...

Checkout [documentation about config](http://gruntjs.com/configuring-tasks) to find out more.


##### What files magic does

Consider following task which will concatenate several files.

    Grunt.initConfig({
      cat: {
        'one': {
          'src': ['downloads/*.json'],
          'dest': 'cat/one.cat',
        },
        'two': {
          'src': ['downloads/*'],
          'dest': 'cat/two.cat',
          'filter': function(filepath) {
            var stats = require('fs').statSync(filepath);
            return stats.size < 600000;
          }
        }
      }
    });

    grunt.registerMultiTask('cat', 'Concatenate files', function() {

      var files = this.files;
  
      this.files.forEach(function(file) {
        var output = file.src.map(function(filepath) {
          return grunt.file.read(filepath);
        }).join('\n');
        grunt.file.write(file.dest, output);
      });
  
    });

Grunt creates a normalized array of file mappings accessible via 
`this.files`.

Although we've specified a pattern in the config, the array contains
only filename mappings.


#### A word of advice

Checkout the documentation to for "magic attributes" which will 
to something with the data provided. 

Especially when you create a configurable tasks, only take "magic keys"
when you want to use them like they're intended. 


### Logging and failing

To keep consistent terminal output we use `grunt.log`, `grunt.verbose`
and `grunt.fail` when communicating to user.

    grunt.log.writeln('My message');
    grunt.log.ok();

    // Only triggered with --verbose flag
    grunt.verbose.writeln('Message');

    grunt.fail.warn('Message');
    grunt.fail.fatal('Message');

Verbose logging will only be visible when the command is executed with
the `--verbose` flag.

    grunt get:node-json --verbose


### Scaffolding - a special task

#### Over and over again

Even for a some quick "messing around" or some front end experimentation 
I probably want to:

* Use `jade`
* Use `sass`
* Have a `package.json`
* Have an `index.html` ready
* Have `jQuery` ready to use
* Serve files from a server in case I want to do GET requests
* Watch for changes in jade/sass to automatically compile
* Live reload the page served on changes

And while we're at it it's probably best to:

* Add a `README.md`
* Add `.gitignore` to not track compiled files/node modules
* Add `.jshintrc` for linting configuration
* Add `.gitattributes` to prevent line ending troubles

Even if this takes 5 - 10 minutes, that still sucks. Luckily, grunt
comes to the rescue.


#### Grunt-init

Use `grunt-init` to do project scaffolding.

Just like `grunt-cli` it's supposed to be installed globally once.

    npm install -g grunt-cli

This only makes it possible to do scaffolding by running the command.
Projects are generated according to _templates_.


#### Project templates

Project templates are supposed to be installed in

    `~/.grunt-init/`

It's recommended to git clone grunt-init-templates in the directory.

    git clone git@github.com:toonketels/grunt-init-tk-front.git ~/.grunt-init/tk-front

There is no limit to the number of templates you can use. On the 
[Grunt website](http://gruntjs.com/project-scaffolding) you'll find some.

To view a list of all your installed templates do:

    grunt-init

Templates get the name of the directory they're downloaded into. This means
you and I can have different names for the same template.

To create a new project with a template do:

    cd /path/to/create/project/into
    mkdir project-name
    cd project-name

    grunt-init tk-front

Some questions will be prompted for and the project will be created.


#### The prompt

Grunt is pretty smart about prompting questions.

It offers decent defaults _(project name is project dir name, title is
Capitalized name)_. If you don't like the supplied defaults, create a 
`defaults.json` in `~/.grunt-init`. 

    {
      "author_name": "Toon Ketels",
      "author_email": "none",
      "author_url": "http://toonketels.be/"
    }

Your defaults will be used on all grunt projects on your computer.

This results in you pressing enter a lot instead of typing in your answer.


#### Create you own template

A template consists of three things:

    my-template/template.js
    my-template/rename.json
    my-template/root/

`template.js` contains the logic of creating new templates, prompting
for information...

`rename.json` is a mapping for renaming files. This is optional.

`root` is the directory all files will be copied from.


#### template.js

Expose two functions to make it work:

    exports.description = "Description";

    // The real meat
    exports.template = function(grunt, init, done) {}

In the template function, we mainly prompt the user for data, supply
our own data to be passed to the files to be copied over and command
the creation of files.

    exports.template = function(grunt, init, done) {
      "use strict";
    
      // Start prompting..
      init.process({
        'type': 'browser',
        "devDependencies": {
          "grunt": "~0.4.1",
          "grunt-contrib-connect": "~0.2.0",
          "grunt-jade": "~0.4.0",
          "grunt-regarde": "~0.1.1",
          "grunt-contrib-livereload": "~0.1.2",
          "grunt-contrib-sass": "~0.3.0"
        }
      }, [
        init.prompt('name'),
        init.prompt('version'),
        init.prompt('title'),
        {
          name: 'langcode',
          message: 'The language of the default index.html',
          default: 'en',
          warning: 'It must be an 2 character language code.'
        }
      ], function(err, props) {
    
        // Grab the files...
        var files = init.filesToCopy(props);
    
        // Actually copy (and process) files...
        init.copyAndProcess(files, props);
    
        // Generate package.json file.
        init.writePackageJSON('package.json', props);
    
        done();
      });
    };

In more detail:

Start the process of prompting for questions, once we have the data,
the callback gets executed and we do our thing.

    init.process({options}, [prompts], cb)

To use the build in default prompts do:
    
    init.prompt('name');

To create custom prompts do:

    {
      name: 'langcode',
      message: 'The language of the default index.html',
      default: 'en',
      warning: 'It must be a 2 character language code.'
    }

To copy the files in `root` dir and swap the variables do:

    // Grab the files...
    var files = init.filesToCopy(props);

    // Actually copy (and process) files...
    init.copyAndProcess(files, props);

To generate a package.json do:

    init.writePackageJSON('package.json', props);

And to generate a license do _(before copyAndProcess)_:

    var licenses = ['MIT'];
    init.addLicenseFiles(files, licenses);


#### Root

All files will be parsed and variables will be replaced with the data
supplied by the options object and data prompted for.

    {%= title %}


#### Final work on project scaffolding

Once you get used getting ready under one minute, you wont go back.

We're not only saving time, just as a normal task, we can invest some
time in tuning this setup, and never pay it when actually using it.


### Q&a

## Meta
