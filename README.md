# CompositeSubstitution
MCVE of IntelliJ Composite Builds Dependency Substitution issues

## System information
### IntelliJ
```
IntelliJ IDEA 2022.3.1 (Community Edition)
Build #IC-223.8214.52, built on December 20, 2022
Runtime version: 17.0.5+1-b653.23 aarch64
VM: OpenJDK 64-Bit Server VM by JetBrains s.r.o.
macOS 12.6.2
GC: G1 Young Generation, G1 Old Generation
Memory: 2048M
Cores: 10
Metal Rendering is ON
Registry:
    debugger.new.tool.window.layout=true
    ide.experimental.ui=true

Non-Bundled Plugins:
    org.intellij.scala (2022.3.15)
    org.sonarlint.idea (7.3.0.59206)

Kotlin: 223-1.7.21-release-272-IJ8214.52
```
### Gradle
```
$ gradle --version

------------------------------------------------------------
Gradle 7.5.1
------------------------------------------------------------

Build time:   2022-08-05 21:17:56 UTC
Revision:     d1daa0cbf1a0103000b71484e1dbfe096e095918

Kotlin:       1.6.21
Groovy:       3.0.10
Ant:          Apache Ant(TM) version 1.10.11 compiled on July 10 2021
JVM:          17.0.5 (Homebrew 17.0.5+0)
OS:           Mac OS X 12.6.2 aarch64
```

## How to reproduce
Running `gradle assemble` works, and it runs as intended with `java -jar ModuleA/build/libs/ModuleA-1.0-SNAPSHOT.jar` outputting the expected `This is a string!`.

