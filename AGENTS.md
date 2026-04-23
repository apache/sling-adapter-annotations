# Project overview
`adapter-annotations` is a small Apache Sling Java library that defines annotation types used to generate Sling Adapter metadata JSON. The module is a single Maven artifact (`jar`) with source under `src/main/java` and no in-repo runtime service layer; build/test behavior is inherited mostly from the Apache Sling parent POM.

# Core commands
- Build/compile: `mvn -q clean package`
- Full test suite: `mvn -q test`
- Run a single test class: `mvn -q -Dtest=MyTest test`
- Run a single test method: `mvn -q -Dtest=MyTest#myMethod test`
- Lint/type-check/quality gates (via parent POM): `mvn -q verify`
- Install artifact locally: `mvn -q clean install`
- Release-style full build (clean + tests + checks): `mvn -q clean verify`
- Dev server(s): `echo "No dev server in this repository (library-only module)."`

# Project layout
```text
.
├── `pom.xml`                 # Maven module definition (inherits Apache Sling parent)
├── `README.md`               # Short module description
├── `CONTRIBUTING.md`         # Points to Apache Sling contribution process
├── `Jenkinsfile`             # CI entrypoint (`slingOsgiBundleBuild()`)
└── `src/`
    └── `main/`
        └── `java/`
            └── `org/apache/sling/adapter/annotations/`
                ├── `Adapter.java`
                ├── `Adaptable.java`
                └── `Adaptables.java`
```

# Development patterns & constraints
- Language/toolchain: Java (`sling.java.version` is `8`) built with Maven (`pom.xml`).
- Module system: plain Java package layout under `org.apache.sling.adapter.annotations`; no JPMS module descriptors.
- Style in existing code:
  - Use 4-space indentation.
  - Keep package names lowercase; type names use `UpperCamelCase`.
  - Keep constants/method names aligned with Java conventions (`UPPER_SNAKE_CASE` for constants, `lowerCamelCase` for members).
  - Preserve ASF license header in Java source files.
  - Existing Javadocs often use `<code>...</code>` formatting; stay consistent nearby.
- Imports: standard Java imports first; avoid wildcard imports.
- CSS/frontend rules: not applicable in this repository (no web UI assets).
- Prefer minimal API-surface changes; this module is consumed as a shared annotations artifact.

# Git workflow
- Primary branch is `master` (protected in `.asf.yaml`); do not commit directly to it.
- Create a short-lived feature branch from `master`: `git checkout -b <type>/<short-description>`.
- Commit messages should follow Apache Sling practice: include issue key when available, e.g. `SLING-12345 concise imperative summary`.
- Open a PR targeting `master`; merged PR branches are auto-deleted (`.asf.yaml`).
- Follow Apache Sling contribution guidance in `https://sling.apache.org/contributing.html`.

# Testing guidelines
- Test framework: Maven Surefire (`maven-surefire-plugin`) runs unit tests with `mvn test`.
- Place tests in `src/test/java` mirroring package structure from `src/main/java`.
- Name tests `*Test.java` (or other Surefire-default patterns) so they are auto-discovered.
- Run one test with `mvn -q -Dtest=MyTest test`; if you intentionally target a maybe-missing test in scripts, add `-Dsurefire.failIfNoSpecifiedTests=false`.
- Coverage: no module-specific coverage profile is defined here; if needed, run coverage via your CI/profile conventions in the parent build chain and publish in external tooling (for example SonarCloud from CI).

# Gotchas
- This repository is intentionally small; most build and quality behavior comes from the parent POM, so check effective Maven config before changing plugins.
- There may be zero tests in-tree at times; `mvn test` can still succeed.
- `README.md` points to a newer related module (`sling-org-apache-sling-adapter-annotations`); avoid confusing the two when making changes.
- CI is driven by `Jenkinsfile` via `slingOsgiBundleBuild()`, so local behavior should be validated with Maven commands above.
