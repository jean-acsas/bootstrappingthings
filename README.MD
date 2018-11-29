# Scala and Eclipse-Scala-IDE installation for Linux 

Painless install routine for scala 2.11.8, sbt 13.11, and eclipse ide. (Tested on Ubuntu 18.04 LTE.)
* Bonus: suggested .gitignore file * 

### Prerequisites 

First make sure you've installed an appropriate **Java _8_ SDK** (e.g. OpenJDK) by opening a terminal and running 

```
java -version
```

should you have multiple JDKs on your system, you can set the prefered one via 

```
update-java-alternatives --set
```

### Install Scala 2.11.8

```
wget www.scala-lang.org/files/archive/scala-2.11.8.deb
sudo dpkg -i scala-2.11.8.deb
```

### Install sbt 13.11: 

```
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823
sudo apt-get update
sudo apt-get install sbt
```

### Install Eclipse Scala-IDE and setup a project

Get a fresh Eclipse-IDE, one **that suits your Scala version (here 2.11.8)** (see http://scala-ide.org/download/prev-stable.html).

*Tip: create a dedicated Eclipse workspace for your Scala projects.*

To create a new Scala project, navigate to that workspace and run 

```
sbt new scala/hello-world.g8
```

Enter a new for the project upon request.

To make the project Eclipse-usable, enter the directory and run:

```
echo 'addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "5.2.4")' > project/plugins.sbt
sbt eclipse
```

This might take a little time. 

You can then start Eclipse and import the project. 
Check whether everything is running by running the included Main.scala as "Scala Application".

### Troubleshooting
- Should problems arise due to scala-incompatible jars on the build path, remove them using "configure build path" (for this project).

- When faced with a project that you want to make eclipse-usable using the above routine, go to the projects directory and open the buil-d.sbt in the root dir of the project.

```
gedit build.sbt
```

Within the file, modify the value for `scalaVersion` like so:

```
scalaVersion := "2.11.8"
```

Afterwards, (re-)run `sbt eclipse` in the root of the project.

### Misc

For a suggested `.gitignore`, see the attached file.
