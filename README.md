## ***v2flags*** is a nicer set of arguments for slightly better JVM performance, especially for games like Minecraft.

It is suggested to use the [GraalVM 25 JDK](https://www.graalvm.org/downloads/#) to run your java application for additional performance gains.

### ***Server:***

Use these flags on a Server, These flags have been rigorously tested on the now defunct Protobit Vanilla+ (which was very heavy to run and unoptimized) & a future project of mine while performing pretty well.

You can either paste these flags into your existing startup script, or use the provided start scripts for [Linux](https://github.com/Cooperly/v2flags/releases/download/v1.1/linux.sh) or [Windows](https://github.com/Cooperly/v2flags/releases/download/v1.1/windows.bat).

*Don't forget to adjust -Xms and -Xmx to your server configuration, along with java path and server.jar (necessary for Fabric/Quilt) if applicable*

```java
java -Xms8G -Xmx8G -XX:+UseZGC -XX:-ZUncommit -XX:-ZProactive -XX:+UseCompactObjectHeaders -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+DisableExplicitGC -XX:+UseNUMA -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -XX:+ParallelRefProcEnabled -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseFMA -XX:+AlwaysPreTouch -XX:+UseTransparentHugePages -XX:+UseLargePages -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:AllocatePrefetchStyle=3 -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -DPaper.IgnoreJavaVersion=true -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar server.jar –-nogui
```

### ***Server, Pterodactyl:***

Use these flags on a server running via Pterodactyl Panel or any panel that accepts Pterodactyl Eggs, You will either need admin panel access in order to import this egg, or to contact your host's Support Team.

Get the [GraalVM Template](https://github.com/Cooperly/v2flags/releases/download/v1.0e/Pterodactyl.json) egg within releases and download your server.jar of choice to your server's files.

Usually the CUTDOWN value can be left alone, but if your server hangs unexpectedly or abruptly stops, increase this value to reserve more memory to other processes like the OS.

```java
java -Xms$(({{SERVER_MEMORY}} - ({{CUTDOWN}})))M -Xmx$(({{SERVER_MEMORY}} - ({{CUTDOWN}})))M -XX:+UseZGC -XX:-ZUncommit -XX:-ZProactive -XX:+UseCompactObjectHeaders -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+DisableExplicitGC -XX:+UseNUMA -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -XX:+ParallelRefProcEnabled -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseFMA -XX:+AlwaysPreTouch -XX:+UseTransparentHugePages -XX:+UseLargePages -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:AllocatePrefetchStyle=3 -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -DPaper.IgnoreJavaVersion=true -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar {{SERVER_JARFILE}} –-nogui
```

### ***Client:***

Use these flags on a Client, These flags have been tested on my own modded client (v2client) and generally performs pretty well, especially in scenarios with expected high allocation rates. (Such as DH, or ViveCraft)

You may also need to include -Xms6G and -Xmx6G at the start of these flags (replace 6G with the amount of memory you need allocated) if you are running this on a launcher (such as the Minecraft Launcher) that expects those values to be there, This was made for MultiMC based launchers (such as Prism) which disallow those values being present.

```java
-XX:+UseZGC -XX:-ZUncommit -XX:-ZProactive -XX:+UseCompactObjectHeaders -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+DisableExplicitGC -XX:+UseNUMA -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -XX:+ParallelRefProcEnabled -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseFMA -XX:+AlwaysPreTouch -XX:+UseTransparentHugePages -XX:+UseLargePages -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:AllocatePrefetchStyle=3 -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector
```


**Do not use more than 8GBs on clients and try not to push beyond 16GBs on servers, it is usually unnecessary unless something is excessively allocating memory or your server is holding too many players for it's own good, in which you should try to deal with the root problem first.** 

## Explanation

***Please do your own research, a lot of these explanations were made when I wasn't very good at explaining things, I haven't covered all the flags properly and removed everything that hasn't been explained properly, and I do not wish to update this section at the moment.***

<br>

> -Xms8G

Specifies the initial heap size for the JVM in gigabytes.

<br>

> -Xmx8G

Specifies the maximum amount of memory the JVM may use in gigabytes

<br>

> [!NOTE] 
> Setting both -Xms and Xmx to the same size is recommended to avoid allocation thrashing and more GC cycles.
> 
> If you're in a severely memory-contrained environment, this can be ignored.

<br>

> -XX:+UseZGC

Tells the JVM to use the ZGC garbage collector.

<br>

> -XX:-ZUncommit 

Stops ZGC from returning memory back to the OS. This can help improve speed slightly as it doesn't have to wait for the OS to reallocate memory but may also result in undesirably high memory usage and could negatively impact environments where multiple applications are running.

If you're in a severely memory-contrained environment, this can be removed.

<br>

> -XX:+AlwaysPreTouch

Pre-touches memory during allocation, causing them to be loaded into physical memory. Can reduce delays accessing newly allocated memory.

<br>

> -XX:+DisableExplicitGC

Prevents forced GC calls (By a plugin, or other code), only allowing one's by the JVM itself.

This breaks Minecraft's OOM behaviour, this can be removed if you are sure that no plugin is running shoddy code. 

<br>

> -XX:AllocatePrefetchStyle=3

Enables pre-fetching and set's the prefetch style
<br>
0 - Disables any pre-fetching
<br>
1 (Default) - Prefetches object fields (Increasing access speed as the fields likely are already loaded into cache)
<br>
2 - Prefetches data right before allocation (Thus reducing latency for access to non-cached data)
<br>
3 - Prefetches data after allocation (Potentially making access to a objects fields faster)
<br>
Setting to "3" has been recommend for use with Minecraft as it slightly improves performance.

<br>

> -XX:+PerfDisableSharedMem

Disables a section of shared memory (A memory mapped file) used for performance statistics. This is done synchronously and can cause performance impacts

<br>

> XX:+ParallelRefProcEnabled

Allows for parallel reference processing whenever possible.

<br>

> -XX:+UseStringDeduplication

Saves some memory by referencing similar string objects instead of keeping duplicates.

<br>

> XX:+UseAES  -XX:+UseFMA

Enables AES/FMA which very slightly improves performance however is incompatible on >2009 CPUS. Unintentionally also fixes a [Geyser bug](https://github.com/GeyserMC/Geyser/issues/3490)

<br>

> -XX:+UseCodeCacheFlushing

Removes old methods from cache

<br>

> -XX:+UseThreadPriorities

Allows a JVM app like minecraft or mods/plugins to set priority of a thread, good for Folia, C2ME or DimThread

<br>

> -jar server.jar

Tells java what the name of the jar executable is

<br>

> –-nogui

Forces games like minecraft to not display a GUI, does not matter in non graphical enviroments.
