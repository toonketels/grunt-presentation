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

to download "your version" (or a later one) of grunt. You can always edit `package.json`
to lock it down for an exact version of grunt in case of issues.

The `package.json` is now updated like this:

    {
      "name": "grunt-presentation",
      "version": "0.1.0",
      "devDependencies": {
        "grunt": "~0.4.1"
      }
    }

Download `grunt-contrib-sass` and `grunt-jade` so we have some grunt plugins
to play with.

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




### Scaffolding - a special task


## Meta
