# Building Android Projects with Gradle

This Getting Started guide will walk you through a few basic steps of setting up and using Gradle to build an Android application.


## Downloads

Within this repository are two directories, [`complete`] and [`start`]. [`complete`] contains the full working copy of the code being demonstrated in this guide. [`start`] contains the initial project setup, but otherwise empty code files. You may use the [`start`] folder as a way to quickly begin working through the implementation steps.

Using [git], clone the repository with the following command:

```sh
$ git clone https://github.com/springframework-meta/gs-template.git
```

> Note: if you are unfamiliar with [git], then you may want to try the [GitHub for Mac] or [GitHub for Windows] clients. 

In both cases, you will find a simple Android project that we will build. Our focus will be on the build.gradle and gradle.settings files. The Java files are only there as source for our build efforts.

We will start by installing Gradle and then populate the `build.gradle` file. If you already have Gradle installed, then you can jump to the [fun part](#finding-out-what-gradle-can-do).


## Installing the Android Development Environment

Building Android applications requires the installation of the [Android SDK].

### Install the Android SDK

1. Download the correct version of the [Android SDK] for your operating system from the Android web site.

2. Unzip the archive and place it in a location of your choosing. For example on Linux or Mac, you may want to place it in the root of your user directory. See the [Android Developers] web site for additional installation details.

3. Configure the `ANDROID_HOME` environment variable based on the location where you installed the Android SDK. Additionally, you should consider adding `ANDROID_HOME/tools` and `ANDROID_HOME/platform-tools` to your PATH.

	Mac OS X:

	```sh
	$ export ANDROID_HOME=/<installation location>/android-sdk-macosx
    $ export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
    ```
    
    Linux:
    
    ```sh
    $ export ANDROID_HOME=/<installation location>/android-sdk-linux
    $ export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
    ```
	    
    Windows:
    
    ```sh
    set ANDROID_HOME=C:\<installation location>\android-sdk-windows
    set PATH=%PATH%;%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools
    ```

4. Once the SDK is installed, we need to add the relevant [Platforms and Packages]. We are using Android 4.2.2 (API 17) in this guide.

### Install Android SDK Platforms and Packages

The Android SDK download does not include any specific Android platform SDKs. In order to run the sample code you need to download and install the latest SDK Platform. You accomplish this by using the Android SDK and AVD Manager that was installed from the previous step.

1. Open the Android SDK Manager window:

	```sh
	$ android
	```

	> Note: if this command does not open the Android SDK Manager, then your path is not configured correctly.
	
2. Select the checkbox for *Tools*

3. Select the checkbox for the latest Android SDK, "Android 4.2.2 (API 17)" as of this writing

4. Select the checkbox for the *Android Support Library* from the *Extras* folder

5. Click the **Install packages...** button to complete the download and installation.

	> Note: you may want to simply install all the available updates, but be aware it will take longer, as each SDK level is a sizable download.


## Installing Gradle

The "[New Build System]" for Android is only supported on Gradle 1.3 or 1.4. 

1. Download Gradle 1.4 from the [Gradle Downloads] page

	> Note: Only the binaries are required, so look for the link to `gradle-1.4-bin.zip`. Alternatively, you may choose `gradle-1.4-all.zip` to download the sources and documentation as well as the binaries.

2. Unzip the archive and place it in a location of your choosing. For example on Linux or Mac, you may want to place it in the root of your user directory. See the [Installing Gradle] page for additional details.

3. Configure the `GRADLE_HOME` environment variable based on the location where you installed Gradle.

	Mac/Linux:

	```sh
	$ export GRADLE_HOME=/<installation location>/gradle-1.4
    $ export PATH=${PATH}:$GRADLE_HOME/bin
    ```
    	    
    Windows:
    
    ```sh
    set GRADLE_HOME=C:\<installation location>\gradle-1.4
    set PATH=%PATH%;%GRADLE_HOME%\bin
    ```

4. Test the Gradle installation by running the following command:

	```sh
	$ gradle
	```

	If the installation is correct, we will see a welcome message that looks something like the following:

	```sh
	:help

	Welcome to Gradle 1.4.

	To run a build, run gradle <task> ...

	To see a list of available tasks, run gradle tasks

	To see a list of command-line options, run gradle --help

	BUILD SUCCESSFUL

	Total time: 5.588 secs 
	```

Congratulations! You now have Gradle installed.


## Finding Out What Gradle Can Do

Now that Gradle is installed, we can explore some of its functionality. Run the following command to view a list of available tasks:

```sh
$ gradle tasks
```

We will see a list of tasks that are available in Gradle. Assuming we run Gradle in a folder where there is not already a `build.gradle` file, we will see some very elementary tasks such as this:

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

Even though these tasks are available, they do not offer much value without a project build configuration, however as we complete our `build.gradle` file, some of these tasks will be more useful. The list of tasks will grow as we add plugins to our `build.gradle` file, so you may want to run `gradle tasks` again to see what tasks are available.


## Building Android Code

The most simple Android project has the following `build.gradle`:

```groovy
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.3'
    }
}

apply plugin: 'android'

android {
    compileSdkVersion 17
}
```

This build configuration brings a significant amount of power. If you run `gradle tasks` again, we see some new tasks added to the list, including tasks for building the project, creating JavaDoc, and running tests.

The `build` task is probably one of the tasks we use most often with Gradle. This task compiles, tests, and packages the code into an APK file. We can run it like this:

```sh
$ gradle build
```

After a few seconds, we see the words "BUILD SUCCESSFUL". The complete output can be seen below. 

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

Total time: 6.476 secs
```

The results of the build process can be found in the *build* folder. Here we see several folders related to various parts of the build or application. The assembled Android package can be found in the *apk* folder. The APK file found here is ready to be deployed to a device or emulator


## Declaring Dependencies

Our simple Hello World sample is completely self-contained and does not depend on any additional libraries. Most application, however, depend on external libraries to handle common and/or complex functionality.

For example, suppose we want our application to print the current date and time. While we could use the date and time facilities in the native Java libraries, we can make things more interesting by using the Joda Time libraries.

To do this, modify HelloActivity.java to look like this:

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
		setContentView(R.layout.main);

		TextView textView = (TextView) findViewById(R.id.text_view);

		LocalTime currentTime = new LocalTime();
		textView.setText("The current local time is: " + currentTime);
	}

}
```

In this example, we are using Joda Time's `LocalTime` class to retrieve and display the current time. 

If we were to run `gradle build` to build our project now, the build would fail because we have not declared Joda Time as a compile dependency in our build. We can fix that by adding the following lines to *build.gradle*:

```groovy
repositories { mavenCentral() }
dependencies {
  compile "joda-time:joda-time:2.2"
}
```

The first line here indicates that our build should resolve its dependencies from the Maven Central repository. Gradle leans heavily on many conventions and facilities established by the Maven build tool, including the option of using Maven Central as a source of library dependencies.

Within the `dependencies` block, we are declaring a single dependency for Joda Time. Specifically, we are asking for (reading right to left) version 2.2 of the *joda-time* library, in the *joda-time* group. 

Another thing to note about this dependency is that it is a `compile` dependency, indicating that it should be available during compile-time. Now if we run `gradle build`, Gradle should resolve the Joda Time dependency from the Maven Central repository and the build will be successful.


## Next Steps

Congratulations! You have now created a very simple, yet effective Gradle build file for building Android projects.

There's much more to building projects with Gradle. For continued exploration of Gradle, you may want to have a look at the following Getting Started guides:

* Building web applications with Gradle
* Setting Gradle properties
* Using the Gradle wrapper

And for an alternate approach to building projects, you may want to look at [Building Android Projects with Maven].



[`complete`]:complete/
[`start`]:start/
[git]:http://git-scm.com
[GitHub for Mac]:http://mac.github.com
[GitHub for Windows]:http://windows.github.com
[Android SDK]: http://developer.android.com/sdk/index.html
[Android Developers]:http://developer.android.com/sdk/installing/index.html
[Platforms and Packages]:http://developer.android.com/sdk/installing/adding-packages.html
[New Build System]:http://tools.android.com/tech-docs/new-build-system
[Gradle]:http://www.gradle.org
[Gradle Downloads]:http://www.gradle.org/downloads
[Installing Gradle]:http://www.gradle.org/docs/current/userguide/installation.html
[Building Android Projects with Maven]:https://github.com/springframework-meta/gs-maven-android
