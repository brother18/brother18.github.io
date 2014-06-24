---
layout: post
title:  "Gradle模板"
date:   2014-06-24 21:29:43
categories:  Gradle
---
#1. 基本模板
{% highlight ruby %}
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:0.11.+'
    }
}
apply plugin: 'android'

dependencies {
    compile fileTree(dir: 'libs', include: '*.jar')
}

android {
    compileSdkVersion 19
    buildToolsVersion "19.1.0"
    defaultConfig {
        versionCode 0
        versionName "0.1.0"
    }
    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDirs = ['src']
            resources.srcDirs = ['src']
            aidl.srcDirs = ['src']
            renderscript.srcDirs = ['src']
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
        }

        // Move the tests to tests/java, tests/res, etc...
        instrumentTest.setRoot('tests')

        // Move the build types to build-types/<type>
        // For instance, build-types/debug/java, build-types/debug/AndroidManifest.xml, ...
        // This moves them out of them default location under src/<type>/... which would
        // conflict with src/ being used by the main source set.
        // Adding new build types or product flavors should be accompanied
        // by a similar customization.
        debug.setRoot('build-types/debug')
        release.setRoot('build-types/release')
    }

    signingConfigs {
        release {
            storeFile file(RELEASE_STORE_FILE)
            storePassword RELEASE_STORE_PASSWORD
            keyAlias RELEASE_KEY_ALIAS
            keyPassword RELEASE_KEY_PASSWORD
        }
    }

    buildTypes {
        debug {
            applicationIdSuffix ".d"
            versionNameSuffix "-debug"
        }

        release {
            runProguard true
            proguardFile file('proguard-project.txt')
            signingConfig signingConfigs.release
        }

        beta.initWith(buildTypes.release)
        beta {
            applicationIdSuffix ".beta"
            versionNameSuffix "-beta"
            debuggable true
        }
        alpha.initWith(buildTypes.release)
        alpha {
            applicationIdSuffix ".alpha"
            versionNameSuffix "-alpha"
            debuggable true
        }
    }
}

new ByteArrayOutputStream().withStream { os->
    def result = exec {
        executable = 'git'
        args = ['rev-parse', '--short', 'HEAD']
        standardOutput = os
    }
    project.ext.gitHash = os.toString().trim();
}

android.applicationVariants.all { variant ->
    def versionSuffix = variant.buildType.versionNameSuffix ? variant.buildType.versionNameSuffix : ""
    def versionName = variant.mergedFlavor.versionName + versionSuffix + "-${gitHash}";
    def apk = variant.outputFile;
    def newName = apk.name.replace(variant.buildType.name, versionName);
    variant.outputFile = new File(apk.parentFile, newName);
}
{% endhighlight %}


在~/.gradle/gradle.properties中添加如下信息
{% highlight ruby %}
RELEASE_STORE_FILE=/path/to/your/keystore
RELEASE_STORE_PASSWORD=foo
RELEASE_KEY_ALIAS=foo
RELEASE_KEY_PASSWORD=foo
{% endhighlight %}

# 2.多工程
修改settings.gradle
{% highlight ruby %}
include ':app'
include ':library:volley'
{% endhighlight %}

修改build.gradle
{% highlight ruby %}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile project(':library:volley')
}
{% endhighlight %}

# 3.Product Falvor   
很简单,添加如下即可
{% highlight ruby %}
productFlavors {
    freeFlavor {
        applicationId "demo.cvte.com.gradledemoproductflavor.free"
    }

    paidFlavor {
        applicationId "demo.cvte.com.gradledemoproductflavor.paid"
    }
}
{% endhighlight %}