In my "original" project I got weird issues with it not being able to resolve the included dependency in IntelliJ. Everything worked but it couldn't resolve them, meaning everything was red. With this MCVE I got other issues before even reaching there, as seen in the stacktrace below:
```
Exception during working with external system: java.lang.AssertionError
	at org.jetbrains.plugins.gradle.service.project.CommonGradleProjectResolverExtension.createModule(CommonGradleProjectResolverExtension.java:140)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.kotlin.idea.gradleJava.configuration.kpm.KotlinKPMGradleProjectResolver.createModule(KotlinKPMGradleProjectResolver.kt:58)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.kotlin.idea.gradleJava.configuration.KotlinMPPGradleProjectResolver.createModule(KotlinMPPGradleProjectResolver.kt:77)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at com.android.tools.idea.gradle.project.sync.idea.AndroidGradleProjectResolver.createModule(AndroidGradleProjectResolver.kt:204)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.kotlin.idea.gradleJava.configuration.KotlinGradleProjectResolverExtension.createModule(KotlinGradleProjectResolverExtension.kt:188)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.kotlin.android.configure.KotlinAndroidMPPGradleProjectResolver.createModule(KotlinAndroidMPPGradleProjectResolver.kt:62)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.AbstractProjectResolverExtension.createModule(AbstractProjectResolverExtension.java:80)
	at org.jetbrains.plugins.gradle.service.project.TracedProjectResolverExtension.createModule(TracedProjectResolverExtension.java:37)
	at org.jetbrains.plugins.gradle.service.project.GradleProjectResolver.convertData(GradleProjectResolver.java:398)
	at org.jetbrains.plugins.gradle.service.project.GradleProjectResolver.doResolveProjectInfo(GradleProjectResolver.java:317)
	at org.jetbrains.plugins.gradle.service.project.GradleProjectResolver$ProjectConnectionDataNodeFunction.fun(GradleProjectResolver.java:789)
	at org.jetbrains.plugins.gradle.service.project.GradleProjectResolver$ProjectConnectionDataNodeFunction.fun(GradleProjectResolver.java:771)
	at org.jetbrains.plugins.gradle.service.execution.GradleExecutionHelper.lambda$execute$0(GradleExecutionHelper.java:133)
	at org.jetbrains.plugins.gradle.service.execution.GradleExecutionHelper.maybeFixSystemProperties(GradleExecutionHelper.java:159)
	at org.jetbrains.plugins.gradle.service.execution.GradleExecutionHelper.lambda$execute$1(GradleExecutionHelper.java:133)
	at org.jetbrains.plugins.gradle.GradleConnectorService$Companion.withGradleConnection(GradleConnectorService.kt:181)
	at org.jetbrains.plugins.gradle.GradleConnectorService.withGradleConnection(GradleConnectorService.kt)
	at org.jetbrains.plugins.gradle.service.execution.GradleExecutionHelper.execute(GradleExecutionHelper.java:129)
	at org.jetbrains.plugins.gradle.service.project.GradleProjectResolver.resolveProjectInfo(GradleProjectResolver.java:151)
	at org.jetbrains.plugins.gradle.service.project.GradleProjectResolver.resolveProjectInfo(GradleProjectResolver.java:71)
	at com.intellij.openapi.externalSystem.service.remote.RemoteExternalSystemProjectResolverImpl.lambda$resolveProjectInfo$0(RemoteExternalSystemProjectResolverImpl.java:37)
	at com.intellij.openapi.externalSystem.service.remote.AbstractRemoteExternalSystemService.execute(AbstractRemoteExternalSystemService.java:43)
	at com.intellij.openapi.externalSystem.service.remote.RemoteExternalSystemProjectResolverImpl.resolveProjectInfo(RemoteExternalSystemProjectResolverImpl.java:36)
	at com.intellij.openapi.externalSystem.service.remote.wrapper.ExternalSystemProjectResolverWrapper.resolveProjectInfo(ExternalSystemProjectResolverWrapper.java:48)
	at com.intellij.openapi.externalSystem.service.internal.ExternalSystemResolveProjectTask.doExecute(ExternalSystemResolveProjectTask.java:115)
	at com.intellij.openapi.externalSystem.service.internal.AbstractExternalSystemTask.execute(AbstractExternalSystemTask.java:151)
	at com.intellij.openapi.externalSystem.service.internal.AbstractExternalSystemTask.execute(AbstractExternalSystemTask.java:135)
	at com.intellij.openapi.externalSystem.util.ExternalSystemUtil$2.executeImpl(ExternalSystemUtil.java:498)
	at com.intellij.openapi.externalSystem.util.ExternalSystemUtil$2.lambda$execute$0(ExternalSystemUtil.java:329)
	at com.intellij.openapi.project.DumbServiceHeavyActivities.suspendIndexingAndRun(DumbServiceHeavyActivities.java:21)
	at com.intellij.openapi.project.DumbServiceImpl.suspendIndexingAndRun(DumbServiceImpl.java:186)
	at com.intellij.openapi.externalSystem.util.ExternalSystemUtil$2.execute(ExternalSystemUtil.java:329)
	at com.intellij.openapi.externalSystem.util.ExternalSystemUtil$4.run(ExternalSystemUtil.java:619)
	at com.intellij.openapi.progress.impl.CoreProgressManager.startTask(CoreProgressManager.java:423)
	at com.intellij.openapi.progress.impl.ProgressManagerImpl.startTask(ProgressManagerImpl.java:114)
	at com.intellij.openapi.progress.impl.CoreProgressManager.lambda$runProcessWithProgressAsynchronously$6(CoreProgressManager.java:474)
	at com.intellij.openapi.progress.impl.ProgressRunner.lambda$submit$3(ProgressRunner.java:252)
	at com.intellij.openapi.progress.impl.CoreProgressManager.lambda$runProcess$2(CoreProgressManager.java:188)
	at com.intellij.openapi.progress.impl.CoreProgressManager.lambda$executeProcessUnderProgress$13(CoreProgressManager.java:589)
	at com.intellij.openapi.progress.impl.CoreProgressManager.registerIndicatorAndRun(CoreProgressManager.java:664)
	at com.intellij.openapi.progress.impl.CoreProgressManager.computeUnderProgress(CoreProgressManager.java:620)
	at com.intellij.openapi.progress.impl.CoreProgressManager.executeProcessUnderProgress(CoreProgressManager.java:588)
	at com.intellij.openapi.progress.impl.ProgressManagerImpl.executeProcessUnderProgress(ProgressManagerImpl.java:60)
	at com.intellij.openapi.progress.impl.CoreProgressManager.runProcess(CoreProgressManager.java:175)
	at com.intellij.openapi.progress.impl.ProgressRunner.lambda$submit$4(ProgressRunner.java:252)
	at java.base/java.util.concurrent.CompletableFuture$AsyncSupply.run(CompletableFuture.java:1768)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1$1.run(Executors.java:702)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1$1.run(Executors.java:699)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:399)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1.run(Executors.java:699)
	at java.base/java.lang.Thread.run(Thread.java:833)
```

## Other Information
Now using Composite Builds together with Dependency Substitution might not be the norm, but as it works from commandline I would assume that this should work in IntelliJ as well. As for the use-case we have, it's that we have some logic that is shared between our `buildSrc` and actual runtime logic in the application in regards to how we package some plugins.