# Snyk for Java \(Gradle, Maven\)

Snyk offers security scanning to test your projects for vulnerabilities, both through your CLI and through different integrations from our UI.

**Supported versions:** For officially supported Java versions, operating systems, and Node.js versions see the [Gradle support](https://github.com/snyk/snyk-gradle-plugin#support) and [Maven support](https://github.com/snyk/snyk-mvn-plugin#support) tables.

## Features

The following table provides a general outline of the general features we offer by language. In addition to these features, we also offer additional functionality related to the specific integrations you configure and more.

{% hint style="info" %}
Some features might not be available, depending on your pricing plan. See [pricing plans](https://snyk.io/plans/) for more details.
{% endhint %}

| Package managers / Features | CLI support | Git support | License scanning | Remediation | Runtime monitoring |
| :--- | :--- | :--- | :--- | :--- | :--- |
| [Maven](https://maven.apache.org/) | ✔︎ | ✔︎ | ✔︎ | ✔︎ | ✔︎ |
| [Gradle](https://gradle.org/) | ✔︎ | ✔︎ | ✔︎ | ✔︎ \(advice\) |  |

## Snyk CLI tool for Java projects \(CI/CD\)

The way Snyk analyzes and builds the dependencies varies depending on the language and package manager of the project.

Learn how to use the tool for your Java projects as follows:

* Snyk CLI with Gradle: To build the dependency graph, Snyk integrates with Gradle and inspects the dependencies returned by the build. The following manifest files are supported: `build.gradle` and `build.gradle.kts`
* Snyk CLI with Maven: To build the dependency tree, Snyk analyzes the output of the `pom.xml` files.

## CLI parameters for Java

This section describes the unique CLI commands available when working with Java-based projects as follows:

### Prerequisites

* Install the relevant package manager before you use the Snyk CLI tool.
* Include the relevant manifest files supported by Snyk before testing.
* Install and authenticate the Snyk CLI to start analyzing projects from your local environment. See [Getting started with the CLI](https://docs.snyk.io/snyk-cli/guides-for-our-cli/getting-started-with-the-cli).

### Snyk CLI parameters

When working with Gradle projects from our CLI, you can add any of the following options to further refine the way the scan works:

| **Option** | **Description** |
| :--- | :--- |
| `--sub-project=` | For Gradle "multi-project" configurations, test a specific sub-project. |
| `--all-sub-projects` | For "multi-project" configurations, test all sub-projects. |
| `--configuration-matching=` | Resolve dependencies using only configuration\(s\) that match the provided Java regular expression. For example: `'^releaseRuntimeClasspath$'` |
| `--configuration-attributes=` | Select certain values of configuration attributes to resolve the dependencies. For example: `'buildtype:release,usage:java-runtime'` |
| `--all-projects` | Use for monorepos. This will detect all supported manifests. For Gradle monorepos Snyk will only look for root level **build.gradle / build.gradle.kts** files and apply the same logic as `--all-sub-projects` behind the scenes. This command is designed to be run in the root of your monorepo. |

### Gradle sub-projects

Gradle build can consist of several sub-projects, where each sub-project has its own build.gradle, while the root project is the only one that also includes a `settings.gradle` file. Sub-projects depend on the root project, but can be configured otherwise.

By default, Snyk CLI scans only the current project \(the project in the root of the current folder\), or the project that is specified by `--file=path/to/build.gradle`\).

* To scan all projects at once \(recommended\), use the `--all-sub-projects` flag:

  ```text
  snyk test --all-sub-projects
  ```

**Note:** Each of the individual sub-projects appears as a separate Snyk project in the UI.

* To scan a specific project \(for example, _myapp_\):

  ```text
  snyk test --sub-project=myapp
  ```

### Solving Issues on Gradle CLI Projects with lockfile

If your gradle project makes use of a single **gradle.lockfile** or multiple **\*.lockfile** per configuration and you are having the following issue

**Gradle Error \(short\): &gt; Could not resolve all dependencies for configuration ':compileOnly'. &gt; Locking strict mode: Configuration ':compileOnly' is locked but does not have lock state.**

Bear in mind that **compileOnly configuration** **has been deprecated** and even if your project successfully generates a lockfile, it will not contain \`compileOnly\` state because this configuration cannot be resolved. Only resolvable configurations compute a dependency graph. In order to solve this issue we suggest you **update your build.gradle containing dependencyLocking logic with the following instruction**

```text
compileOnly {resolutionStrategy.deactivateDependencyLocking() }
```

This will **ignore compileOnly** and save only the necessary information to analyse your project/projects.

### Configurations

Gradle dependencies are declared for a particular scope, each scope is represented by Gradle with the help of [Configurations](https://docs.gradle.org/current/userguide/declaring_dependencies.html#sec:what-are-dependency-configurations). For example:

* **compileOnly** configuration for development dependencies
* **compile** configuration that includes compile and runtime dependencies

By default Snyk merges all configurations returned by Gradle to dependency graph based on the sum total of the dependencies across all configurations in the project/projects.

To test a specific configuration:

* Use the `--configuration-matching` option with a [Java regular expression](https://docs.oracle.com/javase/tutorial/essential/regex/) \(case-insensitive\) as its parameter. The configuration that matches is then tested. If several configurations match, they are all merged and resolved together. For example: `'^releaseRuntimeClasspath$|^compile$'`
* If the different sub-projects include different configurations, scan each sub-project separately.

**Examples of how you can use the --configuration-matching:**

* `--configuration-matching=compile` will match compile, testCompile, compileOnly etc;
* `--configuration-matching=^compile$` will match only compile;
* `--configuration-matching='^(debug|release)compile$'` will match debugCompile and releaseCompile

### Gradle Android build variants

Android Gradle supports creating different versions of your app by configuring [build variants.](https://developer.android.com/studio/build/build-variants)

Since Snyk defaults to merging all available configurations the variants will result in a clash of un-mergeable configurations.

In such cases, Snyk scan fails with an error from Gradle which may contain one of the following messages:

* _Cannot choose between the following configurations of `project :mymodulewithvariants`_
* _Cannot choose between the following variants of `project :mymodulewithvariants`_
* _Could not select value from candidates_

To avoid such conflicts:

* **Use a specific configuration\(s\):** if you know of a build configuration that has all the required attributes and the configuration is identical across all sub-projects included in the test, specify that configuration.  
  For example:

  ```text
  --configuration-matching=prodReleaseRuntimeClasspath
  ```

* **Explicitly specify the dependency configuration:** modify intra-project dependencies in your build.gradle file\(s\) to use a specific configuration

  ```text
    dependencies {
        implementation project(path: ':mymodulewithvariants', configuration: 'default')
    }
  ```

* **Suggest configuration attributes:** if you receive an error when running the command, the error may indicate which attribute values are available, while the error details from Gradle also indicate which dependency variants match which attributes. Using these details, add the attribute filter option.  
  For example:

  ```text
  snyk test --configuration-attributes=buildtype:release,usage:java-runtime,mode:demo
  ```

  matches the variants using `com.android.build.api.attributes.BuildTypeAttr=release` and `org.gradle.usage=java-runtime`

### Pass extra arguments directly to Gradle or Maven via Snyk CLI

You can pass any extra Gradle or Maven arguments directly to **gradle** or **mvn** by providing them after a Snyk command like so:

```text
snyk test -- --build-cache
```

**Examples of how you can use Maven arguments with the Snyk CLI**

Test a specific Maven profile called “prod”.

```text
snyk test -- -prod
```

Add a system property from your pom.xml file.

For example:

The package version appears in your pom.xml

```text
${pkg_version}
```

Define the system property like this:

```text
snyk test -- -Dpkg_version=1.4
```

### Gradle daemon

By default, Snyk passes `gradle build --no-daemon` in the background when running `snyk test` and `snyk monitor`. If for any reason, you run into trouble, try this:

1. Start the Gradle daemon.
2. Add `--daemon` to your `snyk test` or `snyk monitor`.

### Support contact

If you are having any trouble testing your projects with Snyk, collect the following details and send them to us at `<`[`support@snyk.io`](mailto:support@snyk.io)`>` so we can help you out:

* `build.gradle`
* `settings.gradle` \(especially if we did not pick up a version of a package\)
* The output from the following commands:
  * `$ snyk test -d`
  * `$ gradle dependencies -q`

## Git services for Gradle projects

After you select a project for import, we build the dependency tree based on the `build.gradle` file and \(optional\) `gradle.lockfile`.

`build.gradle.kts` files are not currently supported in Git.

If a lockfile is present, Snyk will use it to accurately resolve the final version of dependencies used in the project.

Gradle lockfiles are an opt-in feature that, among other benefits, enable reproducible builds.Read more about Gradle dependency locking at [https://docs.gradle.org/current/userguide/dependency\_locking.html](https://docs.gradle.org/current/userguide/dependency_locking.html)

## Git services for Maven projects

After you select a project for import, we build the dependency tree based on the `pom.xml` file.

## Git settings for Java

From the Snyk UI you can specify mirrors or repositories from which you’d like to resolve packages in Artifactory for Maven.

See the page below for more details on configuring the Artifactory integration.

{% page-ref page="../../integrations/private-registry-integrations/artifactory-registry-for-maven.md" %}

## Additional Snyk support for Java

In addition to the CLI and Snyk UI features, you can also check your Java projects with these plugins:

* [Maven plugin for your build flow](../../integrations/ci-cd-integrations/maven-plugin-integration.md)

