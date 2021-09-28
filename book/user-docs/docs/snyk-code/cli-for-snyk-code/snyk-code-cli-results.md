# Snyk Code CLI results

This example runs a code test against a project we have set locally, `goof`:

```text
snyk code test goof
```

This produces results like this:

```text
Testing goof ...

 ✗ [Medium] Use of Hard-coded Credentials
     Path: typeorm-db.js, line 11
     Info: Do not hardcode credentials in code. Found hardcoded credential used in typeorm.createConnection.

 ✗ [Medium] Use of Hard-coded Credentials
     Path: typeorm-db.js, line 12
     Info: Do not hardcode passwords in code. Found hardcoded password used in typeorm.createConnection.

 ✗ [Medium] Cleartext Transmission of Sensitive Information
     Path: app.js, line 12
     Info: http (used in require) is an insecure protocol and should not be used in new code.

 ✗ [Medium] Allocation of Resources Without Limits or Throttling
     Path: routes/index.js, line 77
     Info: This endpoint handler performs a system command execution and does not use a rate-limiting mechanism. It may enable the attackers to perform Denial-of-service attacks. Consider using a rate-limiting middleware such as express-limit.

 ✗ [Medium] Allocation of Resources Without Limits or Throttling
     Path: routes/index.js, line 166
     Info: This endpoint handler performs a file system operation and does not use a rate-limiting mechanism. It may enable the attackers to perform Denial-of-service attacks. Consider using a rate-limiting middleware such as express-limit.

 ✗ [Medium] Allocation of Resources Without Limits or Throttling
     Path: routes/index.js, line 223
     Info: This endpoint handler performs a file system operation and does not use a rate-limiting mechanism. It may enable the attackers to perform Denial-of-service attacks. Consider using a rate-limiting middleware such as express-limit.

 ✗ [Medium] Information Exposure
     Path: app.js, line 27
     Info: Disable X-Powered-By header for your Express app (consider using Helmet middleware), because it exposes information about the used framework to potential attackers.

 ✗ [High] Command Injection
     Path: routes/index.js, line 86
     Info: Unsanitized input from the HTTP request body flows into child_process.exec, where it is used to build a shell command. This may result in a Command Injection vulnerability.

 ✗ [High] SQL Injection
     Path: routes/index.js, line 39
     Info: Unsanitized input from the HTTP request body flows into find, where it is used in an SQL query. This may result in an SQL Injection vulnerability.

 ✗ [High] Cross-site Scripting (XSS)
     Path: routes/index.js, line 109
     Info: Unsanitized input from the HTTP request body flows into send, where it is used to render an HTML page returned to the user. This may result in a Cross-Site Scripting attack (XSS).

 ✗ [High] Hardcoded Secret
     Path: app.js, line 73
     Info: Avoid hardcoding values that are meant to be secret. Found a hardcoded string used in here.

✔ Test completed

Organization:      free-plan-org
Test type:         Static code analysis
Project path:      goof

11 Code issues found
4 [High]  7 [Medium]
```

### Results review

Here we can see that our snyk code testing found 11 issues, 4 high and 7 medium.

The issues are ordered by severity where the highest ones are at the bottom of our findings.

Each item includes:

* The name of the issue
* Path: the file and line in the file where the issue was found
* Info: Information about the issue

CLI for Snyk Code uses the exit codes as described in the [CLI reference](../../snyk-cli/guides-for-our-cli/cli-reference.md#EXIT-CODES).

