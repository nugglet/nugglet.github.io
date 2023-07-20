---
title: 'Basics of Writing Unit Test Cases'
date: 2023-02-02
permalink: /posts/2023/02/basics_of_writing_unit_test_cases
tags:
  - SWE
  - testing
excerpt_separator: "<!--more-->"
---

Writing good unit test cases is an important part of any software endeavour. But what constitutes a good unit test? <!--more-->

## Manual Test Cases
Reasons to use: automation is too expensive, tests are too complex, better for more created/empathetic test approaches
Good test cases are easier to maintain, provides versatility and flexibility.

Unit testing can target: verifying functionality, preventing known bugs reoccurring, documenting code, app security (checking for vulnerabilities)

Contents of test cases:
- Think of non-specific scenarios that are easy to write and maintain
- Try to cover edge cases

How to write test cases:
- Keep it simple and consistent. Establishing a style guide for writing test cases may be useful for teams.
- Start with a  good test scenario. Define the test scope well.
- Be only as specific as necessary. Test cases that overfit to the problem are inflexible and difficult to maintain. 20% of your tests should cover 80% of your features.
- Know when to be precise. If the test case is too vague, it becomes ineffective. Depending on the test case's complexity, more details may be needed and it may take a few iterations to find the suitable accuracy. This is particularly so when handling sensitive data. Highly specific test cases can be used to prevent known bugs.
- Invest time to explore features to discover new test cases and issues. Try to experience the software from a user perspective.

```
Sample Test Case Format:
	Title: Check updating username with an empty username field
	Prerequisites:  Be logged in and be in the profile settings page
	Actions:
	    1. Scroll to update username form
	    2. Leave username field empty
	    3. Submit the form
	Expectation: Update failure message is returned
```


## Links and Resources
- [Writing unit tests with javascript mocha](https://snyk.io/blog/how-to-write-unit-test-in-javascript/)
- [Automated testing with Selenium](https://www.lambdatest.com/blog/how-to-write-test-scripts-in-selenium/)