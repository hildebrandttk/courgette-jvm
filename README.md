
[![Build Status](https://travis-ci.org/prashant-ramcharan/courgette-jvm.svg?branch=master)](https://travis-ci.org/prashant-ramcharan/courgette-jvm)
[ ![Download](https://api.bintray.com/packages/prashantr/Courgette-JVM/courgette-jvm/images/download.svg) ](https://bintray.com/prashantr/Courgette-JVM/courgette-jvm/_latestVersion)

# Courgette-JVM #

Courgette-JVM is an extension of Cucumber-JVM with added capabilities to **run cucumber features files in parallel** and to **automatically re-run failed scenarios**.

## Key Features
- **All feature files** are executed in parallel on independent threads.
- **Automatic re-run** of failed scenarios.
- **Requires only 1 annotated class** to run all feature files in parallel.
- **Single report generation** for all executed features including embedded files (Json and Html reports)
- **Single re-run file** listing all failed scenarios that occured during parallel execution.
- Can be used with **Gradle** or **Maven**.
- Includes a clear and concise **Courgette-JVM Execution Report**.

## Requirements
- Java 8

## Installation

#### Repository: [jcenter](https://bintray.com/bintray/jcenter?filterByPkgName=courgette-jvm)

<a href='https://bintray.com/prashantr/Courgette-JVM/courgette-jvm?source=watch' alt='Get automatic notifications about new "courgette-jvm" versions'><img src='https://www.bintray.com/docs/images/bintray_badge_color.png'></a>

#### Maven
````markdown
<dependency>
  <groupId>io.github.prashant-ramcharan</groupId>
  <artifactId>courgette-jvm</artifactId>
  <version>1.2.0</version>
  <type>pom</type>
</dependency>
````

#### Gradle
````markdown
compile 'io.github.prashant-ramcharan:courgette-jvm:1.2.0'
````

#### Included Dependencies
* cucumber-core 1.2.5
* cucumber-java 1.2.5
* cucumber-java8 1.2.5
* cucumber-junit 1.2.5


## Usage
Courgette-JVM uses JUnit to run cucumber features. A runner class must be annotated with **@RunWith(Courgette.class)** and must include **@CourgetteOptions**

* **threads** : The number of concurrent threads to run cucumber features. 

    * _Example: If you have 10 cucumber features and you use 6 threads, 6 features would first run in parallel then the following 4 features would run in parallel_.

    
* **rerunFailedScenarios** : If set to true, any failed scenario will be immediately re-run in the same thread. If the re-run succeeds, the initial failure will be ignored and not cause the build to fail.

* **showTestOutput** : If set to true, the output for each feature will be redirected to the current I/O source or destination.
    
* **cucumberOptions** : The standard cucumber options for specifying feature paths, glue, tags etc..

### Additional

* At the end of the test run, a **single report** ( _if included in the cucumberOptions_ ) listing all executed features and scenarios will be created in the specified report path. All embedded images will be placed in the images folder in the specified report path.

* A **courgette-rerun.txt** file listing all failed scenarios will be created in the specified rerun plugin path or the target folder ( _default_ )

* A Courgette-JVM execution report will be created in the target folder.

````java
@RunWith(Courgette.class)
@CourgetteOptions(
        threads = 10,
        rerunFailedScenarios = true,
        showTestOutput = true,
        cucumberOptions = @CucumberOptions(
                features = "src/test/resources/features",
                glue = "steps",
                tags = {"@regression"},
                plugin = {
                        "pretty",
                        "json:target/courgette-report/courgette.json",
                        "html:target/courgette-report/courgette.html"}
        ))
public class RegressionTestSuite {
}
````

## Build Task

Gradle
````
gradle -Dtest.single=RegressionTestSuite test
````

Maven
````
mvn -Dtest=RegressionTestSuite test
````

## Limitations

* JUnit test notifier is not updated when running features in the IDE during parallel test execution.
   * _Each feature is run using the Cucumber CLI and because of this JUnit is not notified off the test result. The workaround to this is the Courgette-JVM Execution report which lists all test passes, failures and re-runs. Alternatively, you can review the Cucumber reports or the results from the build tool_.


## Submitting Issues
For any issues or requests, please submit [here](https://github.com/prashant-ramcharan/courgette-jvm/issues/new)
