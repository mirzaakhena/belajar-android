<#assign project_id="gs-gradle-android"/>

# Getting Started: Building Android Projects with Gradle

What you'll build
-----------------

This guide walks you through using Gradle to build a simple Android project.


What you'll need
----------------

 - About 15 minutes
 - A favorite text editor or IDE
 - [Android SDK][sdk]
 - An Android device or Emulator

[sdk]: http://developer.android.com/sdk/index.html

## <@how_to_complete_this_guide jump_ahead='Install Gradle'/>


<a name="scratch"></a>
Set up the project
------------------

First, you need to set up an Android project for Gradle to build. To keep the focus on Gradle, make the project as simple as possible for now. If this is your first time working with Android projects, refer to [Getting Started with Android](../gs-android/README.md) to help configure your development environment.

<@create_directory_structure_org_hello/>

<@create_android_manifest/>

    <@snippet path="src/main/AndroidManifest.xml" prefix="initial"/>

### Create a string resource
Add a text string. Text strings can be referenced from the application or from other resource files.

    <@snippet path="src/main/res/values/strings.xml" prefix="initial"/>

### Create a layout
Here you define the visual structure for the user interface of your application.

    <@snippet path="src/main/res/layout/hello_layout.xml" prefix="initial"/>

### Create Java classes

Within the `src/main/java/org/hello` directory, you can create any Java classes you want. To maintain consistency with the rest of this guide, create the following class:

    <@snippet path="src/main/java/org/hello/HelloActivity.java" prefix="initial"/>


<a name="initial"></a>
Install Gradle
--------------

Now that you have a project that you can build with Gradle, you can install Gradle. 

1. Download the latest version of Gradle (1.6 as of this writing) from the [Gradle Downloads] page. 

    > **Note:** Only the binaries are required, so look for the link to `gradle-1.6-bin.zip`. Alternatively, you can choose `gradle-1.6-all.zip` to download the sources and documentation as well as the binaries.

2. Unzip the archive and place it in a location of your choosing. For example, on Linux or Mac, you may want to place it in the root of your user directory. See the [Installing Gradle] page for additional details.

3. Configure the `GRADLE_HOME` environment variable based on the location where you installed Gradle.

    Mac/Linux:

    <#noparse>
    ```sh
    $ export GRADLE_HOME=/<installation location>/gradle-1.6
    $ export PATH=${PATH}:$GRADLE_HOME/bin
    ```
    </#noparse>

    Windows:

    <#noparse>
    ```sh
    set GRADLE_HOME=C:\<installation location>\gradle-1.6
    set PATH=%PATH%;%GRADLE_HOME%\bin
    ```
    </#noparse>

4. Test the Gradle installation with following command:

    ```sh
    $ gradle
    ```

    If the installation is correct, you see a welcome message:

    ```sh
    :help
    
    Welcome to Gradle 1.6.
    
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

    <@snippet path="build.gradle" prefix="initial"/>

This build configuration brings a significant amount of power. Run **gradle tasks** again, and you see new tasks for building the project, creating JavaDoc, and running tests.

The **gradle build** task is one of the most frequently used tasks. This task compiles, tests, and packages the code into an APK file. You can run it like this:

```sh
$ gradle build
```

After a few seconds, you see "BUILD SUCCESSFUL". The complete output can be seen below. 

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

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'complete'.
dependencyInsight - Displays the insight into a specific dependency in root project 'complete'.
help - Displays a help message
projects - Displays the sub-projects of root project 'complete'.
properties - Displays the properties of root project 'complete'.
tasks - Displays the tasks runnable from root project 'complete' (some of the displayed tasks may belong to subprojects).

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

Total time: 6.871 secs
```

You can see results of the build process in the `build` folder. Here you see several folders related to various parts of the build or application. The assembled Android package resides in the `apk` folder. The APK file here is ready to be deployed to a device or emulator.


Declare dependencies
--------------------

The simple Hello World sample is completely self-contained and does not depend on any additional libraries. Most applications, however, depend on external libraries to handle common and/or complex functionality.

For example, suppose you want the application to print the current date and time. You could use the date and time facilities in the native Java libraries, but you can make things more interesting by using the Joda Time libraries.

To do this, modify `HelloActivity.java` to look like this:

    <@snippet path="src/main/java/org/hello/HelloActivity.java" prefix="complete"/>

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

The completed build.gradle now looks like the following:

    <@snippet path="build.gradle" prefix="complete"/>


Summary
-------
Congratulations! You have created a simple yet effective Gradle build file for building Android projects.


[New Build System]:http://tools.android.com/tech-docs/new-build-system
[Gradle]:http://www.gradle.org
[Gradle Downloads]:http://www.gradle.org/downloads
[Installing Gradle]:http://www.gradle.org/docs/current/userguide/installation.html
[Building Android Projects with Maven]:https://github.com/springframework-meta/gs-maven-android
