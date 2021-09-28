# SDK Reference

### NAME

`snyk-iac-rules` - SDK to write, debug, test, and bundle custom rules for Snyk IaC

### SYNOPSIS

`snyk-iac-rules` \[COMMAND\] \[PATH\] \[OPTIONS\]

### DESCRIPTION

The SDK helps you write, debug, test, and bundle custom rules written in Rego, which can then be used by the Snyk IaC CLI to find vulnerabilities in IaC configuration files.

For more information on Snyk, see [https://snyk.io](https://snyk.io)

#### Not sure where to start?

1. Start writing a new rule with `$ snyk-iac-rules template`
2. To start writing Rego, parse fixtures into JSON with `$ snyk-iac-rules parse`
3. Test your rule with `$ snyk-iac-rules test`
4. Bundle your rules with `$ snyk-iac-rules build`

### COMMANDS

To see command-specific flags and usage, see `help` command, e.g. `snyk-iac-rules --help`.

Available top-level SDK commands:

`template` Generate the scaffolding for writing new rules. See `snyk-iac-rules template --help` for full instructions.

`parse` Return the JSON format of the provided fixture file. Rego requires JSON input so use this to build your Rego rules. See `snyk-iac-rules parse --help` for full instructions.

`test` Execute all test cases discovered in matching files. See `snyk-iac-rules test --help` for full instructions.

`build` Package OPA policy and data files into a bundle. See `snyk-iac-rules build --help` for full instructions.

### PATH

All the commands in the SDK can take an optional path to a folder, which dictates where the rules would be located. The `parse` command is the only command where this path is strictly required.

Any path can be provided - relative or absolute.

### OPTIONS

To see command-specific flags and usage, see the help command, e.g. `snyk-iac-rules template --help`.

#### Template options

`--rule`=RULE

The name of the rule you want to define. This will generate a `rules/` folder at the top-level, which will contain a folder named after the rule and a Rego rule and Rego test file. At the same time, it will generate a `lib/` folder, containing utility functions that can be accessed from `data.lib` for writing rules and extending the testing library, or `data.lib.testing` for the tests.

The scaffolded folder structure looks like so:

`rules   
└── RULE      
        ├── fixtures   
                ├── allowed.tf   
                └── denied.tf  
        ├── main.rego   
        └── main_test.rego  
lib      
└── testing   
        └── main.rego   
└── main.rego`

Note: the rule name cannot contain any whitespace or start with `SNYK-`.

#### Parse options

`--format`=`hcl2`\|`yaml`

The format of the fixture. The format dictates what parser we use to generate JSON from the fixture.

Default: `hcl2`

#### Test options

`--verbose`

Output tracing logs.  


`--explain`=`full`\|`notes`\|`fails`

Filter tracing logs.

Default: `fails`  


`--timeout`=TIMEOUT

The amount of time \(in nanoseconds\) to wait for the tests to run. If it takes longer than TIMEOUT, the tests fail.

Default: 5000000 \(5 seconds\).

`--ignore`

Accepts a regular expression that can be used to ignore files and folders from being loaded for testing. 

Default: ".\*" \(hidden files\), "fixtures"  


`--run`

Accepts a regular expression that can be used for running a subset of the tests.

#### Build options

`--output`

The name and location of the resulting bundle.

Default: bundle.tar.gz  


`--entrypoint`

By default, the Template command places the Rego for the rules under a `./rules/<rule>` folder. The Rego belongs to the rules package and the entry function is called deny, returning JSON representing a vulnerability if a misconfiguration was found. This structure works with the `rules/deny` entrypoint, but a custom entrypoint can be provided if the generated file and package structure needs to be modified.

Default: "rules/deny"  


`--ignore`

Accepts a regular expression that can be used to ignore files and folders from being loaded for bundling. 

Default: ".\*”"\(hidden files\), "fixtures", "testing", "\*\_test.rego"  


`--target`=`rego`\|`wasm`

What format to use for the bundle. We don’t support Rego bundles for now in the Snyk IaC CLI.

Default: `wasm`

#### Flags available across all commands

\[COMMAND\] `--help`, `--help` \[COMMAND\], `-h`

Prints a help text. You may specify a COMMAND to get more details.

### EXAMPLES

Generate a new rule with the name `CUSTOM_RULE`

```text
$ snyk-iac-rules template --rule CUSTOMRULE
```

Test the rule

```text
$ snyk-iac-rules test --run CUSTOM_RULE
```

Generate the custom rules bundle

```text
$ snyk-iac-rules build --output bundle-v1.0.0.tar.gz
```

### EXIT CODES

Possible exit codes and their meaning:

**0**: success

**1**: failure, the custom rule might be invalid  


