# Test Result Plugin

Use the webapp.io test result plugin to view beautifully formatted JUnit XML reports.
The plugin makes use of JUnit XML test reports to gather information from your runs and generate detailed reports.
These reports can be viewed on the job details page.

## Getting Started
The Cypress plugin can be installed by adding the test result plugin widget to your pull requests in the `Pull Request Builder`
section or installed in the `Plugins` section of the `settings tab.

The test result plugin looks for generated JUnit XML files from `/webappio/junitXML` directory (absolute path) on the virtual machine.

Note that the plugin is only able to fetch files from running servers. If the server has stopped you will have to manually start it again.

## Basic JUnit XML Structure
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites>
   <testsuite name="JUnitXmlReporter" errors="0" tests="0" failures="0" time="0" timestamp="2013-05-24T10:23:58" />
   <testsuite name="JUnitXmlReporter.constructor" errors="0" skipped="1" tests="3" failures="1" time="0.006" timestamp="2013-05-24T10:23:58">
      <properties>
         <property name="java.vendor" value="Sun Microsystems Inc." />
         <property name="compiler.debug" value="on" />
         <property name="project.jdk.classpath" value="jdk.classpath.1.6" />
      </properties>
      <testcase classname="JUnitXmlReporter.constructor" name="should default path to an empty string" time="0.006">
         <failure message="test failure">Assertion failed</failure>
      </testcase>
      <testcase classname="JUnitXmlReporter.constructor" name="should default consolidate to true" time="0">
         <skipped />
      </testcase>
      <testcase classname="JUnitXmlReporter.constructor" name="should default useDotNotation to true" time="0" />
   </testsuite>
</testsuites>
```

Notice that a report can contain 1 or more test suite. Each test suite has a set of properties (recording environment information).   Each test suite also contains 1 or more test case and each test case will either contain a skipped, failure or error node if the test did not pass.  If the test case has passed, then it will not contain any nodes.

## XML Schema Definition
For more details of which attributes are valid for each node you can consult the XML Schema Definition for jenkins made by JUnit 5 team: [jenkins-junit.xsd](https://github.com/junit-team/junit5/blob/main/platform-tests/src/test/resources/jenkins-junit.xsd).
