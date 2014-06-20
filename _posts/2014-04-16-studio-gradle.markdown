---
layout: post
title:  "Android Studio Gradle使用常用技巧"
date:   2014-04-16 21:29:43
categories:  Gradle
---


#1.自动签名
{% highlight ruby %}
signingConfigs {
    releaseConfig {
        storeFile file("/path/to/your/keystore")
        storePassword ('blabla')
        keyAlias 'blabla'
        keyPassword 'blabla'
    }
}

buildTypes {
    release {
        signingConfig signingConfigs.releaseConfig
    }
}
{% endhighlight %}


#2.加入混淆
{% highlight ruby %}
buildTypes {
    release {
        runProguard true
        proguardFile file('proguard-project.txt')
    }
}
{% endhighlight %}

#3.对Release和Debug模式配置不同的AndroidManifest.xml
{% highlight ruby %}
debug {
    manifest.srcFile 'AndroidManifest.xml'
}
release {
    manifest.srcFile 'AndroidManifestRelease.xml'
}
{% endhighlight %}

#4.JNI so库处理
{% highlight ruby %}
task copyNativeLibs(type: Copy) {
    from fileTree(dir: 'libs', include: '**/*.so' ) into 'build/native-libs'
}
tasks.withType(Compile) {
    compileTask -> compileTask.dependsOn copyNativeLibs
}
clean.dependsOn 'cleanCopyNativeLibs'
tasks.withType(com.android.build.gradle.tasks.PackageApplication) {
    pkgTask -> pkgTask.jniFolders = new HashSet()
        pkgTask.jniFolders.add(new File(projectDir, 'build/native-libs'))
}
{% endhighlight %}

#然后执行
{% highlight ruby %}
 ./gradlew assembleRelease
{% endhighlight %}


#Gradle Tasks
{% highlight ruby %}
cd myproject/
gradle <taskName1> [<taskName2>...]
gradle tasks [--all]
{% endhighlight %}
