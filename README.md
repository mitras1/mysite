# mysite
**mysite testing for grunt push**

##Create a folder to contain the project and init your package.json file within it:
```shell
mkdir mysite && cd mysite
npm init
```
Start by installing the development dependencies you'll need to run Grunt and Browserify through Grunt:

```shell
npm i grunt grunt-browserify grunt-contrib-copy --save-dev
```
At the time of this writing, AngularJS does not have an official package on npm and that is too bad. But! That isn't going to stop us. You can easily install arbitrary packages not setup for `npm usingnapa:`

```shell
npm i napa --save-dev
```
Then in your `package.json` add the following install script:
```js
{
  "scripts": {
    "install": "napa"
  },
  "napa": {
    "angular": "angular/bower-angular",
    "angular-route": "angular/bower-angular-route"
  }
}
```
Now any time you run `npm install` it will also install AngularJS into the `node_modules/angular/folder` and Angular Route into the `node_modules/angular-route/` from their official releases.

##Folder Structure
Let's setup a folder structure based upon the angular-seed example. Create the files and folders to make your app look like the following (don't worry about what's in them right now):
```js
|- app/
|--- css/
|----- app.css
|--- js/
|----- app.js
|--- partials/
|----- partial1.html
|----- partial2.html
|--- index.html
|- Gruntfile.js
|- package.json
```
##Gruntfile
Next let's setup your `Gruntfile.js` to build your site upon typing the grunt command within the project folder:
```js
module.exports = function(grunt) {
  grunt.initConfig({
    browserify: {
      js: {
        // A single entry point for our app
        src: 'app/js/app.js',
        // Compile to a single file to add a script tag for in your HTML
        dest: 'dist/js/app.js',
      },
    },
    copy: {
      all: {
        // This copies all the html and css into the dist/ folder
        expand: true,
        cwd: 'app/',
        src: ['**/*.html', '**/*.css'],
        dest: 'dist/',
      },
    },
  });

  // Load the npm installed tasks
  grunt.loadNpmTasks('grunt-browserify');
  grunt.loadNpmTasks('grunt-contrib-copy');

  // The default tasks to run when you type: grunt
  grunt.registerTask('default', ['browserify', 'copy']);
};
```
###Run `grunt` and check

##Steps to add minifying in the build
Go to the project directory and run
```shell
sudo npm install grunt-contrib-uglify --save-dev
```
Add the below code in the `grunt.initConfig` -
```js
uglify: {
      my_target: {
        files: [{
          expand: true,
          cwd: 'app/',
          src: '**/*.js',
          dest: 'dist/'
        }]
      }
    }
```
	***Check uglify for useful codes***
	
####Load the npm task using - 
```shell
grunt.loadNpmTasks('grunt-contrib-uglify’);
```
Register the task - 
```shell
	grunt.registerTask('default', ['browserify', 'uglify', 'copy']);
```
Run `grunt` in the project directory.

##Clean
####Workflow Step: I need to delete the contents of the build directory before I initiate a new build process.

To accomplish step, I used the `grunt-contrib-clean` task.  

To install this task, you can simply use npm.  In addition, all of 
the tasks in this reference will be development dependencies so I will 
be using the `--save-dev` option for each of them.
```shell
npm install grunt-contrib-clean --save-dev
```
Next, we will add this task to the `Gruntfile.js` file.  We are creating a version of this task called build. This will delete the contents of the `build/ directory` before we start a new build.

```js
module.exports = function(grunt) {
  grunt.initConfig({
    clean: {
      build: [
        'build'
      ]
    }
  });
  grunt.loadNpmTasks('grunt-contrib-clean');
};
```
Now, you can simply run this task from the command line by executing `grunt clean:build` from the command line.

##JSHint
Workflow Step: I need to validate that my JavaScript code 
follows a set of best practices that I have defined and alert me if it 
does not meet the standards.

As with all of the tasks, the `grunt-contrib-jshint` can be installed using npm:
```shell
npm install grunt-contrib-jshint --save-dev
```
I have added the jshint task to my Gruntfile and also added a work target which will check both my Gruntfile as well as any JavaScript files in the root of my work/js/ directory.  I chose to not include any vendor specific files (such as jQuery) in this target.
```js
module.exports = function(grunt) {
  grunt.initConfig({
    ...
    jshint: {
      work: [
        'work/js/*.js',
        'Gruntfile.js' ]
    },
    ...
  });
  grunt.loadNpmTasks('grunt-contrib-jshint');
};
```

With this in place, you can run this task by executing `grunt jshint:work` on the command line.



http://davidtucker.net/articles/automating-with-grunt/
