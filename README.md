<a href="./LICENSE">
<img src="./images/public-domain.svg" alt="Public Domain"
align="right" width="20%" height="auto"/>
</a>

# Modern Java/JVM Build Practices

[![Gradle build](https://github.com/binkley/modern-java-practices/actions/workflows/ci-earthly-gradle.yml/badge.svg)](https://github.com/binkley/modern-java-practices/actions)
[![Maven build](https://github.com/binkley/modern-java-practices/actions/workflows/ci-earthly-maven.yml/badge.svg)](https://github.com/binkley/modern-java-practices/actions)
[![pull requests](https://img.shields.io/github/issues-pr/binkley/modern-java-practices.svg)](https://github.com/binkley/modern-java-practices/pulls)
[![issues](https://img.shields.io/github/issues/binkley/modern-java-practices.svg)](https://github.com/binkley/modern-java-practices/issues/)
[![vulnerabilities](https://snyk.io/test/github/binkley/modern-java-practices/badge.svg)](https://snyk.io/test/github/binkley/modern-java-practices)
[![license](https://img.shields.io/badge/license-Public%20Domain-blue.svg)](http://unlicense.org/)

**Modern Java/JVM Build Practices** is an article-as-repo on building modern
Java/JVM projects using
[Gradle](https://docs.gradle.org/current/userguide/userguide.html) and
[Maven](https://maven.apache.org/what-is-maven.html), and a _starter project_
in Java.

> [!NOTE]
> I am in progress of moving sections of the `README.md` to individual pages
> in [the wiki](https://github.com/binkley/modern-java-practices/wiki), and
> make it easier for bringing advice up to date, and adding new approaches.
> Please forgive the shambles while this is in progress.

The best documentation headline on how to approach your build and pipeline:
[**Builds are
programs**](https://www.clojure.org/guides/tools_build#_builds_are_programs).
This is true regardless of what language or build tool you choose, and you
should treat your build and your pipeline as worthy of your attention the same
as you would your project source code.
I'm showing you practices and tools that help you make these as _first-class_
alongside your program or library.

The focus is _best build practices_ and _project hygiene_.
This document is _agnostic_ between Gradle and Maven: discussion in each section
covers both tools (alphabetical order, Gradle before Maven).
See [_My Final Take on Gradle (vs.
Maven)_](https://blog.frankel.ch/final-take-gradle/) for an opinionated view.

This is not a JVM starter for only Java:
I use it for starting my Kotlin projects, and substitute complilation and code
quality plugins.
Any language on the JVM can find practices and tips.

> [!NOTE]
> _Scala_ and _Clojure_ have their own prefered build tools not covered here;
> however, the advice and examples for your **build pipeline** are intended to
> be just as helpful for those JVM languages.
> Groovy and Kotlin can use the examples directly (they both tend towards the
> _Gradle_ option on build tools).

As a _guide_, this project focuses on:

* A quick starter for JVM projects using Gradle or Maven.
  [Fork](https://github.com/binkley/modern-java-practices/fork) me,
  [clone](https://github.com/binkley/modern-java-practices.git) me, copy/paste
  freely!
  I am [_Public Domain_](http://unlicense.org/)
* Discuss&mdash;and illustrate (through code)&mdash;sensible default practices;
  highlight good build tools and plugins
* Document pitfalls that turned up.
  Some were easy to address after Internet search; some were challenging
  (see "Tips" sections)
* Do not be an "all-in-one" solution. You know your circumstances best.
  I hope this project helps you discover build improvements you love.
  Please share with others through
  [issues](https://github.com/binkley/modern-java-practices/issues) or
  [PRs](https://github.com/binkley/modern-java-practices/pulls)

### Two recurring themes

* _Shift problems left_ &mdash; Find issues earlier in your build&mdash;before
  you see them in production
* _Make developer life easier_ &mdash; Automate build tasks often done by
  hand: get your build to complain (_fail_) locally before sharing with your
  team, or fail in CI before deployment

### What is a _Starter_?

A project starter has several goals:
- Help a new project get up and running with minimal fuss.
- Show examples of _best practices_.
- Explain the _why_ for choices, and help you pick what makes most sense for
  your project.

This starter project is focused on _build_:
- Easy on-ramp for new folks to try out your project for themselves
- Support new contributors to your project that they become productive quickly
- Support current contributors in the build, get out of their way, and make
  everyday things easy

This starter project has minimal dependencies.
The focus is on Gradle and Maven plugins and configuration so that you and
contributors can focus on the code, not on setting up the build.

### Summing up

- _I'm not a great programmer; I'm just a good programmer with great habits._
  &mdash;
  [Kent Beck](https://www.goodreads.com/quotes/532211-i-m-not-a-great-programmer-i-m-just-a-good-programmer)
- _Make it work, make it right, make it fast_
  &mdash; [C2 Wiki](http://wiki.c2.com/?MakeItWorkMakeItRightMakeItFast)

> [!NOTE]
> **NB** &mdash; This is a _living document_.
> The project is frequently updated to pick up new dependency or plugin 
> versions, and improved practices; the `README.md` and
> [wiki](https://github.com/binkley/modern-java-practices/wiki) update
> recommendations.
> This is part of what _great habits_ looks like: you do not just show love
> for your developers and users, but enable them to feed back into projects
> and help others.
> See [_Reusing this
> project_](https://github.com/binkley/modern-java-practices/wiki/Reusing-this-project)
> for tips on pulling in updates.

(Credit to Yegor Bugayenko for [_Elegant
READMEs_](https://www.yegor256.com/2019/04/23/elegant-readme.html).)

---

<a title="Try it">
<img src="./images/try.png" alt="Run from a local script"
align="right" width="20%" height="auto"/>
</a>

## Try it

You should "kick the tires" and get a feel for what parts of this project
you'd like to pull into your own projects and builds.
You run across lots of projects:
Let's make this one helpful for you.

After cloning or forking this project to your machine, try out the local build
that makes sense for you:

```shell
$ ./gradlew build  # Local-only build
$ earthly +build-with-gradle  # CI build with Earthly
$ ./mvnw verify  # Local-only build
$ earthly +build-with-maven  # CI build with Earthly
```

Notice that you can run the build purely localy, or _in a container_?

I want to convince you that running your builds in a container fixes the "it
worked on my machine" problem, and show you how to pick up improvements for
your build that helps you and others be _awesome_.

See what the starter "run" program does:

```shell
$ ./run-with-gradle.sh
$ ./run-with-maven.sh
```

A "starter" program is the simplest of all possible ["smoke
tests"](https://en.wikipedia.org/wiki/Smoke_testing_(software)), meaning,
the minimal things _just work_, and when you check other things, maybe smoke
drifts from your computer as circuits burn out\[\*\].

\* I'm just kidding.
Amazon or Microsoft or Google cloud would have quite different problems than
"white smoke" from computers \[\*\*\].

\*\* Actually, this really happened me in a data center before the cloud.

---

<a title="Changes">
<img src="./images/changes.png" alt="Changes"
align="right" width="20%" height="auto"/>
</a>

## Recent significant changes

- **IN PROGRESS**: Move sections from `README.md` to the GitHub wiki.
  This is for breaking up an overlong (11k words) README into digestible
  sections.
- Batect: Remove support for Batect as the author has archived that project.
  Please use _Earthly_ for local containerized builds.
  I'll be researching other options, and updating to show those and examples.
  Advice remains the same: Run your local build in a container for
  reproducibility, and have CI do the same to exactly repeat your local
  builds.
- This project uses JDK 21.
  Here is [the previous commit using JDK 17](https://github.com/binkley/modern-java-practices/commit/039f6f45fade51da0c548bf5d61b8013423ab8b9)
- Gradle: build with Gradle 8.x.
- Gradle: remove use of `testsets` plugin for integration testing in favor of
  native Gradle.
  This is in support of Gradle 8 and may be helpful in seeing changes you need
  for Gradle 8 support.

---

<a title="Table of Contents">
<img src="./images/table-of-contents.png" alt="Table of Contents"
align="right" width="20%" height="auto"/>
</a>

## Table Of Contents

* [Try it](#try-it)
* [Recent significant changes](#recent-significant-changes)
* [Introduction](https://github.com/binkley/modern-java-practices/wiki#introduction)
* [Reusing this project](https://github.com/binkley/modern-java-practices/wiki/Reusing-this-project)
* [Contributing](#contributing)
* [You and your project](https://github.com/binkley/modern-java-practices/wiki/You-and-your-project)
* [Commits](https://github.com/binkley/modern-java-practices/wiki/Commits)
* [Getting your project
  started](https://github.com/binkley/modern-java-practices/wiki/Getting-your-project-started)
* [The JDK](https://github.com/binkley/modern-java-practices/wiki/The-jdk)
* [Use Gradle or
  Maven](https://github.com/binkley/modern-java-practices/wiki/Use-gradle-or-maven)
* [Setup your
  CI](https://github.com/binkley/modern-java-practices/wiki/Setup-your-ci)
* [Keep local consistent with CI](https://github.com/binkley/modern-java-practices/wiki/Keep-local-consistent-with-CI)
* [Maintain your
  build](https://github.com/binkley/modern-java-practices/wiki/Maintain-your-build)
* [Choose your code style](https://github.com/binkley/modern-java-practices/wiki/Choose-your-code-style)
* [Generate
  code](https://github.com/binkley/modern-java-practices/wiki/Generate-code)
* [Leverage the compiler](https://github.com/binkley/modern-java-practices/wiki/Leverage-the-compiler)
* [Use
  linting](https://github.com/binkley/modern-java-practices/wiki/Use-linting)
* [Use static code analysis](https://github.com/binkley/modern-java-practices/wiki/Use-static-code-analysis)
* [Shift security left](https://github.com/binkley/modern-java-practices/wiki/Shift-security-left)
* [Leverage unit testing and coverage](#leverage-unit-testing-and-coverage)
* [Use mutation testing](https://github.com/binkley/modern-java-practices/wiki/Use-mutation-testing)
* [Use integration testing](https://github.com/binkley/modern-java-practices/wiki/Use-integration-testing)
* [Debugging](https://github.com/binkley/modern-java-practices/wiki/Debugging)
* [Samples](#samples)
* [Going
  further](https://github.com/binkley/modern-java-practices/wiki/Going-further)
* [Problems](#problems)
* [Credits](#credits)

---

<a href="https://modernagile.org/" title="Modern Agile">
<img src="./images/modern-agile-wheel-english.png" alt="Modern Agile"
align="right" width="20%" height="auto"/>
</a>

## Introduction

See
[_Introduction_](https://github.com/binkley/modern-java-practices/wiki#introduction)
in the wiki.

---

<a href="https://github.com/binkley/modern-java-practices/fork" title="Reuse">
<img src="./images/reuse.png" alt="Reuse"
align="right" width="20%" height="auto"/>
</a>

## Reusing this project

See
[_Reusing this project_](https://github.com/binkley/modern-java-practices/wiki/Reusing-this-project)
in the wiki.

---

<img src="./images/Wikibooks-contribute-icon.svg" alt="Contributing"
align="right" width="20%" height="auto"/>
</a>

## Contributing

See [`CONTRIBUTING.md`](./CONTRIBUTING.md).
Please [file issues](https://github.com/binkley/modern-java-practices/issues),
or contribute [pull
requests](https://github.com/binkley/modern-java-practices/pulls)!
I'd love a conversation with you.

---

## Commits

See [_Commits_](https://github.com/binkley/modern-java-practices/wiki/Commits)
in the wiki.

---

<!-- TODO: Should this section be moved or removed? It is awkward here -->
## You and your project

See [_You and your
project_](https://github.com/binkley/modern-java-practices/wiki/You-and-your-project)
in the wiki.

---

## Getting your project started

See [_Getting your project
started_](https://github.com/binkley/modern-java-practices/wiki/Getting-your-project-started)
in the wiki.

---

<a href="https://adoptium.net/" title="Adoptium">
<img src="./images/adoptium.png" alt="Adoptium"
align="right" width="20%" height="auto"/>
</a>

## The JDK

See [_The JDK_](https://github.com/binkley/modern-java-practices/wiki/The-jdk)
in the wiki.

---

<!--- TODO: better formating for images vs text -->
<a href="https://maven.apache.org/" title="Maven">
<img src="./images/maven.png" alt="Maven"
align="right" width="15%" height="auto"/></a>
<a href="https://gradle.org/" title="Gradle">
<img src="./images/gradle.png" alt="Gradle"
align="right" width="15%" height="auto"/></a> 

## Use Gradle or Maven

See [_Use Gradle or Maven_](https://github.com/binkley/modern-java-practices/wiki/Use-gradle-or-maven)
in the wiki.

---

<a href="http://www.ambysoft.com/essays/whyAgileWorksFeedback.html"
title="Why Agile Software Development Techniques Work: Improved Feedback">
<img src="./images/bug-costs.jpg" alt="Length of Feedback Cycle"
align="right" width="20%" height="auto"/>
</a>

## Setup your CI

See [_Setup your
CI_](https://github.com/binkley/modern-java-practices/wiki/Setup-your-ci)
in the wiki.

---

## Keep local consistent with CI

See [_Keep local consistent with
CI_](https://github.com/binkley/modern-java-practices/wiki/Getting-your-project-started)
in the wiki.

---

<img src="./images/maintain-build.jpg" alt="Maintain build"
align="right" width="20%" height="auto"/>

## Maintain your build

Treat your build as you would your codebase: Maintain it, refactor as needed,
run performance testing, _et al_.

### Know what your build does

What does your build do exactly, and in what order? You can ask Gradle or Maven
to find out:

* [Gradle Task Tree plugin](https://github.com/dorongold/gradle-task-tree)
  with `./gradlew some...tasks taskTree`
* [Maven Buildplan plugin](https://buildplan.jcgay.fr/)
  with `./mvnw buildplan:list` (see plugin documentation for other goals and
  output format)

Each of these have many options and features, and are worth exploring.

### Keep your build clean

Let tools tell you when you have dodgy dependencies, or an inconsistent setup.
For example, leverage `jdeps` which
[comes with the JDK](https://docs.oracle.com/en/java/javase/21/docs/specs/man/jdeps.html).
Jdeps spots, for example, if you have a multi-version jar as a dependency that
does not include _your_ JDK version (an example of this may be is JUnit), or if
your code depends on _internal_ (non-public) classes of the JDK
(important especially when using the JDK module system).

#### Gradle

The [Kordamp plugin](https://github.com/kordamp/jdeps-gradle-plugin) used for
Gradle does not fail the build when jdeps errors, and only generates a report
text file. See
[this issue](https://github.com/kordamp/jdeps-gradle-plugin/issues/16).

#### Maven

Try Maven with `dependency:tree -Dverbose`.
This will show conflicting versions of dependencies.

### Keep local builds quiet

It is frustrating for local devs when something horrible happened during the
build (say a production with "ERROR" output during a test), but:

1. The build is **GREEN**, and developers should trust that
2. There is too much output in the local build, so developers don't spot
   telltale signs of trouble

There are many approaches to this problem. This project uses JDK logging as [an
example](https://docs.oracle.com/en/java/javase/21/docs/api/java.logging/java/util/logging/FileHandler.html),
and keeps the build quiet in
[`config/logging.properties`](config/logging.properties).

### Keep CI builds noisy

In CI, this is different, and there you want as much output as possible to
diagnose the unexpected.

<!-- TODO: This section is under construction.
Looking for input on quiet local builds and noisy CI builds. -->

### Keep your build current

An important part of _build hygiene_ is keeping your build system, plugins, and
dependencies up to date. This might be simply to address bug fixes
(including bugs you weren't aware of), or might be critical security fixes. The
best policy is: _Stay current_. Others will have found&mdash;reported
problems&mdash;, and 3<sup>rd</sup>-parties may have addressed them. Leverage
the power of [_Linus' Law_](https://en.wikipedia.org/wiki/Linus%27s_law) ("given
enough eyeballs, all bugs are shallow").

### Keep plugins and dependencies up-to-date

* [Gradle](https://github.com/ben-manes/gradle-versions-plugin)
  Benjamin Manes is kind enough in his plugin project to list alternatives.
  If you are moving towards [Gradle version
  catalogs](https://docs.gradle.org/current/userguide/platforms.html), you might
  consider
  [refreshVersions](https://jmfayard.github.io/refreshVersions/)
* [Maven](https://www.mojohaus.org/versions-maven-plugin/)
* Team agreement on release updates only, or if non-release plugins and
  dependencies make sense for your situation
* Each of these plugins for Gradle or Maven have their quirks.  **Do not treat
  them as sources of truth but as recommendations**. _Use your judgment_. In
  parallel, take advantage of CI tooling such as
  [Dependabot (Github)](https://github.com/dependabot) or
  [Dependabot (GitLab)](https://gitlab.com/dependabot-gitlab/dependabot)

An example use which shows most outdated plugins and dependencies (note that one
Maven example modifies your `pom.xml`, a fact you can choose or avoid):

```shell
$ ./gradlew dependencyUpdates
# output ommitted
$ ./mvnw versions:update-properties  # Updates pom.xml in place
$ ./mvnw versions:display-property-updates  # Just lists proposed updates
# output ommitted
```

This project keeps Gradle version numbers in
[`gradle.properties`](./gradle.properties), and for Maven in
[the POM](./pom.xml), and you should do the same.

Since your `pom.xml` is in Git, `versions:update-properties` is _safe_ as you
can always revert changes, but some folks want to look before doing.

#### Tips

* Gradle and Maven provide _default versions_ of bundled plugins. In both built
  tools, the version update plugins need you to be _explicit_ in stating
  versions for bundled plugins, so those versions are visible for update
* Enable _HTML reports_ for local use; enable _XML reports_ for CI use in
  integrating with report tooling
* To open the report for Jdeps, build locally and use the
  `<project root>/build/reports/jdeps/` (Gradle) path.
  The path shown in a Docker build is relative to the interior of the container

### Automated dependency upgrade PRs

*NB* &mdash;
[Dependabot](https://github.blog/2020-06-01-keep-all-your-packages-up-to-date-with-dependabot/)
may prove speedier for you than updating dependency versions locally, and runs
in CI (GitHub) on a schedule you pick. It submits PRs to your repository when it
finds out of date dependencies. See
[`dependabot.yml`](./.github/dependabot.yml) for an example using a daily
schedule.

A similar choice is [Renovate](https://github.com/renovatebot/renovate).

#### More on Gradle version numbers

Your simplest approach to Gradle is to keep everything in `build.gradle`. Even
this unfortunately still requires a `settings.gradle` to define a project
artifact name, and leaves duplicate version numbers for related dependencies
scattered through `build.gradle`.

Another approach is to rely on a Gradle plugin such as that from Spring Boot to
manage dependencies for you. This unfortunately does not help with plugins at
all, nor with dependencies that Spring Boot does not know about.

This project uses a 3-file solution for Gradle versioning, and you should
consider doing the same:

* [`gradle.properties`](./gradle.properties) is the sole source of truth for
  version numbers, both plugins and dependencies
* [`settings.gradle`](./settings.gradle) configures plugin versions using the
  properties
* [`build.gradle`](./build.gradle) uses plugins without needing version numbers,
  and dependencies refer to their property versions

The benefits of this approach grow for Gradle multi-project projects, where you
may have plugin and dependency versions scattered across each `build.gradle`
file for you project and subprojects.

So to adjust a version, edit `gradle.properties`. To see this approach in action
for dependencies, try:

```shell
$ grep junitVersion gradle.properties setttings.gradle build.gradle
gradle.properties:junitVersion=5.7.0
build.gradle:    testImplementation "org.junit.jupiter:junit-jupiter:$junitVersion"
build.gradle:    testImplementation "org.junit.jupiter:junit-jupiter-params:$junitVersion"
```

#### Note on `toolVersion` property

If you use the `toolVersion` property for a plugin to update the called tool
separately from the plugin itself, _this is a convention_, not something the
Gradle API provides to plugins. As a consequence, the Versions plugin is unable
to know if your tool version is out of date. An example is the JaCoCo plugin
distributed with Gradle.

Two options:

- Do not use the `toolVersion` property unless needed to address a discovered
  build issue, and remove it once the plugin catches up to provide the tool
  version you need
- Continue using the `toolVersion` property, and as part of running
  `./gradlew dependencyUpdates`, manually check all `toolVersion`
  properties, and update `gradle.properties` as accordingly

**NB** &mdash; Maven handles this differently, and does not have this concern.

### Keep your build fast

A fast local build is one of the best things you can do for your team. There are
variants of profiling your build for Gradle and Maven:

* [Gradle build scan](https://scans.gradle.com/) with the `--scan` flag
* [Maven profiler extension](https://github.com/jcgay/maven-profiler) with
  the `-Dprofile` flag

See [an example build scan](https://scans.gradle.com/s/fik7c7bq25l3w) from May
1, 2023.

**NB** &mdash; [Build Scan](https://scans.gradle.com/#maven) supports Maven as
well when using the paid enterprise version.

### Keep your developers fast

Some shortcuts to speed up the red-green-refactor cycle:

* Just validate code coverage; do not run other parts of the build:
  - Gradle: `./gradlew clean jacocoTestReport jacocoTestCoverageVerification`
  - Maven: `./mvnw clean test jacoco:report jacoco:check`

### Tips

* Both [_dependency vulnerability checks_](#dependency-check) and
  [_mutation testing_](#use-mutation-testing)
  can take a while, depending on your project. If you find they slow your team
  local build too much, these are good candidates for moving to
  [CI-only steps](#setup-your-ci), such as a `-PCI` flag for Maven (see "Tips"
  section of [Use Gradle or Maven](#use-gradle-or-maven) for Gradle for an
  equivalent). This project keeps them as part of the local build, as the
  demonstration code is short
* See the bottom of [`build.gradle`](./build.gradle) for an example of
  customizing "new" versions reported by the Gradle `dependencyUpdates` task
* The equivalent Maven approach for controlling the definition of "new" is to
  use [_Version number
  rules_](https://www.mojohaus.org/versions-maven-plugin/version-rules.html)
* With the Gradle plugin, you can program your build to fail if dependencies are
  outdated. Read at
  [_Configuration option to fail build if stuff is out of
  date_](https://github.com/ben-manes/gradle-versions-plugin/issues/431#issuecomment-703286879)
  for details

---

## Choose your code style

Style is an often overlooked but very critical attribute of writing. The style
of writing directly impacts the readability and understandability of the end
product

There are 2 main java code styles

- [Sun code style](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf)
- [Google code style](https://google.github.io/styleguide/javaguide.html)

It is up to you which one you should choose. But the style should be chosen and
the style should be the same for everyone.

To maintain the same standard `config/ide/eclipse-java-google-style.xml`
or `intellij-java-google-style.xml` should be imported to your IDE. Checkstyle
should be configured based on the chosen standard

---

<img src="./images/coffee-grinder.png" alt="Coffee grinder"
align="right" width="20%" height="auto"/>

## Generate code

When sensible, prefer to generate rather than write code. Here's why:

* [Intelligent laziness is a virtue](http://threevirtues.com/)
* Tools always work, unless they have bugs, and you can fix bugs. Programmers
  make typos, and fixing typos is a challenge when not obvious. Worse are [_
  thinkos_](https://en.wiktionary.org/wiki/thinko); code generation does not "
  think", so is immune to this problem
* Generated code does not need code review, only the source input for generation
  needs review, and this is usually shorter and easier to understand. Only your
  hand-written code needs review
* Generated code is usually ignored by tooling such as linting or code
  coverage (and there are simple workarounds when this is not the case). Your
  hand-written code needs tooling to shift problems left

Note that many features for which in Java one would use code generation
(_eg_, Lombok's [`@Getter`](https://projectlombok.org/features/GetterSetter)
or [`@ToString`](https://www.projectlombok.org/features/ToString)), can be
built-in language features in other languages such as Kotlin or Scala (_eg_,
[properties](https://kotlinlang.org/docs/reference/properties.html)
or [data classes](https://kotlinlang.org/docs/reference/data-classes.html)).

### Lombok

[Lombok](https://projectlombok.org/) is by far the most popular tool in Java for
code generation.
Lombok is an _annotation processor_, that is, a library (jar) which 
cooperates with the Java compiler.
([_An introductory guide to annotations and annotation
processors_](https://blog.frankel.ch/introductory-guide-annotation-processor/#handling-annotations-at-compile-time-annotation-processors)
is a good article if you'd like to read more on how annotation processing
works.)

Lombok covers many common use cases, does not have runtime dependencies, there
are plugins for popular IDEs that understand Lombok's code generation, and has
tooling integration for JaCoCo's output code coverage (see
[below](#leverage-lombok-to-tweak-code-coverage)).

Do note though, Lombok is not a panacea, and has detractors.
For example, to generate code as an annotation processor, it in places relies on
internal JDK APIs, though the situation has improved as the JDK exposes those
APIs in portable ways.

#### Leverage Lombok to tweak code coverage

Be sparing in disabling code coverage!
JaCoCo knows about Lombok's
[`@Generated`](https://projectlombok.org/api/lombok/Generated.html), and will
ignore annotated code.

A typical use is for the `main()` method in a framework such as Spring Boot
or [Micronaut](https://micronaut.io/).
For a _command-line program_, you will want to test your `main()`.

Do note that Lombok reflects on internal features of the JDK.
If you have issues, for _Maven_: use in your project the
`--add-opens java.base/java.lang=ALL-UNNAMED`
example from `.mvn/jvm.config`, and look to address these.
The solutions in the project are a "workaround" assuming Java 21.
This is a two-edged sword: as the JVM improves access controls, you may find,
especially dependencies, that there are times you want deep reflection.

#### Lombok configuration

[Configure Lombok](https://projectlombok.org/features/configuration) in
[`src/lombok.config`](./src/lombok.config) rather than the project root or a
separate `config` directory.
At a minimum:

```properties
config.stopBubbling=true
lombok.addLombokGeneratedAnnotation=true
lombok.anyConstructor.addConstructorProperties=true
lombok.extern.findbugs.addSuppressFBWarnings=true
```

Lines:

1. `stopBubbling` tells Lombok that there are no more configuration files higher
   in the directory tree
2. `addLombokGeneratedAnnotation` helps JaCoCo ignore code generated by Lombok
3. `addConstructorProperties` helps JSON/XML frameworks such as Jackson
   (this may not be relevant for your project, but is generally harmless, so the
   benefit comes for free)
4. `addSuppressFBWarnings` helps SpotBugs ignore code generated by Lombok

### More examples

* Automating `Dockerfile` &mdash; [_YMNNALFT: Easy Docker Image Creation with
  the Spring Boot Maven Plugin and
  Buildpacks_](https://spring.io/blog/2021/01/04/ymnnalft-easy-docker-image-creation-with-the-spring-boot-maven-plugin-and-buildpacks)
  (**NB** &mdash; you do not need to have a Spring Boot project to use the
  plugin: just [treat the plugin as a "regular"
  one](https://docs.spring.io/spring-boot/docs/2.4.1/maven-plugin/reference/htmlsingle/#build-image))

---

<img src="./images/gear.png" alt="Gear"
align="right" width="20%" height="auto"/>

## Leverage the compiler

Compilers targeting the JVM generally provide warning flags for dodgy code, and
a flag to turn warnings into errors: Use them.
The compiler is your first line of defense against code issues.

For example, add these flags with `javac`:

* `-Werror` -- turn warnings into errors, and fails the build
* `-Xlint:all,-processing` -- enable all warnings excluding annotation
  processing

Be judicious in disabling compiler warnings: they usually warn you for good
reasons.
For `javac`, disabled warnings might include `serial` or `deprecation`.

JVM compilers support `-Werror` (_eg_, `javac`, `kotlinc`, `scalac`, _et al_);
enabling/disabling specific warnings may be compiler-specific.

### Tips

* Consider using [_Error Prone_](https://errorprone.info/).
  _Error Prone_ is an excellent compiler plugin to fail problems earlier: fail 
  at compile-time rather than a runtime, however it can be overly strict
* Lombok annotation processing fails `-Xlint:all`.
  Use `-Xlint:all,-processing` to bypass warnings about annotation processing.
  In addition, using Lombok's configuration to add suppression annotations on
  generated code (so other tools will ignore generated code) needs the older
  Spotbugs annotations provided as a dependency

---

## Use linting

"Linting" is static code analysis with an eye towards style and dodgy code
constructs. The term
[derives from early UNIX](https://en.wikipedia.org/wiki/Lint_(software)).

Linting for modern languages is simple: the compiler complains on your behalf.
This is the case, for example, Golang. Having common team agreements on style
and formatting is a boon for avoiding
[bikeshedding](https://en.wikipedia.org/wiki/Law_of_triviality), and aids in:

* Reading a code base, relying on a similar style throughout
* Code reviews, focusing on substantive over superficial changes
* Merging code, avoiding trivial or irrelevant conflicts

Code style and formatting are _entirely_ a matter of team discussion and
agreement.
In Java, there is no recommended style, and `javac` is good at parsing almost
anything thrown at it.
However, humans reading code are not as well-equipped.

**Pick a team style, stick to it, and _enforce_ it with tooling.**

See the section [_Checkstyle_](#checkstyle) for more details on enforcement.

### Tips

* If you use Google Java coding conventions, consider
  [Spotless](https://github.com/diffplug/spotless) which can autoformat your
  code
* Consider use of [EditorConfig](https://editorconfig.org/) for teams when
  editor choice is up to each developer.
  EditorConfig is a cross-IDE standard means of specifying code formatting
  respected by common code editors (either directly, or through popular
  plugins)
* To open the report for Checkstyle, build locally and use the
  `<project root>/build/reports/checkstyle/` path.
  The path shown in a Docker build is relative to the interior of the container

---

## Use static code analysis

See [_Use static code
analysis_](https://github.com/binkley/modern-java-practices/wiki/Use-static-code-analysis)
in the wiki.

---

## Shift security left

See [_Shift security
left_](https://github.com/binkley/modern-java-practices/wiki/Shift-security-left)
in the wiki.

---

## Leverage unit testing and coverage

* [JaCoCo](https://www.jacoco.org/jacoco/)
* Use the "ratchet" pattern to fail the build when coverage drops.
  Robert Greiner talks more on this in [_Continuous Code Improvement Using 
  Ratcheting_](https://robertgreiner.com/continuous-code-improvement-using-ratcheting/)
  This follows the agile ["Boy Scout"
  principle](https://dzone.com/articles/the-boy-scout-software-development-principle)
* Fluent assertions &mdash; lots of options in this area
    * [AssertJ](https://assertj.github.io/doc/) &mdash; solid choice
    * Built assertions from Junit makes is difficult for developers to
      distinguish "actual" values from "expected" values. This is a limitation
      from Java as it lacks named parameters.
      Other frameworks compatible with JUnit provide more fluent assertions such
      as AssertJ.
      Different choices make sense depending on your source language

Unit testing and code coverage are foundations for code quality.
Your build should help you with these as much as possible. 100% coverage may
seem absurd;
however, levels of coverage like this come with unexpected benefits such as
finding dead code in your project or helping refactoring to be simple.
An example: with high coverage (say 95%+, your experience will vary)
simplifying your covered code may lower your coverage as uncovered code becomes
more prominent in the total ratio.

Setup for needed plugins:

* For Gradle use the `java` plugin
* For Maven, use more recent versions of the
  [Maven Surefire Plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)

(See [_suggestion : Ignore the generated
code_](https://github.com/hcoles/pitest/issues/347) for a Lombok/PITest issue.)

To see the coverage report (on passed or failed coverage), open:

* For Gradle, `build/reports/jacoco/test/html/index.html`
* For Maven, `target/site/jacoco/index.html`

This project also provides the coverage report as part of Maven's project
report.

The [`coverage`](./coverage.sh) script is helpful for checking your current
coverage state: try `./coverage -f all`.
Current limitations:
- Maven builds only
- Single module builds only

### Tips

* With Maven, _do use_ the available BOM (bill of materials) for JUnit.
  An example `pom.xml` block is:
  ```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.junit</groupId>
                <artifactId>junit-bom</artifactId>
                <version>${junit.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    ```
  This helps avoid dependency conflicts from other dependencies or plugins
* See [discussion on Lombok](#leverage-lombok-to-tweak-code-coverage) how to
  _sparingly_ leverage the `@Generated` annotation for marking code that JaCoCo
  should ignore
* Discuss with your team the concept of a "coverage ratchet". This means, once a
  baseline coverage percentage is agreed to, the build configuration will only
  raise this value, not lower it. This is fairly simple to do by periodically
  examining the JaCoCo report, and raising the build coverage percentage over
  time to match improvements in the report
* Unfortunately neither Gradle's nor Maven's JaCoCo plugin will fail your build
  when coverage _rises_!  This would be helpful for supporting the coverage
  ratchet
* You may find _mocking_ helpful for injection. The Java community is not of one
  mind on mocking, so use your judgment:
    * [Mockito](https://site.mockito.org/) is the "standard" choice, and is a
      dependency for the sample projects.
      For modern versions of Mockito, please use the `mockito-core` dependency
      rather than `mockito-inline`.
      See `TheFooTest.shouldRedAlertAsStaticMock` for an example.
      Note that this project has updated to Mockito 5.
      See [_v5.0.0_ release
      notes](https://github.com/mockito/mockito/releases/tag/v5.0.0) when
      updating from Mockito 4
    * [PowerMock](https://powermock.github.io/) provides additional features;
      however, Mockito normally covers use cases
    * Other Modern JVM languages &mdash; these languages may prefer different
      mocking libraries, _eg_, [MockK](https://mockk.io/) for Kotlin
    * You might consider complementary libraries to Mockito for specific
      circumstances, _eg_,
      [System Lambda](https://github.com/stefanbirkner/system-lambda)
      for checking STDOUT and STDERR, program exits, and use of system
      properties (_eg_, validate logging), also a dependency for the sample
      projects.  (*NB* &mdash; these are generally not parallelizable tests as
      they alter the state of the JVM. Another is the
      [JUnit Pioneer](https://junit-pioneer.org/) extension pack. If you need
      these, be cautious about using parallel testing features, and avoiding
      [Flaky Tests](https://hackernoon.com/flaky-tests-a-war-that-never-ends-9aa32fdef359))
* To open the report for JaCoCo, build locally and use the
  `<project root>/build/reports/jacoco/test/html/` path.
  The path shown in a Docker build is relative to the interior of the container

---

## Use mutation testing

See [_Use mutation
testing_](https://github.com/binkley/modern-java-practices/wiki/Use-mutation-testing)
in the wiki.

---

## Use integration testing

Here the project says "integration testing". Your team may call it by another
name. This means bringing up your application, possibly with
[fakes, stubs, mocks, spies, dummies, or doubles](http://xunitpatterns.com/Mocks,%20Fakes,%20Stubs%20and%20Dummies.html)
for external dependencies (databases, other services, _etc_), and running tests
against high-level functionality, but _not_ starting up external dependencies
themselves (_ie_, Docker, or manual command-line steps). Think of CI: what are
called here "integration tests" are those which do
_not_ need your CI to provide other services.

An example is testing `STDOUT` and `STDERR` for a command-line application.
(If you are in Spring Framework/Boot-land, use controller tests for your REST
services.)

Unlike `src/main/java` and `src/test/java`, there is no generally agreed
convention for where to put integration tests. This project keeps all tests
regardless of type in `src/test/java` for simplicity of presentation, naming
integration tests with "*IT.java". A more sophisticated approach may make sense
for your project.

If you'd like to keep your integration tests in a separate source root from unit
tests, consider these plugins:

* For Gradle, use [native Gradle to add new test
  sets](https://docs.gradle.org/current/userguide/java_testing.html#sec:configuring_java_integration_tests).
  (Previous versions of this project used the excellent
  [`testsets` plugin](https://github.com/unbroken-dome/gradle-testsets-plugin),
  however, it does not support Gradle 8)
* For Maven, use
  the [Maven Failsafe Plugin](https://maven.apache.org/failsafe/maven-failsafe-plugin/)

**Caution**: This project _duplicates_
[`ApplicationIT.java`](./src/test/java/demo/ApplicationIT.java) and
[`ApplicationTest.java`](./src/integrationTest/java/demo/ApplicationTest.java)
reflecting the split in philosophy between Gradle and Maven for integration
tests. Clearly in a production project, you would have only one of these.

### Tips

* For Maven projects, Apache maintains Failsafe and Surefire plugins as a pair,
  and share the same version numbers.
  This project uses a shared `maven-testing-plugins.version` property
* Baeldung
  has [a good introduction article](https://www.baeldung.com/maven-failsafe-plugin)
  on Maven Failsafe

---

## Going further

Can you do more to improve your build, and shift problems left (before they hit
CI or production)?
Of course!
Below are some topics to discuss with your team about making them part of the
local build.

### The Test Pyramid

<a href="https://martinfowler.com/bliki/TestPyramid.html"
title="TestPyramid">
<img src="./images/test-pyramid.png" alt="The test pyramid"
align="right" width="20%" height="auto"/>
</a>

What is the "Test Pyramid"? This is an important conceptual framework for
validating your project at multiple levels of interaction. Canonical resources
describing the test pyramid include:

* [_TestPyramid_](https://martinfowler.com/bliki/TestPyramid.html)
* [_The Practical Test
  Pyramid_](https://martinfowler.com/articles/practical-test-pyramid.html)

As you move your testing "to the left" (helping local builds cover more
concerns), you'll want to enhance your build with more testing at different
levels of interaction. These are not covered in this article, so research is
needed.

There are alternatives to the "test pyramid" perspective. Consider
[swiss cheese](https://blog.korny.info/2020/01/20/the-swiss-cheese-model-and-acceptance-tests.html)
if it makes more sense for your project.
The build techniques still apply.

**NB** &mdash; What this article calls
["integration tests"](#use-integration-testing) may have a different name for
your team.
You may have "system tests" for example.

### Use automated live testing when appropriate

"Live testing" here means spinning up a database or other remote service for
local tests, and not
using [fakes, stubs, mocks, spies, dummies, or
doubles](http://xunitpatterns.com/Mocks,%20Fakes,%20Stubs%20and%20Dummies.html).
In these tests, your project calls on _real_ external dependencies, albeit
dependencies spun up locally rather than in production or another environment.
These might be call "out of process" tests.

This is a complex topic, and this document is no guide on these. Some
potentially useful resources to pull into your build:

* [Flyway](https://flywaydb.org/) &mdash; Version your schema in production, and
  version your test data
* [LocalStack](https://github.com/localstack/localstack) &mdash; Local testing
  for AWS services
* [TestContainers](https://www.testcontainers.org/) &mdash; Local Docker for
  real database instances, or any Docker-provided service

### Use contract testing when appropriate

Depending on your program, you may want additional testing specific to
circumstances. For example, with REST services and Spring Cloud, consider:

* [_Consumer Driven Contracts_](https://spring.io/guides/gs/contract-rest/)

There are many options in this area. Find the choices which work best for you
and your project.

### Provide User Journey tests when applicable

Another dimension to consider for local testing: _User Journey_ tests.

* [_Why test the user
  journey?_](https://www.thoughtworks.com/insights/blog/why-test-user-journey)

---

<img src="./images/debugging.png" alt="Debugging in the container"
align="right" width="20%" height="auto"/>

## Debugging

> [!NOTE]
> This section is in progress, and needs instructions and examples for using
> "debug" from container builds using Earthly.

---

<img src="./images/sample.svg" alt="Sample"
align="right" width="20%" height="auto"/>

## Samples

These samples are external projects, are at varying states of maturity, and
are frequently updated (espcially for dependency versions).

### Kotlin

- [KUnits](https://github.com/binkley/kunits) (Maven) is a pleasure project to 
  represent units of measurement in Kotlin
- [Kotlin Rational](https://github.com/binkley/kotlin-rational) (Maven) 
  explores a math library for rationals (fractions) akin to `BigDecimal`
- [Magic Bus](https://github.com/binkley/kotlin-magic-bus) (Gradle) is a 
  library for using messaging patterns within a single application (it talks 
  to itself)

### Spring Boot

- [Spring Boot HATEOAS
  Database](https://github.com/binkley/kotlin-spring-boot-hateoas-database)
  (Maven) looks at Spring Boot features for Open API (Swagger), REST APIs, 
  HATEOAS, GraphQL, Prometheus, _et al_

## Problems

<a href="https://xkcd.com/303/" title="Compiling">
<img src="./images/compiling.png" alt="Compiling"
align="right" width="20%" height="auto"/>
</a>

### Why is my local build slow?

Both Gradle and Maven have tools to track performance time of steps in your
build:

* [Gradle build scans](https://scans.gradle.com/) &mdash; Not limited to
  Enterprise licenses, just build with `./gradlew --scan <tasks>` and follow the
  link in the output.
  <a
  href="https://cdn.jsdelivr.net/gh/binkley/modern-java-practices/docs/profile-run/gradle-profile.html"
  title="A sample Gradle profile for this project" type="text/html">
  See a sample Gradle profile for this project</a>.
* [Maven profiler](https://github.com/jcgay/maven-profiler) &mdash; run
  with `./mvnw -Dprofile <goals>` and open the local link in the output. This
  project includes the setup for [Maven extensions](.mvn/extensions.xml).
  <a
  href="https://cdn.jsdelivr.net/gh/binkley/modern-java-practices/docs/profile-run/maven-profile.html"
  title="A sample Maven profile for this project" type="text/html">
  See a sample Maven profile for this project</a>.

**TODO**: Fix the sample profile links to display as pages, not as raw HTML.

### My local build is still too slow

Congratulations!  You care, and you notice what is happening for your team.  
Local build time is _important_: gone are the days when a multi-hour, or even
30+ minute build, are viewed in most cases as the "cost of doing business".
And "compiling" is rarely any longer where your project takes most local build
time.

Use the Gradle or Maven instructions
in [keep your build fast](#keep-your-build-fast) to profile your build, and spot
where it spends time.

If you find your local build is taking too long, consider testing moving these
parts to CI with the cost to you of issues arising from delayed feedback:

* [Jdeps](#keep-your-build-clean)
* [DependencyCheck](#shift-security-left)
* [Integration tests](#use-integration-testing)
* [PITest](#use-mutation-testing)

_But beware_!  Your local build is now drifting away from CI, so you are pushing
problems off later in your build pipeline. Not everyone pays close attention to
CI failures, that is until something bad happens in production.

*IMPORTANT* &mdash; if you disable tools like the above in the _local_ build,
ensure you retain them in your _CI_ build. Your goal in this case is speed up
the feedback cycle locally while retaining the benefits of automated tooling.
You are making a bet: problems these tools find come up rarely (but can be
catastrophic when they do), so time saved locally repays time lost waiting for
CI to find these problems.

In the Gradle and Maven samples in this repository, _DependencyCheck_ and
_Mutation testing_ are typically the slowest steps in a local build;
_Integration tests_ are fast only because this project has very few (1), and are
samples only.
[YMMV](http://www.catb.org/jargon/html/Y/Your-mileage-may-vary.html)

Every project is different; your team and stakeholders need to judge the value
of quicker feedback to programmers of these concerns, and quicker feedback from
a faster local build. There is no "one size fits all" recommendation.

### It fails in CI, but passes locally

As much as you would like local builds to be identical to CI, this can still
happen for reasons of environment. Examples can include:

- Credentials needed in CI have changed: Update your CI configuration
- Network routing has changed, and CI is in a different subnet from local:
  Talk with your Infrastructure team
- CI includes steps to push successful builds further down the line to other
  environments, and something there went wrong: Talk with your Infrastructure
  team
- Dependencies break in CI: If CI uses an internal dependency repository, check
  in with the maintainers of the repository

## Credits

Many thanks to:

* [Kristoffer Haugsbakk](https://github.com/LemmingAvalanche) &mdash; 
  _Proofreading_
* [Sergei Bukharov](https://github.com/Bukharovsi) &mdash; _PMD enhancements_

All suggestions and ideas welcome!
Please [file an
issue](https://github.com/binkley/modern-java-practices/issues). ☺
