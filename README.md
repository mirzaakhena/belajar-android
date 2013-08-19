This guide walks you through the process of using Gradle to build a simple Android project.

What you'll build
-----------------

You'll create a simple Spring app for Android and then build it with Gradle.


What you'll need
----------------

 - About 15 minutes
 - A favorite text editor or IDE
 - [Android SDK][sdk]
 - An Android device or Emulator

[sdk]: http://developer.android.com/sdk/index.html

How to complete this guide
--------------------------

Like all Spring's [Getting Started guides](/guides/gs), you can start from scratch and complete each step, or you can bypass basic setup steps that are already familiar to you. Either way, you end up with working code.

To **start from scratch**, move on to [Set up the project](#scratch).

To **skip the basics**, do the following:

 - [Download][zip] and unzip the source repository for this guide, or clone it using [Git][u-git]:
`git clone https://github.com/springframework-meta/gs-gradle-android.git`
 - cd into `gs-gradle-android/initial`.
 - Jump ahead to [Install Gradle](#initial).

**When you're finished**, you can check your results against the code in `gs-gradle-android/complete`.
[zip]: https://github.com/springframework-meta/gs-gradle-android/archive/master.zip
[u-git]: /understanding/Git


<a name="scratch"></a>
Set up the project
------------------

First, you need to set up an Android project for Gradle to build. To keep the focus on Gradle, make the project as simple as possible for now. If this is your first time working with Android projects, refer to [Getting Started with Android][gs-android] to help configure your development environment.

### Create the directory structure

In a project directory of your choosing, create the following subdirectory structure; for example, with the following command on Mac or Linux:

```sh
$ mkdir -p src/main/java/org/hello
```

    └── src
        └── main
            └── java
                └── org
                    └── hello

### Create an Android manifest

The [Android Manifest] contains all the information required to run an Android application, and it cannot build without one.

[Android Manifest]: http://developer.android.com/guide/topics/manifest/manifest-intro.html

`src/main/AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="org.hello"
    android:versionCode="1"
    android:versionName="1.0.0" >

    <application android:label="@string/app_name" >
        <activity
            android:name=".HelloActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

### Create a string resource
Add a text string. Text strings can be referenced from the application or from other resource files.

`src/main/res/values/strings.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">Android Gradle</string>
</resources>
```

### Create a layout
Here you define the visual structure for the user interface of your application.

`src/main/res/layout/hello_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView  
    android:id="@+id/text_view"
    android:layout_width="fill_parent" 
    android:layout_height="wrap_content" 
    />
</LinearLayout>
```

### Create Java classes

Within the `src/main/java/org/hello` directory, you can create any Java classes you want. To maintain consistency with the rest of this guide, create the following class:

`src/main/java/org/hello/HelloActivity.java`
```java
package org.hello;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class HelloActivity extends Activity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.hello_layout);
    }

    @Override
    public void onStart() {
        super.onStart();
        TextView textView = (TextView) findViewById(R.id.text_view);
        textView.setText("Hello world!");
    }

}
```


<a name="initial"></a>
Install Gradle
--------------

Now that you have a project that you can build with Gradle, you can install Gradle. 

1. Download the latest version of Gradle (1.7 as of this writing) from the [Gradle Downloads] page.

    > **Note:** Only the binaries are required, so look for the link to `gradle-1.7-bin.zip`. Alternatively, you can choose `gradle-1.7-all.zip` to download the sources and documentation as well as the binaries.

2. Unzip the archive and place it in a location of your choosing. For example, on Linux or Mac, you may want to place it in the root of your user directory. See the [Installing Gradle] page for additional details.

3. Configure the `GRADLE_HOME` environment variable based on the location where you installed Gradle.

    Mac/Linux:

    ```sh
    $ export GRADLE_HOME=/<installation location>/gradle-1.7
    $ export PATH=${PATH}:$GRADLE_HOME/bin
    ```

    Windows:

    ```sh
    set GRADLE_HOME=C:\<installation location>\gradle-1.7
    set PATH=%PATH%;%GRADLE_HOME%\bin
    ```

4. Test the Gradle installation with following command:

    ```sh
    $ gradle
    ```

    If the installation is correct, you see a welcome message:

    ```sh
    :help
    
    Welcome to Gradle 1.7.
    
    To run a build, run gradle <task> ...
    
    To see a list of available tasks, run gradle tasks
    
    To see a list of command-line options, run gradle --help
    
    BUILD SUCCESSFUL
    
    Total time: 2.923 secs 
    ```

You now have Gradle installed.


Find out what Gradle can do
---------------------------

Before you even create a build.gradle file for the project, you can ask Gradle what tasks are available:

```sh
$ gradle tasks
```

You should see a list of available tasks. Assuming you run Gradle in a folder that does not contain a `build.gradle` file, you see basic tasks such as the following:

```sh
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'gs-gradle-android'.
dependencyInsight - Displays the insight into a specific dependency in root project 'gs-gradle-android'.
help - Displays a help message
projects - Displays the sub-projects of root project 'gs-gradle-android'.
properties - Displays the properties of root project 'gs-gradle-android'.
tasks - Displays the tasks runnable from root project 'gs-gradle-android' (some of the displayed tasks may belong to subprojects).

