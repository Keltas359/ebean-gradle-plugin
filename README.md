# Credit

Credit goes to khomich for his work https://github.com/khomich/gradle-ebean-enhancer - this plugin is based off that
(with updated enhancement, kapt support etc).

# Documentation

Refer to http://ebean-orm.github.io/docs/tooling/gradle

# ebean-gradle-plugin
Plugin that performs Enhancement (entity, transactional, query bean) and can generate query beans from entity beans written in Kotlin via kapt.

- Add `ebean-gradle-plugin` to buildscript/dependencies/classpath
- Add `apply plugin: 'io.ebean'`
- Add `generated/source/kapt/main` to sourceSets
- Add kapt generateStubs = true
- Add ebean plugin configuration

## Status

Currently using this with entity beans written in Kotlin (rather than Java).  
Tested with java project with gradle sub projects.

## Example build.gradle (kotlin)

```groovy
group 'org.example'
version '1.0-SNAPSHOT'

buildscript {
    ext.kotlin_version = '1.2.0'
    ext.ebean_version = "11.24.1"
    ext.postgresql_driver_version = "9.4.1207.jre7"

    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "io.ebean:ebean-gradle-plugin:11.2.1"
    }
}

apply plugin: 'kotlin'
apply plugin: 'io.ebean'

repositories {
    mavenLocal()
    mavenCentral()
}

sourceSets {
    main.java.srcDirs += [file("$buildDir/generated/source/kapt/main")]
}

dependencies {

    compile "org.postgresql:postgresql:$postgresql_driver_version"
    compile "io.ebean:ebean:$ebean_version"
    compile "io.ebean:ebean-querybean:11.24.1"

    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"

    testCompile "org.avaje.composite:junit:1.1"
}

kapt {
    generateStubs = true
}

ebean {
    debugLevel = 1 //1 - 9
}

test {
    useTestNG()
    testLogging.showStandardStreams = true
    testLogging.exceptionFormat = 'full'
}

```
## Example build.gradle (java)

```groovy
group 'org.example'
version '1.0-SNAPSHOT'

buildscript {

    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "io.ebean:ebean-gradle-plugin:11.12.1"
    }
}

apply plugin: 'io.ebean'

repositories {
    mavenLocal()
    mavenCentral()
}


dependencies {

    compile "io.ebean:ebean:11.24.1"

    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"

    testCompile "org.avaje.composite:junit:1.1"
}

ebean {
    debugLevel = 1 //1 - 9
}


```
