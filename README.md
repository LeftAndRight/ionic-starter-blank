This is an addon starter template for the [Ionic Framework](http://ionicframework.com/).

## How to use this template

*This template does not work on its own*. It is missing the Ionic library, and AngularJS.

To use this, either create a new ionic project using the ionic node.js utility, or copy and paste this into an existing Cordova project and download a release of Ionic separately.

### With the Ionic tool:

```bash
$ ionic start myApp https://github.com/LeftAndRight/ionic-starter-blank
$ cd myApp
$ ionic setup sass
$ npm install karma karma-jasmine karma-jasmine-jquery karma-phantomjs-launcher karma-spec-reporter --save-dev
```

Then copy this into the gulp file for testing:
```javascript
var Server 		= require("karma").Server;
var fs 			= require("fs");

gulp.task("test", function(done){

	// Pulls the script tags from the index.html
	function getInitialScripts(){
		var file			= "www/index.html";
		var excludes		= ["cordova.js"];

		var exHash			= {};
		excludes.forEach(function(item, i){ exHash[item] = true; });
		var index			= fs.readFileSync(file, "utf8");
		var scriptTags		= index.match(/<script.+?src=.+?<\/script>/g);
		var scriptUrls		= [];
		scriptTags.forEach(function(item, i){
			var srcReg		= /src="(.+?)"/g;
			var matches		= srcReg.exec(item);
			var value		= matches.length > 0 ? matches[1] : null;
			if (value && !exHash[value]){
				scriptUrls.push(value);
			}
		});
		return scriptUrls;
	}

	var options = {
		basePath	: "www",
		frameworks	: ["jasmine-jquery", "jasmine"],

		files		: getInitialScripts().concat([
			"lib/angular-mocks/angular-mocks.js",
			"js/**/*-test.js",
			"templates/**/*.html"
		]),

		reporters	: ["spec"],	// Reporter for pretty output
		logLevel	: "INFO",	// possible values: DISABLE || ERROR || WARN || INFO || DEBUG
		autoWatch	: true,
		browsers	: ["PhantomJS"],
		singleRun	: true,

		cmd			: "start"
	};

	new Server(options, done).start();
});
```
