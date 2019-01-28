## Code snippets for testing



1. System Under Test
Skip to end of metadata

    <System-under-test> tag is required in every test case.  It is an input of a test case. One of following tags are available inside <system-under-test>.  They are action to be performed on an input.  One and only one is allowed for each <system-under-test>:

    <data-type>: test data type of column

    <structure>: test structure of SSAS model

    <execution>: Executing a query

    <members>

    <resultSet>: Input as csv file



Following are option tag inside <system-under-test>:

    <query connectionString="...">: a specific connection that is will be used for this <system-under-test>.



Following tags are required:

      <testSuite>: Require once for all test in a suite
      <settings>: Connections; however, if <system-under-test> or <assert> can have its own connection string.

              <test name>: name of test case

              <system-under-test>: input to be executed or test to be validated.
              <assert>: Expected resultant to be validated.



Following tags are optional:

      <description>: description about the test

      <edition>: Version of the test

      <condition>: system condition must be satisfied for the test case to be executed.  For instance, SSIS service is running.

      <variables>: is a parameter that can be used for global in all test cases in the suite.
