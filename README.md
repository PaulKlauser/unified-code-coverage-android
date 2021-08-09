# Unified Code Coverage for Android

> Forked from tramalho's multi-module example here:
> [Multi Module Example](https://github.com/tramalho/unified-code-coverage-android/tree/mixed-languages-multi-module)

Fixes an issue found with including the jacoco task in every module that creates a race and non-deterministic failures
of the jacoco task.

The issue with including the task in every module is that when you run ./gradlew jacocoTestReport, all modules will be running their
own tests (fine) then their own jacoco task (fine), but each task will be trying to grab _all_ of the exec files for each module.

Sometimes this leads to one module's running of the task trying to read an exec file from another module as that
module is writing to it. This fails the task.

Not to mention, this approach will be duplicating the generation of the report, and only the last module to generate the report
will technically "own" the report, though each report _should_ be identical.

## AGP Version

Currently written with AGP 4.2.0 as there are some issues with AGP 7.0.0 creating coverage unit test coverage files
in conjunction with instrumented test coverage files described here: https://issuetracker.google.com/issues/195860510