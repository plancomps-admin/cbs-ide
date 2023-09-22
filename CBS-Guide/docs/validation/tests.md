---
title: Tests
parent: Validation
--- 
# Tests

There are two types of tests that can be used for a language written with CBS:

- The tests is with the interpreter, useful when running the funcon terms obtained and to compare the expected and actual outcomes. 
- The SPoofax Test ones, useful when something goes wrong during parsing or transformation and running is just not possible because there aren't funcon terms at that time.

## Testing funcon terms 

The interpreter `runfct` can read options both from comand line and configuration files. 
By default when a `.fct` file is given to `runfct` it searches for a file with the same name and a `.config` extension. 

For example, `let.fct` is:
  ```
  let m = 42 in
  let n = m in
  n
  ```
The corresponding `let.config` file:
  ```
  tests {
    result-term: 42;
  }
  ```
 Once running `runfct let.fct`, an empty output indicates the test ran succesfully, otherwise it will contain both the expected and the actual results.

In order to run the terms without the configuration use `--auto-config false` as argument of `runfct`. 
Read about all the options available for the interpreter at
[Funcons-Tools](https://hackage.haskell.org/package/funcons-tools-0.2.0.15/docs/Funcons-Tools.html) in the documentation of `funcons-tools` package. 

Inputs can also be provided in configuration files fro simulation, but the next small correction is needed. Use parenthesis to group the values as a sequence instead of the brackets for a list:

  - `input.config`
  ```
  inputs {
    standard-input: (1,2,3);
  }
  ```

## Testing previous steps using SPT

Because the derived editors from CBS are built each one as a Spoofax project, 
Its possible to use the SPoofax Testing Language (SPT) for test translation and intermediate steps.
This gives the oportunity to check intermediate steps and also keep track of rejected fragments. 

The tests have the following sintax:

`test ::= "test" name "[[" code "]]" expectation`

The code fragment can be enclosed in two to four markers (or brackets), `[[` and `]]` are the mostly used.

Expectations tells what action is going to be performed and what is the expected outcome. They fall in five categories but those that are useful for CBS derived languages are: Parse and Transformation Expectations. 

Detailed descriptions of all test expectations are available at [SPT reference: Test expectations](https://spoofax.dev/references/spt/test-expectations/#test-expectations) in Spoofax's new documentation, test suites and execution of the tests are covered in adyacent sections.

Metaborg's [SPT repository](https://github.com/metaborg/spt) contains a complete especification of the language under `org.metaborg.meta.lang.spt` package.

The tests need to be in a SPT test suite (also called module) as showed below in the `CD-tests.spt` suite with a few examples:

- `CD-tests.spt`
  ```
  // Head of test suite
  module CD-tests
  language CD
  start symbol Start

  // Code that parses succesfully:
  test identity [[
    let f = lambda x. x in f 42
  ]] parse succeeds

  // Rejected fragment, fails at parsing:
  test sum fails [[
    let a = 3 + 2
  ]] parse fails

  // Also rejected, a parse can be partial:  
  test partial parse [[
    let 3
  ]] parse to FCTApp("let", FCTInt ("3"))
  // FCTApp and FCTInt are ATerms that cannot be expanded further.
  
  // Transform to funcons, as with `Generate Funcons` strategy used in `editor/Main.esv`:
  test integer [[
    42
  ]] transform "Generate Funcons" to 
  "initialise-binding finalise-failing decimal
    \"42\""
  // Just a example, not recommended to avoid duplicity
  ```
  {:.note}
  > The last result here is a simple string with scaped quotes in place. This expectation appears to not be in the SPT reference but actually since the string is counted as a single ATerm, it is. 

The next is desired but not available yet due to minor differences between SPT and SDF3 syntax regarding ATerm names, is expected to be fixed soon.

- `AST-tests.spt`
  ```
  // Parse to an ATerm, such as the AST generated with `Show parsed AST`:
  test integer to aterm [[
    42
  ]] parse to L-start--L-exp(L-exp--L-int(LEX-int("42")))
  ```
### Running SPT tests
Inside of the Spoofax IDE the options to run SPT tests are in `Spoofax(meta)` menu as: 

- `Run selected tests` for just one suite,
- `Run all selected tests` for all test suites in the selected project.

The failed test are highlighted in the suite and in the report of SPT Test Runner.  
The tests will continue to run unless the editor analysis is disabled. They also run when the test project is being built.

### Run using the command line

To run via command line [Run SPT test: using the command line](https://spoofax.dev/references/spt/running-tests/#run-using-the-command-line-runner) guide indicates the next requirements:
  
* SPT.cmd, the latest development version has to be built (from master branch, currently 2.6.0-SNAPSHOT) even if previous versions can be downloaded from the release notes. The reason being before [Remove Stratego interpreter usage](https://github.com/metaborg/spt/commit/834bba11f65d6468f99e048f30de009c5c5e606a) fix, it depends on the  `Stratego ctree` file instead of the proper `jar` package leaving the following steps with no effect.

* SPT and Stratego built language packages, with the maven configuration from [Spoofax development Maven configuration](https://spoofax.dev/howtos/development/setup-maven-for-spoofax-dev/.#how-to-setup-maven-for-spoofax-development)

Having all the requirements, the command is like below:
  ```
  java -jar spt/org.metaborg.spt.cmd/target/org.metaborg.spt.cmd-2.6.0-SNAPSHOT.jar \ 
  --lut   path/to/My-Languages/CBS-Demo/CD-Editor \
  --tests path/to/My-Languages/CBS-Demo/CD-Tests/CD-Tests.spt \ 
  --spt   spt/org.metaborg.meta.lang.spt 
  --lang stratego/org.metaborg.meta.lang.stratego
  ```
It can also run all the test suites using  `--tests path/to/My-Languages/CBS-Demo/CD-Tests` instead.

The output for `CD-Tests.spt`looks like:
```
13:38:28.988 INFO  o.m.c.t.LoggingTestReporterService - running tests
13:38:33.275 INFO  o.m.c.t.LoggingTestReporterService - running test suite CD-tests
*************************************************************************************
[INFO]  - t.core.run.SpoofaxTestCaseRunner | Evaluating Test: Tests.spt/. - identity 
13:38:33.409 INFO  o.m.c.t.LoggingTestReporterService - test identity ... ok
**************************************************************************************
[INFO]  - t.core.run.SpoofaxTestCaseRunner | Evaluating Test: Tests.spt/. - sum fails 
13:38:33.471 INFO  o.m.c.t.LoggingTestReporterService - test sum fails ... ok
******************************************************************************************
[INFO]  - t.core.run.SpoofaxTestCaseRunner | Evaluating Test: Tests.spt/. - partial parse 
13:38:33.476 INFO  o.m.c.t.LoggingTestReporterService - test partial parse ... ok
************************************************************************************
[INFO]  - t.core.run.SpoofaxTestCaseRunner | Evaluating Test: Tests.spt/. - integer 
[DEBUG] - raint.AbstractConstraintAnalyzer | . errors: []
[DEBUG] - raint.AbstractConstraintAnalyzer | collected messages: {}
[DEBUG] - etaborg.util.collection.Multimap | get(file:///path/to/My-Languages/CBS-Demo/CD-Tests/Tests.spt) = []; hash = -1110077119
[DEBUG] - raint.AbstractConstraintAnalyzer | result: file:///path/to/My-Languages/CBS-Demo/CD-Tests/Tests.spt; messages: []
[INFO]  - ext.constraint.ConstraintContext | put entry: file:///path/to/My-Languages/CBS-Demo/CD-Tests/Tests.spt; errors: []
13:38:34.774 INFO  stderr - .
13:38:34.831 INFO  o.m.c.t.LoggingTestReporterService - test integer ... ok
13:38:34.831 INFO  o.m.c.t.LoggingTestReporterService - test suite result: ok. 4 passed; 0 failed; 0 ignored
13:38:34.831 INFO  o.m.c.t.LoggingTestReporterService - test result: ok. 4 passed; 0 failed; 0 ignored
```