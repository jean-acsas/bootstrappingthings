## Scala and Eclipse-Scala-IDE installation for Linux

Painless install routine for Scala 2.11.8, sbt 13.11, and the Eclipse Scala-IDE.
*Bonus: suggested .gitignore file*

Tested on Ubuntu 18.04 LTE.

### Prerequisites

First make sure you've installed an appropriate **Java 8 SDK** (e.g. OpenJDK) by opening a terminal and running

```Bash
java -version
```

should you have multiple JDKs on your system, you can set the prefered one via

```Bash
update-java-alternatives --set
```

### Install Scala 2.11.8

```Bash
wget www.scala-lang.org/files/archive/scala-2.11.8.deb
sudo dpkg -i scala-2.11.8.deb
```

### Install sbt 13.11:

```Bash
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823
sudo apt-get update
sudo apt-get install sbt
```

### Install Eclipse Scala-IDE and setup a project

Get a fresh Eclipse-IDE, **one that suits your Scala version (here 2.11.8)**
- see http://scala-ide.org/download/prev-stable.html for your needed eclipse
- here, we go for Eclipse Oxigen: https://www.eclipse.org/downloads/packages/release/oxygen/3a
- This was tested on the vanilla Oxigen version: https://www.eclipse.org/downloads/packages/release/oxygen/3a/eclipse-ide-java-developers

After Extracting Eclipse, we can go ahead and create a Scala project.

*Tip: create a dedicated Eclipse workspace for your Scala projects.*

To create a new Scala project, navigate to your workspace and run

```Bash
sbt new scala/hello-world.g8
```

Enter a new for the project upon request.

To make the project Eclipse-usable, enter the directory and run:

```Bash
echo 'addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "5.2.4")' > project/plugins.sbt
sbt eclipse
```

This might take a little time.

You can then start Eclipse and import the project.
Check whether everything is running by running the included Main.scala as "Scala Application".

### Troubleshooting
- Should problems arise due to scala-incompatible jars on the build path, remove them using "configure build path" (for this project).

- When faced with a project that you want to make eclipse-usable using the above routine, go to the projects directory and open the buil-d.sbt in the root dir of the project.

```Bash
gedit build.sbt
```

Within the file, modify the value for `scalaVersion` like so:

```sbt
scalaVersion := "2.11.8"
```

Afterwards, (re-)run `sbt eclipse` in the root of the project.

### Misc

For a suggested `.gitignore`, see the attached file.


## Bootstrapping a NodeJS, Bootstrap and jQuery Project from Scratch

We need only a few steps to bring your web development environment up and running.
For super quick prototyping, we will use [lite-server](https://www.npmjs.com/package/light-server), a super lightweight cli-based http server that features livereload.

### Getting NodeJS and the Node Package manacher (npm)

If your on a Windows machine, then you should head over to https://www.debian.org/ or http://ubuntu.com/ and install a proper operating system. Just kidding. This tutorial assumes you are using either a Linux machine or a Mac, so we have access to a built in package-manager.

Depending on your package manager, installing NodeJS and npm is achieved simply by updating your local packages and fetching both bundles using only one line each. On Ubuntu 18.04 it's:

```Bash
sudo apt update
sudo apt-get install nodejs
sudo apt-get install npm
```
You can check your NodeJS versions using:
```Bash
nodejs -v
```
And there you go. NodeJS and npm are ready.

### Creating a bare bone project and deploying it on lite-server

Fire up a shell, and create a directory at your point of convenience, for example:

```Bash
mkdir ~/nodejs-demo
```
`cd` into the directory and (for simplicity) create an "emtpy" html-file by creating an index.html (f.e. by typing `gedit index.html`) and pasting the following minimal html code into the file.

```html
<!DOCTYPE html>
<html lang="en">
 <head>
   <meta charset="utf-8">
   <title>Hello world</title>
 </head>
 <body>
  <h2>This is awesome</h2>
 </body>
</html>
```
Now, while still within the `nodejs-demo` directory, we can initialize NodeJS using npm:
```Bash
npm init
```
This will might take a little. During the process you will be asked several things, most of which you can just leave as default by hitting enter. At one point it will ask for the main entry point, there you will type in `index.html` (instead of the default `index.js` that is purposed).

After the initialization is finished, you will see a file with the name `package.json` in the directory. This file defines metadata of the project and the packages/dependencies that npm will download for your project.

Go ahead and open `package.json` in your preferred editor.
Add the following two lines to the `"scripts"`-section:

Before:
```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1"
}
```
After:
```json
"scripts": {
  "start":"npm run lite",
  "test": "echo \"Error: no test specified\" && exit 1",
  "lite":"lite-server"
}
```
We do this because We want our project to be deployed (on startup) on a local lite-server instance so we see instant results to code changes we make.

With the above modified `package.json` in place, we can fire up the installation process while simultaneously installing lite-server:
```Bash
npm install lite-server --save-dev
```
After that process is done, we can start the project:
```Bash
npm start
```
You can then open your browser and head over to http://localhost:3000/. Here you can see the `index.html` we created. But not only that; you edit the html file as you wish and save it - the server will instantly redeploy the changes and they will be visible in the browser.  

### Adding Bootstrap and jQuery for ze eye candy

Because we do want to add Bootstrap and jQuery as basic web frameworks, again within our `package.json`, below the `"devDependencies"` section, we add a `"dependencies"`-section like so:
```json
"devDependencies": {
  "lite-server": "^2.3.0"
},
"dependencies": {
  "bootstrap": "^4.0.0",
  "jquery": "^3.3.1",
  "popper.js": "^1.12.9"
}
```
After saving the file have to install these dependencies via npm:
```Bash
npm install bootstrap@4.0.0 --save
npm install jquery@3.3.1 popper.js@1.12.9 --save
```
All those dependencies will be installed into the `node_modules`-directory. If you use Git, make sure to add this directory to your `.gitignore`. Other people can then fetch the dependencies to their machines themselves (by virtue of the declarations in `package.json`) - and they will not clutter the Git-repo.

Now off course we want to use these dependencies, so we add them our `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
 <head>
   <title>Hello world</title>
   <!-- Required meta tags always come first -->
   <meta charset="utf-8">
   <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
   <meta http-equiv="x-ua-compatible" content="ie=edge">
   <!-- Bootstrap CSS -->
   <link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.min.css">
 </head>
 <body>
  <h2>This is awesome -> now powered by Bootstrap and jQuery</h2>

  <!-- jQuery first, then Popper.js, then Bootstrap JS. -->
  <script src="node_modules/jquery/dist/jquery.slim.min.js"></script>
  <script src="node_modules/popper.js/dist/umd/popper.min.js"></script>
  <script src="node_modules/bootstrap/dist/js/bootstrap.min.js"></script>
 </body>
</html>
```
And there you go! You kann kill the lite-server process if you haven't already done so, and rerun the application:
```Bash
npm start
```
Thats it.
You can now start developing your web application using Bootstrap and jQuery by adding code to your `index.html`. Saved changes will be instantly visible at http://localhost:3000/.
