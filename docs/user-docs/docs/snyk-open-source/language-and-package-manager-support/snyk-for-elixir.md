# Snyk for Elixir

Snyk offers security scanning to test your [Elixir](https://www.notion.so/Elixir-7c925900bf774c84b83b65c14084e80e) projects for vulnerabilities using the CLI.

## Features

| Package managers/Features | CLI support | Git support | License scanning | Remediation | Runtime monitoring |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ![hex\_80x80.png](../../.gitbook/assets/hex_80x80.png) | [Mix](https://hexdocs.pm/mix/Mix.html)/[Hex](https://hex.pm/) | ✔︎ |  |  |  |  |

## How it works

Snyk builds a dependency tree for your project by analyzing your manifest and lock files.

After Snyk builds the tree, Snyk uses our [vulnerability database](https://snyk.io/vuln) to find vulnerabilities in the packages anywhere in the dependency tree.

## Snyk CLI tool for Elixir projects

### Mix/Hex

{% hint style="info" %}
To scan your dependencies, first install Elixir and Mix. For details, [see here](https://elixir-lang.org/install.html).
{% endhint %}

Mix is a build tool that provides tasks for creating, compiling, and testing Elixir projects, managing its dependencies, and more.

Mix manages dependencies by integrating with the Hex package manager.

To build the dependency tree, Snyk analyzes your `mix.exs` and `mix.lock` files. The `mix.lock` file must be present and in sync with the `mix.exs` file.

**Project naming**

Projects in the Snyk UI are named according to the `app` keyword from the `project/0` function exported by `Mix.Project` in the main `mix.exs` file.

To override the name, use the `--project-name` CLI parameter.

**Umbrella projects**

If you test a Mix Umbrella project, Snyk detects that this is an umbrella project and includes all the child apps automatically.

Along with the main `mix.exs`, each app `mix.exs` appears as a separate project in the Snyk UI, named according to the path to the app.

**Dependency types**

Snyk fully supports all `:hex` packages listed in the Mix project, including all their transitive dependencies and any vulnerabilities.

Hex support includes both Elixir and Erlang packages.

Snyk also has limited support for `:path`, `:git` and `:github` dependencies, but not their transitive dependencies or vulnerabilities.

* `:path` dependencies appear in the dependency tree by name
* `:git` and `:github` dependencies appear in the dependency tree by repository URL and version \(either `:branch`, `:tag` or `:ref`, as defined in the `mix.exs` file\)

