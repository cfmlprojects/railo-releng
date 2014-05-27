#USAGE

Download the zipball and run the batch or sh script.  Select "build" in the menu.

Or from the command line:

./railo-releng build

This will download cfdistro to your home directory.  The build aritfacts will be 
stored in "${user.home}/cfdistro/artifacts" by default.

##BUILDING

Some notes about building:

	Building *will* take a while, the first time, as it pulls the  Git repo.
	Subsequent builds are fast as fast can be ish!  If you know your system has
	git installed, the argument "use.jgit=false" will speed the clone up.
	
	You can specify the Git repo to pull from by editing the build/src.xml file
	or passing it on the commandline (sc.railo.uri=http://myrepo/dot.git).
	
	As the Railo repo is imported into the ./src directory, any git operations
	for the sources should be done there.

	Different versions of Railo require different Java versions to compile. If 
	you try to build with the wrong version of Java, the build will fail and 
	tell you what the correct version to use is.  If you are on linux/OS X, 
	you can pass in the location of JAVA_HOME via build.java.home
	
	ex: ./railo-releng build branch=3.2.1.006 build.java.home=/path/to/java_home
	
	On Windows, if you set JAVA_HOME via command line, do not quote the path 
	
	ex: set JAVA_HOME=C:\Program Files\Java\jre6
	
	Using the "branch" or "commit" build options will do a *forced* checkout of
	the corresponding sources.  If you have any changes, be sure to commit them
	(preferably to a different branch) them prior!
	
	Running a plain "build", without the branch or commit options, will only run
	the build on the current sources.  Modified non-committed stuff is safe.
	
	The sources are compiled with the Eclipse ECJ compiler, so no JDK is needed.
	
	You will see a commit log after each build.  If you update your sources, the
	log will be of the changes since you last built the sources.  The last commit
	hash will be stored in buildinfo.properties, which you can then use for CI
	checks (i.e., if lastbuild.hash != current.hash, something changed).

Build the default (4.2) branch

 ./railo-build build 

Build the "develop" branch (5.0) leveraging system git

 ./railo-build build branch=develop use.jgit=false
 
Build a tag (uses the branch argument but specify the tag)

 ./railo-build build branch=4.2.1.000
 
Build a random commit 
 
 ./railo-build build commit=81a8897d1b9e273f8de819eb56a13d7bcf5d51cf

## MAVEN TEMPLATES

Maven templates for the POMs (including jetty, heroku, etc.) are here:

	./build/resource/maven/

##TESTING

Build and run the jira tests:
 ./railo-releng build.and.test

Run the jira tests (starts server, runs tests, stops server):
 ./railo-releng test

###LOAD TESTS:  JMETER

	Currently there is only one JMeter test, which goes through the web admin a 
	few times.

Downlaod and install JMeter (defaults to ./build/jmeter-{version}) by entering: 
 ./railo-releng jmeter.install

Build and run the jmeter tests:
 ./railo-releng build.and.loadtest

Run the jmeter tests (this will run jmeter.run and jmeter.report):
 ./railo-releng loadtest

Run the jmeter tests alone (no report generation):
 ./railo-releng jmeter.run

Run the jmeter report generation (no tests are run):
 ./railo-releng jmeter.report

Clean the jmeter test results and reports:
 ./railo-releng jmeter.clean

To run the test server by hand in the foreground (does not background):
 ./railo-releng server.start.fg

Starting the test server by hand (goes to background):
 ./railo-releng server.start

Stopping the test server by hand:
 ./railo-releng server.stop

Editing JMeter tests:
 ./railo-releng server.start
 ./build/jmeter-2.6.1/bin/jmeter.sh

do yer editing or whatever, then
 ./railo-releng server.stop

#PACKAGING

##WAR
	A WAR file is generated as part of the standard build, as is the railo.jar,
	and core.rc file.


##EXPRESS VERSION

 to build the coolest version of express, do 
 
 ./railo-build Railo.express
 
 To build the "normal" version of express, do

 ./railo-build Railo.express.jetty

 To build the winstone version of express, do

 ./railo-build Railo.express.winstone


##MINI VERSION

This build is bare -- no ORM, no PDF stuff, nada. -- and thus small:

 ./railo-build Railo.mini
