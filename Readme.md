# Cucumber Extent Reporter

This tool helps you to generate the custom cucumber-jvm report using ExtentReports plugin.

The [ExtentReports](http://extentreports.relevantcodes.com/) plugin is developed by Anshoo Arora. This is one of the best reporting plugin available for testing world. This plugin can be used with any Test Apis.

## Build Status
[![Build Status](https://travis-ci.org/email2vimalraj/CucumberExtentReporter.svg?branch=master)](https://travis-ci.org/email2vimalraj/CucumberExtentReporter)

<a target='_blank' rel='nofollow' href='https://app.codesponsor.io/link/XhfFiVtGMXhfiynNYZxdsCvP/email2vimalraj/CucumberExtentReporter'>
  <img alt='Sponsor' width='888' height='68' src='https://app.codesponsor.io/embed/XhfFiVtGMXhfiynNYZxdsCvP/email2vimalraj/CucumberExtentReporter.svg' />
</a>

## Usage
If you are using a maven based project, you can directly add this library as a dependency:

```
<dependency>
    <groupId>com.vimalselvam</groupId>
    <artifactId>cucumber-extentsreport</artifactId>
    <version>3.0.1</version>
</dependency>
```

Please note that **Java 8+** and adding the dependency of **ExtentReport v3.0.6+** is mandatory.

If you are not using maven, download the jar from [here](http://search.maven.org/#search%7Cga%7C1%7Ccucumber-extentsreport).

## Release Notes
For more details, look at [Changelog](Changelog.md).

## Getting Started

### Runner Class example:
Create a runner class and add the `com.cucumber.listener.ExtentCucumberFormatter:output/report.html` as a plugin followed by the report file as input (**optional parameter**).

A sample example is shown below:

```java
package com.cucumber.runner;

import com.cucumber.listener.Reporter;
import cucumber.api.CucumberOptions;
import cucumber.api.junit.Cucumber;
import org.junit.AfterClass;
import org.junit.runner.RunWith;

import java.io.File;

/**
 * A sample test to demonstrate
 */
@RunWith(Cucumber.class)
@CucumberOptions(
    features = {"src/test/resources/features"},
    glue = {"com.cucumber.stepdefinitions"},
    plugin = {"com.cucumber.listener.ExtentCucumberFormatter:output/report.html"}
)
public class RunCukesTest {
    @AfterClass
    public static void teardown() {
        Reporter.loadXMLConfig(new File("src/test/resources/extent-config.xml"));
        Reporter.setSystemInfo("user", System.getProperty("user.name"));
        Reporter.setSystemInfo("os", "Mac OSX");
        Reporter.setTestRunnerOutput("Sample test runner output message");
    }
}

```

The above setup will generate the report in `output` directory with the name of `report.html`. There are 3 ways to configure the report location:

- As shown above, pass the report file path along with `com.cucumber.listener.ExtentCucumberFormatter:`
- If in case you want a dynamic location, you can leave the file path parameter empty while configuring the plugin. For example:
    `plugin = {"com.cucumber.listener.ExtentCucumberFormatter:"}`
    This will generate the report file in the location `output/Run_<Current Time Stamp>/report.html`.
- You can also configure the report location by using `ExtentProperties` enum as follows. Leave the plugin configuration empty, and configure the report location in your `@BeforeClass` method:
    ```java
    plugin = {"com.cucumber.listener.ExtentCucumberFormatter:"}
    ......
    ......
    @BeforeClass
    public static void setup() {
        ExtentProperties extentProperties = ExtentProperties.INSTANCE;
        extentProperties.setReportPath("output/myreport.html");
    }
    ```

The above example shows a JUnit runner. However, you can use the TestNG runner too. Refer more examples [here](https://github.com/email2vimalraj/CucumberExtentReporter/tree/master/src/test/java/com/cucumber/runner). 
Also make sure the `loadXMLConfig`, `setSystemInfo` and `setTestRunnerOutput` methods should be in your `@AfterClass` method.

### Setting up ExtentX
The current release added a support for `ExtentX`. The results of your execution can be sent to `ExtentX` dashboard.

The `ExtentX` configurations can be set up using `ExtentProperties` enum as follows:

```java
plugin = {"com.cucumber.listener.ExtentCucumberFormatter:"}
......
......
@BeforeClass
public static void setup() {
    ExtentProperties extentProperties = ExtentProperties.INSTANCE;
    extentProperties.setReportPath("output/myreport.html");
    extentProperties.setExtentXServerUrl("http://localhost:1337");
    extentProperties.setProjectName("MyProject");
}
```

You can either setup ExtentX in your local / remote machine using:
- [A docker approach - 2 minutes to setup](http://www.vimalselvam.com/2017/04/13/shipping-extentx-in-a-docker-container/)
- [A plain vanilla approach](http://extentreports.com/docs/extentx/)

### Logging
User can add logs at any step and those logs will be captured and attached to the corresponding step. The log should be added as follows:

```java
Reporter.addStepLog("Step Log message goes here");
```

In case any log to be added at the scenario level, the following can be done:

```java
Reporter.addScenarioLog("Scenario Log message goes here");
```

### Adding screenshot / screen cast
The screenshot or screen cast can be added from any of the step as follows. Please note that the plugin will not take the screenshot, instead it helps you to attach the screenshot file which should be already available in the mentioned path. If you are looking for on how to take the screenshot using Selenium, please refer [this](http://www.seleniumeasy.com/selenium-tutorials/take-screenshot-with-selenium-webdriver):

```java
Reporter.addScreenCaptureFromPath("absolute screenshot path");
Reporter.addScreenCastFromPath("absolute screen cast path");
```

### Assigning Authors to the Scenario
You can assign authors to the scenario using `Reporter.assignAuthor("author1", "author2", ...);`. 

Ideally this should be added at the hooks level. For example, the following sample shows how to assign author to a particular scenario using `Before` hook:

```java
@Before
public void beforeScenario(Scenario scenario) {
    if (scenario.getName().equals("Some scenario name")) {
        Reporter.assignAuthor("Author1");
    }
}
```

## Demo
[Report](http://vimalselvam.com/cucumberextentreport/report.html)

Fore more details, kindly visit [http://www.vimalselvam.com/cucumber-extent-reporter/](http://www.vimalselvam.com/cucumber-extent-reporter/).
