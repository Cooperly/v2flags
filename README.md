### ***v2flags*** is a blazing fast set of arguments for best in class JVM performance.
(though your most likely reading this for Minecraft performance lmfaooo :skull:)

Quick Start
=================
If you quickly want to get to a specific set of flags, since this list is slightly cluttered.
* [Why not Aikar's flags?](#why-not-just-use-aikars-flags)
* [For Who?](#who-should-use-these-flags)
* [Stability + Risks](#stability--risks)
* [Flags](#flags)
    * [Server](#server-zgc)
    * [Pterodactyl](#pterodactyl-zgc)
    * [High End Client](#high-end-client-zgc)
    * [Low End Client](#low-end-client-g1gc)
* [(terrible) Explanation](#explanation)

## Why not just use [Aikar](https://github.com/aikar)'s flags?
Their GC is based on G1, it's extremely stable but extremely slow, so why should current gen servers be held back by last gen algorithms?

I propose to get rid of it in favor of [ZGC](https://github.com/openjdk/zgc), why? It clears memory alongside the server instead of pausing it each time, and it's wicked fast.

## Who should use these flags?
Modded servers or large networks will generally benefit the most from performance flags with increased performance and fewer TPS spikes,

Even smaller servers with weaker hardware can see great performance uplifts,

Pterodactyl server administrators can also receive an extremely fast egg to run JVM servers with, 

And clients can get multiple orders of magnitude better performance and less hitches with client flags.

When using these flags you should try to use [GraalVM Community JDK 22](https://www.graalvm.org/downloads/) as your java runtime,
<br>
Whether your a server, or a client. These flags won't see as much of a gain without it as it tweaks this JDK heavily.

## Stability + Risks
These flags have been rigorously tested to hell and back on Beeper-MC for 4 months straight, and no instability problems have occured as a result of these flags being used so far, only uplifts in performance and consistancy.


However keep in mind that your experience with these will differ due to various hardware configurations and they haven't been dragged through the slums like Aikar's flags. 


***Use them at your own risk.*** You are on your own and void (aka Cooperly, V2K) or anybody involved (Nova) are not responsible for any damages such as crashes or in the worst case even corruption that come as a result of using these flags.

## Flags
- [x] [Vanilla](https://www.minecraft.net/en-us/download/server) (Not recommended, use [Leaf](https://github.com/Winds-Studio/Leaf), or at the very least [Paper](https://github.com/PaperMC/Paper).)
- [x] [Leaf](https://github.com/Winds-Studio/Leaf) / [Paper](https://github.com/PaperMC/Paper)
- [x] [Folia](https://github.com/PaperMC/Folia)
- [x] [Fabric](https://github.com/FabricMC)
- [x] [Quilt](https://github.com/QuiltMC)
- [x] [Forge](https://github.com/MinecraftForge/MinecraftForge)
- [x] [NeoForge](https://github.com/neoforged/NeoForge)
- [x] Most JVM server software, even non-minecraft (No guarantee, not recommended.)

Server-Side Stability flags were removed as they were a misrepresentation of what v2flags is capable of.

The whole premise of these flags is to switch off of an outdated GC while optimizing ZGC for max performance. While G1GC did get some additional performance using the additional parameters, it's not really needed in my opinion.

You can still pull those via commit history if you insist on G1GC based flags (please consider upgrading to modern standards)

Experiment:
<br>
I've set -XX:AllocatePrefetchStyle to 3 instead of 1 on flags based on ZGC, some people say it's unsupported but it seems to be running okay, so either it's being silently dropped, or it's netting a bit of extra performance

If you experience any problems, set it back to 1 and create a pull request changing the value, thanks.

### ***Server, ZGC:***

Use these flags on a server running either bare metal, or in a container. (Not in Pterodactyl, scroll down.)

*Don't forget to adjust -Xms and -Xmx to your server configuration, along with java path and server.jar if applicable*

```java
java -Xms8G -Xmx8G -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+UseZGC -XX:-ZUncommit -XX:-ZProactive -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+AlwaysActAsServerClassMachine -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseNUMA -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -XX:+ParallelRefProcEnabled -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:-UseBiasedLocking -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:AllocatePrefetchStyle=3 -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -DPaper.IgnoreJavaVersion=true -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar server.jar –-nogui
```

### ***Pterodactyl, ZGC:***

Use these flags on Pterodactyl Panel, This is primarily for administrators, customers usually cannot change their server image or flags, however you could ask and link your host's support team to this repository if you really wish to persist for some reason.

Get the [GraalVM Template](https://github.com/Cooperly/v2flags/releases/download/v1.0e/Pterodactyl.json) egg and add your server.jar manually

Adjust CUTDOWN in startup settings to how much memory you want to reserve to the system to prevent OOMKilling, usually 1-4G should be enough depending on the load.

If you wish to not reserve memory to the OS for some reason, (i paid for all my ram sticks boy) at least adjust swap accordingly, preferably to -1 (you could net some performance loss)

```java
java -Xms$(({{SERVER_MEMORY}} - ({{CUTDOWN}})))M -Xmx$(({{SERVER_MEMORY}} - ({{CUTDOWN}})))M -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+UseZGC -XX:-ZUncommit -XX:-ZProactive -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+AlwaysActAsServerClassMachine -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseNUMA -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -XX:+ParallelRefProcEnabled -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:AllocatePrefetchStyle=3 -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -DPaper.IgnoreJavaVersion=true -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar {{SERVER_JARFILE}} --nogui
```

### ***High End Client, ZGC:***

Use these flags on a high end client that wants additional performance and less hitches, especially while using mods like ViveCraft.
<br>
These are not as rigorously tested as the server flags, but no additional issues should arise under normal circumstances. 

You may also need to include -Xms6G and -Xmx6G (or whatever memory you want allocated) if you are running this on the vanilla launcher, (just don't) This is primarily made for MultiMC based launchers (such as Prism) which disallow those values being present.

```java
-XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+UseZGC -XX:-ZUncommit -XX:-ZProactive -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+AlwaysActAsServerClassMachine -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseNUMA -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -XX:+ParallelRefProcEnabled -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:AllocatePrefetchStyle=3 -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector
```

### ***Low End Client, G1GC:***

Use these flags on a weaker client that wants a bit of additional performance, but simply don't have the power to run ZGC.
<br>
These won't give you as much of an impact as Performance Focused, and are not suggested for using mods like ViveCraft.

You may also need to include -Xms2G and -Xmx2G (or whatever memory you want allocated) if you are running this on the vanilla launcher, (just don't) This is primarily made for MultiMC based launchers (such as Prism) which disallow those values being present.

```java
-XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+UseG1GC -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+AlwaysActAsServerClassMachine -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseNUMA -XX:AllocatePrefetchStyle=3 -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -Dgraal.CompilerConfiguration=community -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector
```


> Warning `Option UseBiasedLocking was deprecated in version 15.0 and will likely be removed in a future release.` can be completely ignored without consequences, it won't effect anything and is unlikely to be removed anytime soon.

**Using more than 16GB is generally over-overkill 99.99% of the time unless your in a heavily modded environment** 

## Explanation

***Please try not to rely on these explanations, do your own research as I don't know how to explain things due to my anxiety***
<br>
***Kudos to Nova for filling some things here with more understandable information***

> [!NOTE]
> For most of the flags below `-XX:+UnlockExperimentalVMOptions` and/or `-XX:+UnlockDiagnosticVMOptions` are required.

<br>

> -Xmx8G

Specifies the maximum amount of memory the JVM may use in gigabytes

<br>

> -Xms8G

Specifies the initial heap size for the JVM in gigabytes

<br>

> [!NOTE] 
> Setting both -Xms and Xmx to the same size is recommended to improve predictability, and to have less GC cycles.

<br>

> -XX:+UseZGC

Tells the JVM to use the ZGC garbage collector

<br>

> -XX:-ZUncommit 

Stops ZGC from returning memory back to the OS. This can help improve speed as it doesn't have to wait for the OS to reallocate the memory but may also result on undesirably high memory usage and may negatively impact environments where multiple applications are running

<br>

> -XX:MaxGCPauseMillis=200

Tells JVM to try and keep it's GC (Garbage collector) pauses to or under this specified amount of time. However this only a target and not a guarantee. Doesn't work with `-XX:+UseZGC`

<br>

> -XX:+UseG1GC -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:MaxGCPauseMillis=200

Tells java while using stability flags to use G1GC and make short pauses instead of long ones.

<br>

> -XX:+AlwaysActAsServerClassMachine

Forces java to use garbage collection that would be used with "Server class" hardware. Should show benefits on systems with higher core count CPUs as garbage can be passed of to another thread. `ParallelGC`, `CMS` (Concurrent Mark Sweep) or `G1GC` will be used instead of SerialGC

<br>

> -XX:+AlwaysPreTouch

Pre-touches memory during allocation, causing them to be loaded into physical memory. Can reduce delays accessing newly allocated memory.

<br>

> -XX:+DisableExplicitGC

Prevents forced GC calls (By a plugin, or other code), only allowing one's by the JVM itself

<br>

> -XX:+UseNUMA

Tells JVM that the system uses Non-uniform Memory Access (NUMA), increasing the use of lower latency memory on multi-node topologies (Caches or RAM). Only available when used with `-XX:+UseParallelGC`

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
Setting to "3" has been recommend for use with Minecraft as it improves performance  

<br>

> -XX:+PerfDisableSharedMem

Disables a section of shared memory (A memory mapped file) used for performance statistics. This is done synchronously and can cause performance impacts

<br>

> XX:+ParallelRefProcEnabled

Allows for parallel reference processing whenever possible.

<br>

> -XX:-UseBiasedLocking

Improves performance of un-contended synchronization, also spits an error on startup about depreciation which you ***should ignore***

<br>

> -XX:+UseStringDeduplication

Saves some memory by referencing similar string objects instead of keeping duplicates.

<br>

> XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA

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

Forces games like minecraft to not display the funky gui, does not matter in non graphical enviroments, I personally remove this flag when running JVM servers on my machine.

<br>

> -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal Dlog4j2.formatMsgNoLookups=true -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+OmitStackTraceInFastThrow XX:+TrustFinalNonStaticFields -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -XX:ThreadPriorityPolicy=1 -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll

These flags are generally either [GraalVM](https://www.graalvm.org/) specific optimizations which optimize performance, or they make such little difference in how a JVM app works that they don't deserve documentation on this page for the sake of my own sanity, do your own research.

<br>

> [!NOTE]
> Not all values and flags may have been explained properly, as information was extremely hard to come by and especially in an explainable format, You may get more correct information from other sources.
