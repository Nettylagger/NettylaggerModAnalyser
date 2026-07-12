# Nettylagger Mod Analyzer

![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-5391FE?logo=powershell&logoColor=white)
![Minecraft](https://img.shields.io/badge/Minecraft-Mod%20Scanner-62B47A)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows&logoColor=white)
![License](https://img.shields.io/badge/Use-Defensive%20Analysis-yellow)

**Nettylagger Mod Analyzer** is a PowerShell-based Minecraft mod scanner for detecting suspicious JARs, cheat clients, injected payloads, obfuscated modules, risky download sources, and JVM/runtime injection.

It checks mods by hash, scans class names and bytecode strings, detects known cheat patterns, analyzes obfuscation, and can optionally run an advanced deobfuscation pass for hidden strings.

## Install / Run

```powershell
powershell -ExecutionPolicy Bypass -Command "Invoke-Expression (Invoke-RestMethod 'https://raw.githubusercontent.com/MeowTonynoh/MeowModAnalyzer/main/MeowModAnalyzer.ps1')"
```

## Scan Modes

On startup, enter your mods folder path or press Enter for:

```text
%USERPROFILE%\AppData\Roaming\.minecraft\mods
```

Then choose:

```text
[1] Normal   - fast string and pattern scan
[2] Advanced - includes deobfuscated string decoding
```

## Features

- SHA1 verification with Modrinth and Megabase
- Cheat pattern and hardcoded string detection
- Fullwidth Unicode cheat label detection
- Nested JAR scanning inside `META-INF/jars/`
- Bypass and injection detection
- Fake mod identity detection
- Obfuscation analysis
- JVM agent/runtime injection scan
- Java process / Task Manager runtime checks
- Windows Prefetch and Recent file trace checks
- Download source tracking through `Zone.Identifier`
- Optional advanced string deobfuscation

## Advanced Deobfuscation

Advanced mode attempts to recover hidden strings from common bytecode obfuscation patterns:

- `byte[]` string arrays
- `char[]` string arrays
- XOR string decoding
- XOR with index
- reverse-index XOR
- subtract/index variants
- Base64 constants

Recovered strings are matched against the cheat string database and reported when they hit.

## Detection Areas

The scanner includes detections for:

- Combat cheats: AimAssist, AutoCrystal, TriggerBot, KillAura, ShieldDisabler
- Crystal/anchor modules: AutoAnchor, DoubleAnchor, SafeAnchor, AutoHitCrystal
- Totem/survival modules: AutoTotem, HoverTotem, InventoryTotem, MaceSwap
- Movement cheats: Fly, Speed, NoSlow, JumpReset, ElytraSpeed
- PvP utilities: FakeLag, PingSpoof, FakeInv, AutoPearl, PackSpoof
- Visual cheats: ESP, XRay, Tracers, Freecam, Nametags
- Automation: AutoClicker, FastPlace, AutoEat, AutoMine, ChestSteal
- Anti-cheat bypasses: GrimBypass, VulcanBypass, PacketFly, VerusDisabler
- Known clients and payloads: Prestige, Vape, Meteor, LiquidBounce, Dqrkis, Doomsday

## Output Categories

### VERIFIED MODS

Mods matched by SHA1 hash in Modrinth or Megabase.

### UNKNOWN MODS

Mods not found in databases, but with no detected suspicious content.

### SUSPICIOUS MODS

Mods containing cheat-related patterns, strings, fullwidth labels, or deobfuscated string matches.

### BYPASS / INJECTION DETECTED

Mods with structural loader anomalies, injected payloads, hidden entrypoints, suspicious nested JARs, suspicious network behavior, or fake identity signals.

### OBFUSCATED MODS

Mods with statistically suspicious naming or known obfuscator signatures.

### JVM / RUNTIME INJECTION

Live Java process checks for `-javaagent`, bootclasspath injection, native agents, or debug hooks.

### PREFETCH / RECENT TRACES

Checks Windows Prefetch and Recent files for traces of known suspicious tools and clients, including Prestige Client and 198M / 198Macros related entries.

## Download Source Tracking

The analyzer reads Windows `Zone.Identifier` metadata and classifies common origins:

| Source | Risk |
| --- | --- |
| Modrinth | Safe |
| CurseForge | Safe |
| GitHub | Verify |
| Discord / CDN | Risky |
| MediaFire / MEGA / Drive | Risky |
| AnyDesk | Suspicious |
| Prestige / Doomsday / Dqrkis / 198Macros | Suspicious |

## Runtime / Local Trace Checks

If Java/Minecraft is running, the analyzer inspects the visible Java process command line for suspicious JVM flags and agents.

It also checks local Windows traces:

- Prefetch entries
- Recent shortcut entries
- known suspicious names such as Prestige Client and 198M / 198Macros

## Notes

Normal mode is faster and better for scanning many mods.

Advanced mode is slower, but can catch payloads that hide cheat strings through bytecode string obfuscation.

This tool is intended for defensive mod analysis, server moderation, and client integrity checks.