To see all tasks and more detail, run with --all.

BUILD SUCCESSFUL

Total time: 2.706 secs
```

Even though these tasks are available, they do not offer much value without a project build configuration. As you flesh out the `build.gradle` file, some of these tasks become more useful. The list of tasks will grow as you add plugins to the `build.gradle` file. You can run **gradle tasks** again to see what new tasks are available.


Build Android code
------------------

The most simple Android project has the following `build.gradle` file:

`build.gradle`
```gradle
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.5.6'
    }
}

apply plugin: 'android'

android {
    buildToolsVersion "18.0.1"
    compileSdkVersion 18
}
```

This build configuration brings a significant amount of power. Run **gradle tasks** again, and you see new tasks for building the project, creating JavaDoc, and running tests. The complete output can be seen below:

```sh
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Android tasks
-------------
androidDependencies - Displays the Android dependencies of the project
signingReport - Displays the signing info for each variant

Build tasks
-----------
assemble - Assembles all variants of all applications and secondary packages.
assembleDebug - Assembles all Debug builds
assembleRelease - Assembles all Release builds
assembleTest - Assembles the Test build for the Debug build
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
clean - Deletes the build directory.

Build Setup tasks
-----------------
setupBuild - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'initial'.
dependencyInsight - Displays the insight into a specific dependency in root project 'initial'.
help - Displays a help message
projects - Displays the sub-projects of root project 'initial'.
properties - Displays the properties of root project 'initial'.
tasks - Displays the tasks runnable from root project 'initial'.

Install tasks
-------------
installDebug - Installs the Debug build
installTest - Installs the Test build for the Debug build
uninstallAll - Uninstall all applications.
uninstallDebug - Uninstalls the Debug build
uninstallRelease - Uninstalls the Release build
uninstallTest - Uninstalls the Test build for the Debug build

Verification tasks
------------------
check - Runs all checks.
connectedCheck - Runs all device checks on currently connected devices.
connectedInstrumentTest - Installs and runs the tests for Build 'Debug' on connected devices.
deviceCheck - Runs all device checks using Device Providers and Test Servers.

Rules
-----
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.
Pattern: clean<TaskName>: Cleans the output files of a task.

To see all tasks and more detail, run with --all.

BUILD SUCCESSFUL

Total time: 7.007 secs
```

You'll use the **gradle build** task frequently. This task compiles, tests, and packages the code into an APK file. You can run it like this:

```sh
$ gradle build
```

After a few seconds, you see "BUILD SUCCESSFUL". The complete output can be seen below:

```sh
:prepareDebugDependencies
:compileDebugAidl UP-TO-DATE
:generateDebugBuildConfig UP-TO-DATE
:mergeDebugAssets UP-TO-DATE
:compileDebugRenderscript UP-TO-DATE
:mergeDebugResources UP-TO-DATE
:processDebugManifest UP-TO-DATE
:processDebugResources UP-TO-DATE
:compileDebug UP-TO-DATE
:dexDebug UP-TO-DATE
:processDebugJavaRes UP-TO-DATE
:validateDebugSigning
:packageDebug UP-TO-DATE
:assembleDebug UP-TO-DATE
:prepareReleaseDependencies
:compileReleaseAidl UP-TO-DATE
:generateReleaseBuildConfig UP-TO-DATE
:mergeReleaseAssets UP-TO-DATE
:compileReleaseRenderscript UP-TO-DATE
:mergeReleaseResources UP-TO-DATE
:processReleaseManifest UP-TO-DATE
:processReleaseResources UP-TO-DATE
:compileRelease UP-TO-DATE
:dexRelease UP-TO-DATE
:processReleaseJavaRes UP-TO-DATE
:packageRelease UP-TO-DATE
:assembleRelease UP-TO-DATE
:assemble UP-TO-DATE
:check UP-TO-DATE
:build UP-TO-DATE

