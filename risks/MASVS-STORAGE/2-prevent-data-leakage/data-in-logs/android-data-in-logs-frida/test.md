---
platform: android
title: Leakage of Sensitive Data via Logging APIs
apis: [Log, Logger, System.out.print, System.err.print, java.lang.Throwable#printStackTrace]
type: [dynamic]
---

## Overview

On Android platforms, logging APIs like `Log`, `Logger`, `System.out.print`, `System.err.print`, and `java.lang.Throwable#printStackTrace` can inadvertently lead to the leakage of sensitive information. Log messages are recorded in logcat, a shared memory buffer, accessible since Android 4.1 (API level 16) only to privileged system applications that declare the `READ_LOGS` permission. Nonetheless, the vast ecosystem of Android devices includes pre-loaded apps with the `READ_LOGS` privilege, increasing the risk of sensitive data exposure. Therefore, direct logging to logcat is generally advised against due to its susceptibility to data leaks.

## Steps

1. Install and run the app.

2. Navigate to the screen of the mobile app you want to analyse the log output from.

3. Execute a [method trace](https://mas.owasp.org/MASTG/techniques/android/MASTG-TECH-00xx/) by attaching to the running app, targeting logging APIs and save the output.

## Observation

The **method trace output** contains a list of locations where logging APIs are used in the app for the current execution.

## Evaluation

The test case fails if you can find sensitive data being logged using those APIs.

For example, the following output leaks a key via `Log`:

```shell
Log.println_native(0, 4, "tag", "key: 12345678")
```
