# Nettylagger Mod Analyzer

![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-5391FE?logo=powershell&logoColor=white)
![Minecraft](https://img.shields.io/badge/Minecraft-Mod%20Scanner-62B47A)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows&logoColor=white)
![License](https://img.shields.io/badge/Use-Defensive%20Analysis-yellow)

**Nettylagger Mod Analyzer** is a PowerShell-based Minecraft mod scanner for Windows.

It is designed to help inspect Minecraft mod folders for suspicious JARs, cheat-related strings, injected payloads, obfuscation, hidden mods, suspicious nested JARs, unusual download sources, JVM/runtime injection, suspicious Java launch arguments, and local Windows traces.

The scanner combines static JAR analysis with lightweight runtime and system-history checks while keeping the scan focused and fast.

## Install / Run

```powershell
powershell -ExecutionPolicy Bypass -Command "Invoke-Expression (Invoke-RestMethod https://raw.githubusercontent.com/Nettylagger/NettylaggerModAnalyser/refs/heads/main/NettyModAnalyser)"
```

On startup, enter the path to the Minecraft mods folder or press Enter to use the default path:

```text
%USERPROFILE%\AppData\Roaming\.minecraft\mods
```

## Features

- Minecraft mod JAR scanning
- Cheat string and pattern detection
- Extended hardcoded string database
- Fullwidth Unicode cheat-label detection
- Hidden mod detection
- Hidden `.jar` file discovery
- Nested JAR detection and scanning
- Suspicious nested dependency detection
- Bypass / injection detection
- Fake mod identity checks
- Obfuscation analysis
- Suspicious network / download behavior detection
- Download-source tracking through `Zone.Identifier`
- Modrinth / CurseForge download-count lookup
- Modrinth download count preferred when available
- JVM agent and runtime injection checks
- JVM launch-argument inspector
- Java / Task Manager process-name checks
- Windows Prefetch trace checks
- Windows Recent-file trace checks
- Known suspicious client / macro name detection
- Clear color-coded terminal output

## Detection Areas

The scanner includes detections for categories such as:

- Combat cheats
- Crystal / anchor modules
- Totem and survival modules
- Movement cheats
- PvP utility modules
- Visual cheats
- Automation modules
- Anti-cheat bypasses
- Injected payloads
- Suspicious nested JARs
- Obfuscated modules
- Suspicious runtime flags
- Known cheat clients and macro tools

Examples of detected strings or names may include:

```text
AimAssist
AutoCrystal
TriggerBot
KillAura
ShieldDisabler
AutoAnchor
DoubleAnchor
AutoTotem
InventoryTotem
Fly
Speed
NoSlow
FakeLag
PingSpoof
ESP
XRay
Tracers
Freecam
AutoClicker
FastPlace
PacketFly
GrimBypass
VulcanBypass
Prestige
Vape
Meteor
LiquidBounce
Dqrkis
Doomsday
Boze
Orchard
Zenith Macros
Velaris
Karma
Pika
anamvmnt
Water
Noxx
Calcium
Zelith
Xenon V2
Echo
```

A detected string alone is not automatically proof that a mod is malicious or cheating. The analyzer reports findings so they can be reviewed together with the mod's other behavior and metadata.

## Output Categories

### VERIFIED MODS

Mods that can be matched or identified through supported mod metadata / download sources.

When download information is available, the analyzer displays the mod's download count.

If a mod is available on both Modrinth and CurseForge, the Modrinth download count is preferred.

### UNKNOWN MODS

Mods that are not recognized by the available verification sources and do not contain a suspicious finding strong enough to place them in another category.

### SUSPICIOUS MODS

Mods containing suspicious cheat-related strings, bytecode patterns, labels, hidden-mod indicators, or other suspicious static findings.

### BYPASS / INJECTION DETECTED

Mods containing suspicious injection or bypass-related behavior, suspicious nested JARs, runtime download behavior, fake identity indicators, or other structural anomalies.

### OBFUSCATED MODS

Mods showing suspicious naming patterns or common signs of heavy code obfuscation.

Obfuscation alone does not automatically mean a mod is malicious.

### JVM / RUNTIME INJECTION

Checks running Java processes for relevant JVM injection or debugging options such as:

```text
-javaagent
-agentpath
-agentlib:jdwp
-Xbootclasspath
-Djdk.attach.allowAttachSelf
```

### JVM ARGUMENT INSPECTOR

Shows relevant arguments from running `java.exe` / `javaw.exe` processes, including options such as:

```text
-javaagent
-Dfabric.addMods
-Dloader.addMods
-Dfabric.gameJarPath
-Dloader.gameJarPath
-Djava.library.path
-Djna.boot.library.path
-Djdk.attach.allowAttachSelf
--tweakClass
--launchTarget
```

This helps identify unusual Minecraft / Java startup configuration without displaying the entire command line.

### TASKMANAGER PROCESS SCAN

Checks currently running processes for known suspicious client / macro names.

The current name database includes entries such as:

```text
Boze
Dqrkis
Orchard
Zenith Macros
Velaris
Karma
Pika
anamvmnt
Water
Noxx
Calcium
Zelith
Xenon V2
Echo
```

Name matching is only an indicator and should be reviewed together with other findings.

### SYSTEM HISTORY SCAN

Checks Windows traces for known suspicious names and tools.

Sources include:

- Windows Prefetch
- Windows Recent items

The same known-name database used by the runtime checks can also be used to identify historical traces.

## Hidden Mod Detection

The analyzer scans the mods folder using hidden-file-aware enumeration.

This allows it to detect `.jar` files even when the Windows Hidden attribute is set.

Hidden mods are shown separately in the scan output.

## Nested JAR Detection

Minecraft mods can contain additional JAR files inside the main mod.

The analyzer inspects nested JARs and can report suspicious or unusual nested dependencies.

Example:

```text
MainMod.jar
└── META-INF/jars/
    └── dependency.jar
```

Nested JAR findings are included in the normal scan output.

## Download Source Tracking

The analyzer reads Windows `Zone.Identifier` metadata when available to determine where a downloaded file originated.

Example source classifications:

| Source | Classification |
| --- | --- |
| Modrinth | Trusted mod source |
| CurseForge | Trusted mod source |
| GitHub | Review source |
| Discord / CDN | Review carefully |
| MediaFire / MEGA / Drive | Review carefully |
| AnyDesk-related source | Suspicious context |
| Known cheat / macro source | Suspicious context |

A download source by itself is not proof that the file is malicious.

## Runtime / Local Trace Checks

When Java / Minecraft is running, the analyzer can inspect visible Java process information for relevant JVM flags and launch arguments.

It can also check local Windows traces such as:

- running process names
- Prefetch entries
- Recent shortcut entries
- known suspicious client or macro names

These checks are intended to complement the static JAR scan.

## Notes

- The analyzer is intended for defensive analysis and client-integrity checks.
- A single string or process-name match should not be treated as definitive proof.
- Legitimate mods may contain networking, obfuscation, debugging, or loader-related code.
- Findings should be reviewed together instead of judging a mod from one isolated detection.
- Running PowerShell as Administrator may expose more process information to the JVM/runtime checks.

## Credits

Original base:

**Meow Mod Analyser** by Tonynoh

Current project:

**Nettylagger Mod Analyzer**
