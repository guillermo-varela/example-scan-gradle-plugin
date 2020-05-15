# Examples of Usage for Sonatype Scan Gradle Plugin

This a demo repository showing how to use the [Scan Gradle Plugin](https://github.com/sonatype-nexus-community/scan-gradle-plugin) to analyze open source dependencies in Gradle projects. A dependency with vulnerabilities was added to show how the output looks like.

The file [build.gradle](build.gradle) has a simple Gradle setup applying the plugin. For more options to configure (including usage with credentials) you can refer to the plugin documentation: https://github.com/sonatype-nexus-community/scan-gradle-plugin#how-to-use

## How to use in a multimodule project
Just apply the plugin on the root project and all sub-modules will be processed and the output will be a single report with all components found in each module, no extra configuration is required. This includes Android projects.

## Usage examples
Up next, some examples to configure and run the plugin in several environments or CI tools. Whenever it's possible, first a minimal configuration will be linked to make the plugin run and then a second configuration showing how to leverage cache storing on each CI tool to avoid having to download the Gradle wrapper, dependencies and OSS Index data everytime the job runs.

### Locally
#### OSS Index
Clone this repo on your machine and from the project's root folder run from Terminal/Console:

`./gradlew ossIndexAudit`

The output will have the list of dependencies and the vulnerabilities found for each one (if any):
```
Checking vulnerabilities in 10 artifacts
com.google.j2objc:j2objc-annotations:1.3: 0 vulnerabilities detected
com.h2database:h2:1.4.197: 2 vulnerabilities detected
[CVE-2018-10054]  Improper Input Validation (8.8): https://ossindex.sonatype.org/vuln/b3e3cc9b-766a-4507-9099-51754784a643

[CVE-2018-14335] An issue was discovered in H2 1.4.197. Insecure handling of permissions in the b... (6.5): https://ossindex.sonatype.org/vuln/d3bb2bf8-2080-4598-997a-51bbed9647fb
...
<other dependencies>
...
> Task :ossIndexAudit FAILED
:ossIndexAudit (Thread[Daemon worker,5,main]) completed.

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':ossIndexAudit'.
> Vulnerabilities detected, check log output to review them
```

The task fails if there is at least one dependency with vulnerabilities.

#### Nexus IQ Server
Clone this repo on your machine and from the project's root folder run from Terminal/Console:

`./gradlew nexusIQScan`

The output will have a brief summary of the findings and the URL for the detailed report at IQ Server:
```
Policy evaluation completed in 7 seconds.
Policy Action: None
Number of components affected: 0 critical, 1 severe, 1 moderate
Number of grandfathered policy violations: 0
The detailed report can be viewed online at http://localhost:8070/ui/links/application/appId/report/f43fee82dd9b4566b43c227da2842f83
```

The task only fails if there is a policy violation as a result of this evaluation.

### CircleCI with OSS Index
- Minimal configuration: [.circleci/config.yml](.circleci/config.yml)
- Cache configuration: [.circleci/config-using-cache.yml](.circleci/config-using-cache.yml)

The job output can be seen at: [![CircleCI Build Status](https://circleci.com/gh/guillermo-varela/example-scan-gradle-plugin.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/guillermo-varela/example-scan-gradle-plugin)

### Travis CI with OSS Index
- Minimal configuration: [.travis.yml](.travis.yml)
- Cache configuration: [.travis-cache.yml](.travis-cache.yml)

The job output can be seen at: [![Travis CI Build Status](https://travis-ci.com/guillermo-varela/example-scan-gradle-plugin.svg?branch=master)](https://travis-ci.com/guillermo-varela/example-scan-gradle-plugin)

### GitHub Actions with OSS Index
- Minimal configuration: [.github/workflows/gradle.yml](.github/workflows/gradle.yml)
- Cache configuration: [.github/workflows/gradle-using-cache.yml](.github/workflows/gradle-using-cache.yml)

The workflows output can be seen at: https://github.com/guillermo-varela/example-scan-gradle-plugin/actions

### Jenkins
- Cache configuration: An extra Jenkins plugin is required to use cache, fortunately its docs have an example on how to use it on Gradle projects builds: https://plugins.jenkins.io/jobcacher/

#### OSS Index
Minimal configuration: [Jenkinsfile-ossindex](Jenkinsfile-ossindex)

#### Nexus IQ Server
Minimal configuration: [Jenkinsfile-iq](Jenkinsfile-iq)

## The Fine Print
This repo is meant for informational purposes, copy-paste at your own risk :)
