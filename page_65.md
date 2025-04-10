Once completed, you can access a new native dependency in your project.
Additionally, here are some files to look out for:
 ■build.gradle (Project-Level)—located in android/build.gradle, this file configures 
global project settings, including Gradle plugin versions and dependency repositories 
like Maven Central or JitPack.
 ■build.gradle (App-Level)—located in android/app/build.gradle, this file defines 
app-specific dependencies, build configurations, and compile options.
 ■gradle.properties—contains configuration flags and settings to optimize Gradle 
builds (e.g., enabling Hermes or ProGuard).
 ■gradlew & gradlew.bat—the Gradle Wrapper scripts that come with React Native, 
ensuring consistent Gradle versions across all development environments. The .bat file 
is used for Windows systems.
 ■.gradle/ Directory—stores Gradle's cache, compiled scripts, and temporary build 
files. It is generated during the build process and should not be committed to version 
control.
For example, to list available Gradle tasks, you can run ./gradlew tasks.
Building a project
When working with React Native, it's essential to understand how JavaScript, iOS, and Android 
handle code execution and compilation. Unlike native platforms that use compiled languages, 
JavaScript does not have a traditional build system but undergoes a different transformation 
process.
JavaScript
JavaScript is an interpreted language that does not require a separate compilation step like 
Swift or Kotlin. Instead, JavaScript code is executed by an engine at runtime. However, due 
to the multiple ECMAScript versions and evolving JavaScript standards, React Native does 
require a form of  "compilation" known as transpilation to ensure compatibility across different 
environments.
> cd android && ./gradlew clean
React Native projects come with a Gradle Wrapper (gradlew), which ensures all developers 
use the same Gradle version, avoiding compatibility issues. You can run it like this:
Guide: Understand Platform Differences
The Ultimate Guide to React Native Optimization
72