# casehub-parent

Org-level parent POM and BOM for [casehubio](https://github.com/casehubio) projects.

## What it provides

- **BOM** — dependency version management for all casehubio artifacts, so consumers don't pin individual versions
- **Plugin management** — common Maven plugin versions (compiler, surefire)
- **GitHub Packages publishing** — CI wired to publish SNAPSHOTs on every merge to main

## Current version

`0.2-SNAPSHOT`

## Using the BOM

Import in your `dependencyManagement` section:

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>io.casehubio</groupId>
      <artifactId>casehub-parent</artifactId>
      <version>0.2-SNAPSHOT</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

Then declare casehubio dependencies without versions:

```xml
<dependency>
  <groupId>io.quarkiverse.ledger</groupId>
  <artifactId>quarkus-ledger</artifactId>
</dependency>
```

## GitHub Packages authentication

Add to `~/.m2/settings.xml` for local development:

```xml
<servers>
  <server>
    <id>github-casehubio</id>
    <username>YOUR_GITHUB_USERNAME</username>
    <password>YOUR_GITHUB_PAT</password>
  </server>
</servers>
```

In CI, the `GITHUB_TOKEN` secret handles authentication automatically.

## Projects in this BOM

| Artifact | groupId |
|---|---|
| `quarkus-ledger` | `io.quarkiverse.ledger` |
| `quarkus-ledger-deployment` | `io.quarkiverse.ledger` |
| `casehub-ledger` | `io.casehub` |

## License

Apache 2.0