BUILD SUCCESSFUL

Total time: 6.944 secs
```

You can see results of the build process in the `build` folder. Here you see several folders related to various parts of the build or application. The assembled Android package resides in the `apk` folder. The APK file here is ready to be deployed to a device or emulator.


Declare dependencies
--------------------

The simple Hello World sample is completely self-contained and does not depend on any additional libraries. Most applications, however, depend on external libraries to handle common and/or complex functionality.

For example, suppose you want the application to print the current date and time. You could use the date and time facilities in the native Java libraries, but you can make things more interesting by using the Joda Time libraries.

To do this, modify `HelloActivity.java` to look like this:

`src/main/java/org/hello/HelloActivity.java`
```java
package org.hello;

import org.joda.time.LocalTime;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class HelloActivity extends Activity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.hello_layout);
    }

    @Override
    public void onStart() {
        super.onStart();
        LocalTime currentTime = new LocalTime();
        TextView textView = (TextView) findViewById(R.id.text_view);
        textView.setText("The current local time is: " + currentTime);
    }

}
```

In this example, we are using Joda Time's `LocalTime` class to retrieve and display the current time. 

If you ran `gradle build` to build the project now, the build would fail because you have not declared Joda Time as a compile dependency in the build. You can fix that by adding the following lines to `build.gradle`:

```groovy
repositories { mavenCentral() }
dependencies {
    compile "joda-time:joda-time:2.2"
}
```

The first line here indicates that the build should resolve its dependencies from the Maven Central repository. Gradle leans heavily on many conventions and facilities established by the Maven build tool, including the option of using Maven Central as a source of library dependencies.

Within the `dependencies` block, you declare a single dependency for Joda Time. Specifically, you are asking for (reading right to left) version 2.2 of the joda-time library, in the joda-time group. 

Another thing to note about this dependency is that it is a `compile` dependency, indicating that it should be available during compile-time. Now if you run `gradle build`, Gradle should resolve the Joda Time dependency from the Maven Central repository and the build will be successful.


Build your project with Gradle Wrapper
--------------------------------------

The Gradle Wrapper is the preferred way of starting a Gradle build. It consists of a batch script for Windows support and a shell script for support on OS X and Linux. These scripts allow you to run a Gradle build without requiring that Gradle be installed on your system. You can install the wrapper into your project by adding the following lines to the build.gradle:

```groovy
task wrapper(type: Wrapper) {
    gradleVersion = '1.7'
}
```

Run the following command to download and initialize the wrapper scripts:

```sh
$ gradle wrapper
```

After this task completes, you will notice a few new files. The two scripts are in the root of the folder, while the wrapper jar and properties files have been added to a new `gradle/wrapper` folder.

    └── initial
        └── gradlew
        └── gradlew.bat
        └── gradle
            └── wrapper
                └── gradle-wrapper.jar
                └── gradle-wrapper.properties

The Gradle Wrapper is now available for building your project. It can be used in the exact same way as an installed version of Gradle. Run the wrapper script to perform the build task, just like you did previously:

```sh
$ ./gradlew build
```

The first time you run the wrapper for a specified version of Gradle, it downloads and caches the Gradle binaries for that version. The Gradle Wrapper files are designed to be committed to source control so that anyone can build the project without having to first install and configure a specific version of Gradle.

Here is the completed `build.gradle` file:

`build.gradle`
```gradle
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.5.6'
    }
}

apply plugin: 'android'

android {
    buildToolsVersion "18.0.1"
    compileSdkVersion 18
}

repositories { 
    mavenCentral() 
}

dependencies {
    compile("joda-time:joda-time:2.2")
}

task wrapper(type: Wrapper) {
    gradleVersion = '1.7'
}
```



Summary
-------
Congratulations! You have created a simple yet effective Gradle build file for building Android projects.


[New Build System]:http://tools.android.com/tech-docs/new-build-system
[Gradle]:http://www.gradle.org
[Gradle Downloads]:http://www.gradle.org/downloads
[Installing Gradle]:http://www.gradle.org/docs/current/userguide/installation.html
[Building Android Projects with Maven]:https://github.com/springframework-meta/gs-maven-android
[gs-android]: /guides/gs/android
