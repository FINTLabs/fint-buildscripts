# Common buildscripts

Common build scripts that can be used in module by using `apply from: <url to buildscript>`

## xsd.gradle

`apply from: 'https://raw.githubusercontent.com/FINTprosjektet/fint-buildscripts/master/xsd.gradle'`  

The xsd-files are downloaded to `src/main/resources/schema`.  
If the `jaxb-bindings.xml` file is available in `src/main/resources/schema`, it will be used used during the code generation. If no binding-file is in the project, a default `jaxb-bindings.xml` file is downloaded.  
The generated code is stored in `src/main/java` and the generated classes includes `@ToString` from lombok.

| Task | Description |
|------|-------------|
| downloadXsd | Download xsd files. The list of xsd files should be defined in project build.gradle with `project.ext { xsdUrls = [...]  }` |
| processXsd | Generates code from the downloaded xsd files |

Plugin: https://github.com/dmak/jaxb-xew-plugin

## bintray.gradle

```
plugins {
    id "com.jfrog.bintray" version "1.7"
}
...
if (project.hasProperty('bintrayUser') && project.hasProperty('bintrayKey')) {
    apply from: 'https://raw.githubusercontent.com/FINTprosjektet/fint-buildscripts/master/bintray.gradle'
}

```

The `bintray.gradle` file will automatically set group (no.fint) and version (from jar.version).

| Task | Description |
|------|-------------|
| bintrayUpload | Upload artifact to bintray:<br>`./gradlew bintrayUpload -PbintrayUser=<username> -PbintrayKey=<apiKey>` |

## schemagen.gradle

`apply from: 'https://raw.githubusercontent.com/FINTprosjektet/fint-buildscripts/master/schemagen.gradle'`

| Task | Description |
|------|-------------|
| schemagen | Generate xsd file from Java class. The xsd name used for the generated file and the model directory should be defined in project build.gradle:<br> `project.ext { schemagenXsd = '...xsd' schemagenModelDir = 'src/main/java'}` |

## dependencies.gradle

`apply from: 'https://raw.githubusercontent.com/FINTprosjektet/fint-buildscripts/master/dependencies.gradle'`

Common library versions.  
Apply this gradle file to set version number for project dependencies:
`testCompile("org.spockframework:spock-core:${spockSpringVersion}")`  

| Variable name | Version |
|---------------|---------|
| springBootVersion | 1.4.2.RELEASE |
| lombokVersion | 1.16.10 |
| spockSpringVersion | 1.1-groovy-2.4-rc-3 |
