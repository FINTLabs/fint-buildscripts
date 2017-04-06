# Common buildscripts

Common build scripts that can be used in module by using `apply from: <url to buildscript>`  
Example: `https://raw.githubusercontent.com/FINTlibs/fint-buildscripts/v1.0.3/dependencies.gradle`  

* [xsd](#xsdgradle)
* [bintray](#bintraygradle)
* [schemagen](#schemagengradle)
* [dependencies](#dependenciesgradle)
* [dependencyReport](#dependencyreportgradle)
* [version](#versiongradle)

---

## xsd.gradle

`apply from: 'https://raw.githubusercontent.com/FINTlibs/fint-buildscripts/<release-version>/xsd.gradle'`  

The xsd-files are downloaded to `src/main/resources/schema`.  
If the `jaxb-bindings.xml` file is available in `src/main/resources/schema`, it will be used used during the code generation. If no binding-file is in the project, a default `jaxb-bindings.xml` file is downloaded.  
The generated code is stored in `src/main/java` and the generated classes includes `@ToString` from lombok.

| Task | Description |
|------|-------------|
| downloadXsd | Download xsd files. The list of xsd files should be defined in project build.gradle with `project.ext { xsdUrls = [...]  }` |
| processXsd | Generates code from the downloaded xsd files |

Plugin: https://github.com/dmak/jaxb-xew-plugin

## bintray.gradle

```groovy
plugins {
    id "com.jfrog.bintray" version "1.7"
}
...
if (project.hasProperty('bintrayUser') && project.hasProperty('bintrayKey')) {
    apply from: 'https://raw.githubusercontent.com/FINTlibs/fint-buildscripts/<release-version>/bintray.gradle'
}

```

The `bintray.gradle` file will automatically set group (no.fint) and version (from jar.version).

| Task | Description |
|------|-------------|
| bintrayUpload | Upload artifact to bintray:<br>`./gradlew bintrayUpload -PbintrayUser=<username> -PbintrayKey=<apiKey>` |

## schemagen.gradle

`apply from: 'https://raw.githubusercontent.com/FINTlibs/fint-buildscripts/<release-version>/schemagen.gradle'`

| Task | Description |
|------|-------------|
| schemagen | Generate xsd file from Java class. The xsd name used for the generated file and the model directory should be defined in project build.gradle:<br> `project.ext { schemagenXsd = '...xsd' schemagenModelDir = 'src/main/java'}` |

## dependencies.gradle

`apply from: 'https://raw.githubusercontent.com/FINTlibs/fint-buildscripts/<release-version>/dependencies.gradle'`

Common library versions.  
Apply this gradle file to set version number for project dependencies:  
`testCompile("org.spockframework:spock-core:${spockSpringVersion}")`  

| Variable name | Version |
|---------------|---------|
| gradleVersion | 3.4 |
| springBootVersion | 1.5.1.RELEASE |
| springfoxLoaderVersion |0.1.2 |
| lombokVersion | 1.16.14 |
| spockSpringVersion | 1.0-groovy-2.4 |

To set gradle version:  
```groovy
task wrapper(type: Wrapper) {
    gradleVersion = gradleVersion
}
```

## dependencyReport.gradle

```groovy
apply plugin: 'project-report'
```

[project-reports](https://docs.gradle.org/current/userguide/project_reports_plugin.html)

`apply from: 'https://raw.githubusercontent.com/FINTlibs/fint-buildscripts/<release-version>/dependencyReport.gradle'`

Copies the dependency report into the generated jar file.
The file dependencies.txt will be available on the classpath.

## version.gradle

```groovy
buildscript {
    ...
    dependencies {
        classpath 'org.ajoberstar:gradle-git:1.5.1'
    }
}
```

[Grgit usage](https://github.com/ajoberstar/gradle-git/wiki/Grgit-Usage)

`apply from: 'https://raw.githubusercontent.com/FINTlibs/fint-buildscripts/<release-version>/version.gradle'`

Use `createVersion()` to generate the version number for the project.  
The format is `yyyyMMdd-HHmmss.abbreviatedCommitHash`, reference: https://blog.philipphauer.de/version-numbers-continuous-delivery-maven-docker/

```java
jar {
    version = createVersion()
    baseName = '<project-name>'
}
```