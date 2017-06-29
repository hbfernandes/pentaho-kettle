# Pentaho Data Integration #

Pentaho Data Integration ( ETL ) a.k.a Kettle

### Project Structure

* **[assemblies:](assemblies)** 
Project distribution archives
* **[core:](core)** 
Core implementation
* **[dbdialog:](dbdialog)** 
Database dialog
* **[ui:](ui)** 
User interface
* **[engine:](engine)** 
Engine implementation
* **[engine-ext:](engine-ext)** 
Engine extensions
* **[plugins:](plugins)**
Core plugins
* **[integration:](integration)** 
Integration tests

### Pre-requisites for building the project:
* Maven, version 3+
* Java JDK 1.8
* This [settings.xml](https://github.com/pentaho/maven-parent-poms/blob/master/maven-support-files/settings.xml) 
in your <user-home>/.m2 directory

### Building it

__Build for nightly/release__

All required profiles are activated by the presence of a property named "release".

```
$ mvn clean install -Drelease
```

This will build, unit test, and package the whole project (all of the sub-modules). 
The artifact will be generated in the `target` folders of each module. 

The main distributable archive can currently be found on `assemblies/pdi-ce/target` after a build as been completed.

__Build for CI/dev__

The `release` builds will compile the source for production (meaning potential obfuscation and/or uglification). 
To build without that happening, just eliminate the `release` property.

```
$ mvn clean install
```

__Build a subset of modules__

Currently there are profiles that allow you to build only a part of the project.

* **base:**
Builds the main modules those being **engine**, **core**, **dbdialog**, **ui** and **pdi-extensions**.

```
mvn clean install -DskipDefault -Pbase
```

* **plugins:**
Builds the plugins. Plugins are divided between **highdeps** and **lowdeps** because of external project dependencies
that force us to build them on a specific order on the stack. That will be addressed in the future, but for now 
this division is necessary.

```
mvn clean install -DskipDefault -Pplugins,highdeps
```

```
mvn clean install -DskipDefault -Pplugins,lowdeps
```

* **assemblies:**
Packages the project into it's distributable form.

```
mvn clean install -DskipDefault -Passemblies
```

### Running the tests

__Unit tests__

This will run all tests in the project (and sub-modules).
```
$ mvn test
```

If you want to remote debug a single java unit test (default port is 5005):
```
$ cd core
$ mvn test -Dtest=<<YourTest>> -Dmaven.surefire.debug
```

__Integration tests__

In addition to the unit tests, there are integration tests in the core project.
```
$ mvn verify -DrunITs
```

To run a single integration test:
```
$ mvn verify -DrunITs -Dit.test=<<YourIT>>
```

To run a single integration test in debug mode (for remote debugging in an IDE) on the default port of 5005:
```
$ mvn verify -DrunITs -Dit.test=<<YourIT>> -Dmaven.failsafe.debug
```

### Contributing

1. Submit a pull request, referencing the relevant [Jira case](http://jira.pentaho.com/secure/Dashboard.jspa)
2. Attach a Git patch file to the relevant [Jira case](http://jira.pentaho.com/secure/Dashboard.jspa)

Use of the Pentaho checkstyle format (via `mvn site` and reviewing the report) and developing working 
Unit Tests helps to ensure that pull requests for bugs and improvements are processed quickly.

