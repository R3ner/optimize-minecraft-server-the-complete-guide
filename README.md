![Java](https://img.shields.io/badge/java-ED8B00?logo=openjdk&logoColor=white)
![Minecraft](https://img.shields.io/badge/minecraft-server-62B47A?logo=minecraft&logoColor=white)
![Bukkit](https://img.shields.io/badge/bukkit-api-FF6B00)
![Spigot](https://img.shields.io/badge/spigot-ED8106)
![PaperMC](https://img.shields.io/badge/papermc-FFFFFF?logo=papermc&logoColor=black)
![Pufferfish](https://img.shields.io/badge/pufferfish-ff69b4)
![Purpur](https://img.shields.io/badge/purpur-9B59B6)
![Optimization](https://img.shields.io/badge/optimization-performance-2ECC71)
![Java Flags](https://img.shields.io/badge/java--flags-tuning-007396)
![Java Argument File](https://img.shields.io/badge/java--argument--file-args-4B8BBE)

# Optimize Your Minecraft Server: The Complete Guide

## Introduction

Running a high-performance Minecraft server requires more than just powerful hardware—it demands meticulous configuration. This comprehensive guide will walk you through every aspect of optimizing Paper and Purpur servers, from basic settings to advanced configurations across all relevant files.

Whether you're running a small survival server or a large network, this guide covers everything you need to achieve stable 20 TPS while maintaining an enjoyable vanilla-like experience for your players.

**What You'll Learn:**
- Server software selection and setup
- Java optimization and startup flags
- Configuration of all server files (server.properties, bukkit.yml, spigot.yml, paper-world-defaults.yml, pufferfish.yml, purpur.yml)
- Anti-Xray configuration
- World pre-generation strategies
- Performance monitoring and profiling
- Common pitfalls and how to avoid them

---

## Choosing the Right Server Software

### Paper: The Foundation

**Paper** is the industry standard for high-performance Minecraft servers. It's a fork of Spigot that offers:
- Superior multithreading capabilities
- Extensive optimization patches
- Full compatibility with Bukkit/Spigot plugins
- Active development and support
- Built-in anti-Xray

**Why not Spigot or Bukkit?** These are outdated in terms of performance. Spigot is in maintenance mode, receiving only critical fixes without any meaningful performance improvements. Paper provides everything Spigot does, but significantly better.

### Pufferfish: For High-Load Servers

**Pufferfish** is a Paper fork focused on extreme performance:
- Advanced entity optimization (DAB - Dynamic Activation of Brain)
- Optimized suffocation checks
- Async mob spawning
- Better projectile handling
- Ideal for servers with 50+ concurrent players

### Purpur: Advanced Customization

**Purpur** is a Pufferfish fork that adds hundreds of configuration options for gameplay customization:
- Rideable mobs
- Single-player sleep
- Enhanced AI controls
- Custom gameplay mechanics
- All Pufferfish optimizations included

**Important:** Only use Purpur if you need its specific features. More patches mean potential for more bugs. For pure performance without gameplay changes, stick with Paper or Pufferfish.

### What to Avoid

❌ **Bukkit/CraftBukkit/Spigot** - Extremely outdated performance-wise
❌ **Downstream forks** - Forks beyond Paper/Purpur often have stability issues
❌ **"Miracle" performance forks** - Often unstable and poorly maintained
❌ **Plugin managers/reloaders** - Can cause memory leaks and instability

---

## Java Setup and Flags

### Required Java Version

- **Minecraft 1.20.5+**: Java 21 or higher required
- **Minecraft 1.18-1.20.4**: Java 17 minimum, Java 21 recommended
- **Older versions**: Follow version-specific requirements

### Recommended Java Vendors

✅ **Adoptium** (Eclipse Temurin)
✅ **Amazon Corretto**

❌ **Oracle Java** - No longer recommended due to licensing changes
❌ **OpenJ9** - Not supported by Paper, known to cause issues
❌ **GraalVM** - Experimental, can cause problems

### Java Startup Flags

Using optimized garbage collection flags is crucial for reducing lag spikes. For production servers, use flags specifically tuned for Minecraft.

**Standard Recommended Flags:**
These provide excellent performance for most servers. For a detailed breakdown of each flag and customized profiles based on your specific server type, player count, and requirements, see our comprehensive [Java Flags Guide](https://renerverse.win/assets/guides/java-flags).

```bash
java -Xms4G -Xmx4G -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+ParallelRefProcEnabled -XX:+PerfDisableSharedMem -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1HeapRegionSize=8M -XX:G1HeapWastePercent=5 -XX:G1MaxNewSizePercent=40 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1NewSizePercent=30 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:G1ReservePercent=20 -XX:InitiatingHeapOccupancyPercent=15 -XX:MaxGCPauseMillis=200 -XX:MaxTenuringThreshold=1 -XX:SurvivorRatio=32 -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true -jar server.jar --nogui
```

**Important Notes:**
- Set `-Xms` and `-Xmx` to the same value
- Adjust memory allocation based on player count and world size
- The `--add-modules=jdk.incubator.vector` flag (beta) can provide additional performance gains

For detailed explanations of each parameter and specialized flag configurations for different scenarios (survival, minigames, high player count, etc.), visit our [Java Flags Guide](https://renerverse.win/guides/java-flags).

---

## Pre-Generating Your World

### Why Pre-Generate?

While modern Paper versions have excellent chunk generation optimizations, pre-generation still provides benefits:
- Eliminates chunk generation lag entirely
- Essential for world map plugins (Pl3xMap, Dynmap)
- Smoother player exploration experience
- Reduces CPU spikes during peak hours

### The Easy Way: Pre-Generated Worlds

Instead of spending hours pre-generating chunks and configuring plugins, get professionally optimized, pre-generated worlds tailored to your exact specifications at **[worlds.renerverse.win](https://worlds.renerverse.win)** - the #1 source for custom Minecraft worlds.

**Benefits:**
- Custom-sized worlds to your specifications
- Pre-optimized for performance
- Multiple biome distributions available
- Professionally generated terrain
- Instant download and setup

### Manual Pre-Generation (If Needed)

If you prefer to generate worlds yourself:

1. Set up a world border FIRST
2. Use Chunky plugin for async generation
3. Generation can take hours depending on size
4. Server will remain playable during generation
5. Keep world border in place after generation

**Important:** Don't remove the world border after pre-generation - it prevents players from generating new chunks and undoing your optimization work.

---

## Network Configuration

### server.properties

```properties
# Network Settings
network-compression-threshold=256
# Compression for packets larger than 256 bytes
# Lower values = more CPU usage, less bandwidth
# Higher values = less CPU usage, more bandwidth
# For servers on same machine/network with proxy, set to -1
# For public servers, 256 is optimal

# Core Performance Settings
view-distance=8
# Terrain visibility distance in chunks
# Lower = better performance
# 8 is good for most servers
# Never go below 3

simulation-distance=6
# How far from player chunks actually tick
# Should be ≤ view-distance
# Significantly impacts performance
# Lower = better performance, but may affect farms

# World Settings
spawn-protection=0
# Disable unless needed for spawn area

enable-command-block=false
# Only enable if necessary

max-players=100
# Set to your expected player count

difficulty=normal
# Adjust to your preference

gamemode=survival
# Your default gamemode

pvp=true
# Enable/disable PvP

online-mode=true
# NEVER set to false unless using proxy auth

# Resource Pack (optional)
resource-pack=
resource-pack-sha1=
require-resource-pack=false

# Performance
max-tick-time=60000
# Set high to prevent watchdog from killing server
# Paper handles this better internally

sync-chunk-writes=false
# CRITICAL: Allows async chunk saving
# Automatically false on Paper, but set it anyway
# MASSIVE performance improvement
```

---

## Bukkit Configuration

### bukkit.yml

This file controls entity spawn limits and timing. The defaults are designed for small servers and need adjustment.

```yaml
settings:
  # Disable plugin query for slight performance gain
  query-plugins: false
  
  # Connection throttle (anti-spam)
  connection-throttle: 4000
  
  # Disable timings (use Spark instead)
  # Paper disables this automatically, but set it anyway
  plugin-profiling: false
  
  # Shutdown message
  shutdown-message: Server closed

# CRITICAL: Spawn Limits Per Player
# Default values are way too high for most servers
# Formula: [player_count] × [limit] = total mobs
spawn-limits:
  # Hostile mobs (zombies, skeletons, creepers)
  monsters: 40
  # Reduce to 30-35 for better performance
  # Don't go below 20 or gameplay suffers
  
  # Passive mobs (cows, sheep, pigs)
  animals: 8
  # Reduce to 5-8 for performance
  # Lower if you have many animal farms
  
  # Aquatic creatures (fish, dolphins)
  water-animals: 5
  # 3-5 is good range
  
  # Ambient water (tropical fish, pufferfish)
  water-ambient: 5
  # 2-3 is sufficient
  
  # Underground water creatures
  water-underground-creature: 3
  
  # Axolotls
  axolotls: 3
  
  # Ambient (bats)
  ambient: 1
  # Bats are mostly useless, 1 is fine

# Spawn Timing (ticks between spawn attempts)
# Higher = less frequent checks = better performance
ticks-per:
  # Animal spawning every 400 ticks (20 seconds)
  animal-spawns: 400
  
  # Monster spawning
  monster-spawns: 10
  # Can increase to 15-20 for slight performance gain
  # Imperceptible difference to players
  
  # Water creature spawning
  water-spawns: 400
  water-ambient-spawns: 400
  water-underground-creature-spawns: 400
  
  # Axolotl spawning
  axolotl-spawns: 400
  
  # Ambient spawning
  ambient-spawns: 400
  
  # World autosave interval (in ticks)
  # 6000 = every 5 minutes
  autosave: 6000
  # Don't change unless you have a reason

# Chunk garbage collector
chunk-gc:
  period-in-ticks: 400
  # How often to clean up chunks (20 seconds)
  
  load-threshold: 300
  # Unload chunks after 15 seconds of no activity
  # Lower if you have memory issues
  # Higher for better caching

# Aliases for commands (optional)
aliases: now-in-commands.yml
```

**Key Points:**
- Lower spawn limits reduce entity load
- Higher tick intervals reduce spawn attempt frequency
- Balance between performance and gameplay experience
- For PvE/hardcore servers, keep monster spawns higher

---

## Spigot Configuration

### spigot.yml

Spigot configuration handles entity activation ranges, growth rates, and merge radiuses.

```yaml
settings:
  # Performance
  save-user-cache-on-stop-only: true
  # Only save player cache on shutdown
  # Reduces disk I/O significantly
  
  # BungeeCord
  bungeecord: false
  # Set to true only if using BungeeCord/Velocity
  
  # Netty threads
  netty-threads: 4
  # Auto-detects based on CPU cores
  # Usually no need to change
  
  # Player sample count
  sample-count: 12
  
  # Timeout settings
  timeout-time: 60
  restart-on-crash: false
  restart-script: ./start.sh
  
  # Log settings
  log-villager-deaths: false
  log-named-deaths: true
  
  # Tab complete
  tab-complete: 0
  # 0 = complete all commands
  # 1+ = require that many letters first
  # -1 = disable tab complete

  # Disable outdated plugins warning
  moved-wrongly-threshold: 0.0625
  moved-too-quickly-multiplier: 10.0

# CRITICAL: Entity Activation Ranges
# Distance from player where entities become "active"
# Inactive entities tick much less frequently
world-settings:
  default:
    # Mob activation ranges (in blocks)
    entity-activation-range:
      animals: 16
      # Passive animals (cows, pigs, sheep)
      # 16 blocks is good balance
      # Lower for better performance
      
      monsters: 24
      # Hostile mobs (zombies, skeletons)
      # 24-32 is good range
      # Lower = better performance
      
      raiders: 48
      # Pillagers, vindicators, ravagers
      # Keep higher for raid mechanics
      
      misc: 8
      # Minecarts, boats, etc.
      # Can be very low
      
      water: 8
      # Aquatic mobs
      
      villagers: 16
      # Keep at 16 for trading halls
      # Lower if villagers cause lag
      
      flying-monsters: 48
      # Phantoms, ghasts
      # Keep higher for proper behavior
    
    # Whether to tick inactive villagers
    tick-inactive-villagers: false
    # Set to false for significant performance gain
    # Villagers only tick when players are nearby
    # May affect very large trading halls
    
    # Nerf spawner mobs when too many exist
    nerf-spawner-mobs: false
    # Set to true if you have many mob grinders
    # Nerfed mobs don't have AI
    
    # View distance
    view-distance: default
    # Use "default" to inherit from server.properties
    # Can set per-world here
    
    # Simulation distance  
    simulation-distance: default
    # Use "default" to inherit from server.properties
    
    # Mob spawn range (in chunks)
    mob-spawn-range: 6
    # Distance from player where mobs spawn
    # Should be ≤ simulation-distance
    # Lower = mobs spawn closer to player
    # 4-6 is optimal for most servers
    
    # Arrow despawn rate (ticks)
    arrow-despawn-rate: 1200
    # 1200 ticks = 60 seconds
    # Arrows despawn after this time
    # Lower for better performance
    
    # Trident despawn rate
    trident-despawn-rate: 1200
    
    # Hanging entity (item frames, paintings)
    hanging-tick-frequency: 100
    # How often hanging entities check for removal
    # Higher = better performance
    
    # Zombie aggro range
    zombie-aggressive-towards-villager: true
    # Set to false to disable zombie/villager pathfinding
    
    # Hopper settings
    hopper-transfer: 8
    # Ticks between hopper transfers
    # 8 is default, increase for performance
    # Will slow down hopper-based systems
    
    hopper-check: 1
    # Ticks between hopper checks
    # 1 is default, increase for performance
    
    hopper-amount: 1
    # Items moved per transfer
    # Don't change unless you know what you're doing
    
    # Entity tracking ranges
    entity-tracking-range:
      players: 48
      animals: 48
      monsters: 48
      misc: 32
      other: 64
    # Distance at which entities are sent to clients
    # Separate from entity-activation-range
    # Lower = less network traffic
    # Too low = entities "pop in" suddenly
    
    # Merge radius for dropped items
    merge-radius:
      item: 3.5
      # Items merge within 3.5 blocks
      # Larger = better performance
      # Don't exceed 4.0
      
      exp: 4.0
      # XP orbs merge within 4.0 blocks
      # Larger = better performance
    
    # Growth modifiers (crop/cactus/etc growth speed)
    growth:
      cactus-modifier: 100
      cane-modifier: 100
      melon-modifier: 100
      mushroom-modifier: 100
      pumpkin-modifier: 100
      sapling-modifier: 100
      beetroot-modifier: 100
      carrot-modifier: 100
      potato-modifier: 100
      wheat-modifier: 100
      netherwart-modifier: 100
      vine-modifier: 100
      cocoa-modifier: 100
      bamboo-modifier: 100
      sweetberry-modifier: 100
      kelp-modifier: 100
      twistingvines-modifier: 100
      weepingvines-modifier: 100
      cavevines-modifier: 100
      glowberry-modifier: 100
    # 100 = normal speed
    # Higher = faster growth
    # Lower = slower growth
    # Don't change unless needed
    
    # Max tick time
    max-tick-time:
      tile: 1000
      entity: 1000
    # CRITICAL: Set to 1000 to disable
    # This feature causes more problems than it solves
    # Paper handles this better internally
    
    # Hunger settings
    hunger:
      jump-walk-exhaustion: 0.05
      jump-sprint-exhaustion: 0.2
      combat-exhaustion: 0.1
      regen-exhaustion: 6.0
      swim-multiplier: 0.01
      sprint-multiplier: 0.1
      other-multiplier: 0.0
    # Don't change unless modifying hunger mechanics
    
    # Max entity collisions
    max-entity-collisions: 2
    # How many collision checks per entity
    # 2 is good balance
    # Lower = better performance
    # 0 = no entity pushing (not recommended)
    
    # Dragon death sound radius
    dragon-death-sound-radius: 0
    # 0 = heard across dimension
    # Set to limit if desired

# Messages
messages:
  restart: Server is restarting
  whitelist: You are not whitelisted on this server!
  unknown-command: Unknown command. Type "/help" for help.
  server-full: The server is full!
  outdated-client: Outdated client! Please use {0}
  outdated-server: Outdated server! I'm still on {0}

# Commands and stats
commands:
  # Disable sending commands to console
  silent-commandblock-console: true
  log: true
  tab-complete: 0
  send-namespaced: true
  spam-exclusions:
  - /skill
  replace-commands:
  - setblock
  - summon
  - testforblock

stats:
  disable-saving: false
  forced-stats: {}

# Advanced settings (don't change unless you know what you're doing)
advancements:
  disable-saving: false
  disabled:
  - minecraft:story/disabled
```

**Key Optimizations:**
- Entity activation ranges are crucial
- Lower mob-spawn-range concentrates mobs near players
- Disable tick-inactive-villagers for big performance gain
- Increase hopper-transfer/check if you have many hoppers
- Set max-tick-time to 1000 to disable watchdog

---

## Paper Global Configuration

### config/paper-global.yml

Global settings that apply to the entire server.

```yaml
# Paper Global Configuration
_version: 28

# Async chunks
async-chunks:
  threads: -1
  # -1 = auto-detect based on CPU cores
  # Usually optimal as-is

# Console settings
console:
  enable-brigadier-completions: true
  enable-brigadier-highlighting: true
  has-all-permissions: false

# Item validation
item-validation:
  book:
    author: 8192
    page: 16384
    title: 8192
  book-size:
    page-max: 2560
    total-multiplier: 0.98
  display-name: 8192
  lore-line: 8192
  resolve-selectors-in-books: false

# Logging
logging:
  deobfuscate-stacktraces: true
  log-player-ip-addresses: true
  use-rgb-for-named-text-colors: true

# Messages
messages:
  kick:
    authentication-servers-down: ''
    connection-throttle: Connection throttled! Please wait before reconnecting.
    flying-player: Flying is not enabled on this server
    flying-vehicle: Flying is not enabled on this server
  no-permission: '&cI''m sorry, but you do not have permission to perform this command. Please contact the server administrators if you believe that this is in error.'
  use-display-name-in-quit-message: false

# Miscellaneous
misc:
  fix-entity-position-desync: true
  lag-compensate-block-breaking: true
  load-permissions-yml-before-plugins: true
  region-file-cache-size: 256
  # Number of region files cached in memory
  # Increase if you have RAM to spare
  # Decrease if low on memory
  
  strict-advancement-dimension-check: false
  use-alternative-luck-formula: false
  use-dimension-type-for-custom-spawners: false

# Packet limiter
packet-limiter:
  kick-message: '&cSent too many packets'
  limits:
    all:
      action: KICK
      interval: 7.0
      max-packet-rate: 500.0
    PacketPlayInAutoRecipe:
      action: DROP
      interval: 4.0
      max-packet-rate: 5.0

# Player auto-save
player-auto-save:
  max-per-tick: -1
  rate: -1

# Proxies
proxies:
  bungee-cord:
    online-mode: true
  proxy-protocol: false
  velocity:
    enabled: false
    online-mode: false
    secret: ''

# Scoreboards
scoreboards:
  save-empty-scoreboard-teams: false
  track-plugin-scoreboards: false

# Spam limiter
spam-limiter:
  incoming-packet-threshold: 300
  recipe-spam-increment: 1
  tab-spam-increment: 1
  tab-spam-limit: 500

# Timings (DISABLE - use Spark instead)
timings:
  enabled: false
  # CRITICAL: Disable timings
  # Causes significant performance degradation
  # Use Spark for profiling instead
  
  hidden-config-entries:
  - database
  - settings.bungeecord-addresses
  - settings.velocity-support.secret
  history-interval: 300
  history-length: 3600
  server-name: Unknown Server
  server-name-privacy: false
  url: https://timings.aikar.co
  verbose: false

# Unsupported settings
unsupported-settings:
  allow-headless-pistons: false
  allow-permanent-block-break-exploits: false
  allow-piston-duplication: false
  # NEVER enable these on survival servers
  
  perform-username-validation: true
  # Only disable if using offline mode (not recommended)

# Watchdog
watchdog:
  early-warning-delay: 10000
  early-warning-every: 5000
```

**Critical Settings:**
- **timings.enabled: false** - ALWAYS disable, use Spark instead
- **region-file-cache-size** - Increase if you have RAM
- **unsupported-settings** - Never enable duplication/exploits

---

## Paper World Configuration

### config/paper-world-defaults.yml

This is where the magic happens. These settings control performance for all worlds by default.

```yaml
# Paper World Configuration Defaults
_version: 28

# Anti-Cheat
anticheat:
  # Anti-Xray (detailed section below)
  anti-xray:
    enabled: false
    # See Anti-Xray section for full configuration
    engine-mode: 1
    hidden-blocks:
    # See Anti-Xray section
    max-block-height: 64
    lava-obscures: false
    replacement-blocks:
    # See Anti-Xray section
    update-radius: 2
    use-permission: false
  
  # Prevent moving into unloaded chunks
  obfuscation:
    items:
      hide-durability: false
      hide-itemmeta: false
      hide-itemmeta-with-visual-effects: false

# Chunks
chunks:
  # Auto-save interval (in ticks)
  auto-save-interval: default
  # Use default from bukkit.yml
  
  # Delay before unloading unused chunks
  delay-chunk-unloads-by: 10s
  # 10 seconds is good balance
  # Prevents constant load/unload
  
  # Entity per chunk save limits
  entity-per-chunk-save-limit:
    area_effect_cloud: 8
    arrow: 16
    dragon_fireball: 3
    egg: 8
    ender_pearl: 16
    experience_bottle: 3
    experience_orb: 16
    eye_of_ender: 8
    fireball: 8
    firework_rocket: 8
    llama_spit: 3
    potion: 8
    shulker_bullet: 8
    small_fireball: 8
    snowball: 8
    spectral_arrow: 16
    trident: 16
    wither_skull: 4
  # CRITICAL: Prevents entity overflow crashes
  # These limits stop projectile spam from crashing server
  # Adjust values based on your needs
  # Never set to -1 unless you want crashes
  
  # Fixed chunk inhabited time
  fixed-chunk-inhabited-time: -1
  # -1 = vanilla behavior
  # Set to 0 for faster hostile mob spawns
  
  # Max auto-save chunks per tick
  max-auto-save-chunks-per-tick: 8
  # How many chunks saved per tick during auto-save
  # Lower = slower saves spread over more time
  # 8 is good for most servers
  # Lower to 6 for high player counts
  # Never set below 4
  
  # Prevent moving into unloaded chunks
  prevent-moving-into-unloaded-chunks: true
  # Teleports players back if they enter unloaded chunk
  # Prevents chunk loading exploits

# Collisions
collisions:
  # Allow vehicle collisions
  allow-vehicle-collisions: true
  
  # Fix climbing bypassing cramming rule
  fix-climbing-bypassing-cramming-rule: true
  # Prevents spider farms from creating massive stacks
  
  # Max entity collisions
  max-entity-collisions: 2
  # Overrides spigot.yml setting
  # 2 is optimal
  # Lower = better performance
  # 0 = no pushing (breaks vanilla mechanics)
  
  # Only players collide
  only-players-collide: false
  # Set to true to disable all non-player collisions
  # Good for minigame servers
  # Breaks mob behavior on survival

# Entities
entities:
  # Armor stands
  armor-stands:
    do-collision-entity-lookups: false
    # Set to false for performance
    # Armor stands won't be pushed by entities
    # Acceptable for most decoration uses
    
    tick: false
    # Set to false for static armor stands
    # Massive performance gain
    # Armor stands won't be affected by water/physics
    # Re-enable if you need movable stands
  
  # Behavior
  behavior:
    baby-zombie-movement-modifier: 0.5
    disable-chest-cat-detection: false
    disable-creeper-lingering-effect: false
    disable-player-crits: false
    door-breaking-difficulty:
      husk:
      - HARD
      vindicator:
      - NORMAL
      - HARD
      zombie:
      - HARD
      zombie_villager:
      - HARD
      zombified_piglin:
      - HARD
    ender-dragons-death-always-places-dragon-egg: false
    experience-merge-max-value: -1
    mobs-can-always-pick-up-loot:
      skeletons: false
      zombies: false
    nerf-pigmen-from-nether-portals: false
    parrots-are-unaffected-by-player-movement: false
    phantoms-do-not-spawn-on-creative-players: true
    phantoms-only-attack-insomniacs: true
    phantoms-spawn-attempt-max-seconds: 119
    phantoms-spawn-attempt-min-seconds: 60
    piglins-guard-chests: true
    pillager-patrols:
      disable: false
      spawn-chance: 0.2
      spawn-delay:
        per-player: false
        ticks: 12000
      start:
        day: 5
        per-player: false
    should-remove-dragon: false
    spawner-nerfed-mobs-should-jump: false
    zombie-villager-infection-chance: -1.0
    zombies-target-turtle-eggs: true
  
  # Markers
  markers:
    tick: false
    # Markers don't need to tick
  
  # Mob effects
  mob-effects:
    immune-to-wither-effect:
      spider: false
      undead: true
    spiders-immune-to-poison-effect: true
    undead-immune-to-certain-effects: true
  
  # Spawning
  spawning:
    # All chunks are slime chunks
    all-chunks-are-slime-chunks: false
    # Set to true for slime farm convenience
    # Removes vanilla slime chunk mechanic
    
    # Alt item despawn rates
    alt-item-despawn-rate:
      enabled: true
      # CRITICAL: Enable this
      # Allows custom despawn rates for different items
      items:
        cobblestone: 300
        # Cobblestone despawns in 15 seconds
        netherrack: 300
        sand: 300
        red_sand: 300
        gravel: 300
        dirt: 300
        grass: 300
        pumpkin: 300
        melon_slice: 300
        kelp: 300
        bamboo: 300
        stick: 300
        snowball: 300
        # Add more items as needed
        # Format: item_name: ticks (20 ticks = 1 second)
    
    # Count all mobs for spawning
    count-all-mobs-for-spawning: false
    # false = Paper's improved spawning algorithm
    # true = vanilla behavior
    
    # Creative arrow despawn rate
    creative-arrow-despawn-rate: default
    
    # Despawn ranges
    despawn-ranges:
      ambient:
        hard: 128
        soft: 32
      axolotls:
        hard: 128
        soft: 32
      creature:
        hard: 128
        soft: 32
      misc:
        hard: 128
        soft: 32
      monster:
        hard: 128
        soft: 32
      underground_water_creature:
        hard: 128
        soft: 32
      water_ambient:
        hard: 64
        soft: 32
      water_creature:
        hard: 128
        soft: 32
    # Soft = mob may despawn randomly
    # Hard = mob will despawn
    # Lower for better performance
    
    # Disable mob spawner mob AI
    disable-mob-spawner-spawn-egg-transformation: false
    
    # Duplicate UUID handling
    duplicate-uuid:
      mode: SAFE_REGEN
      safe-regen-delete-range: 32
    
    # Filter bad NBT from spawner minecarts
    filter-bad-tile-entity-nbt-from-falling-blocks: true
    
    # Iron golems can spawn in air
    iron-golems-can-spawn-in-air: false
    # Set to false for vanilla behavior
    
    # Mob spawn range
    monster-spawn-max-light-level: -1
    # -1 = default (0 for monsters)
    
    # Non-player arrows despawn rate
    non-player-arrow-despawn-rate: default
    
    # Per-player mob spawns
    per-player-mob-spawns: true
    # CRITICAL: Enable this
    # Distributes mob spawns evenly among players
    # Prevents one player from using entire mob cap
    # More single-player-like experience
    # Allows lower spawn-limits without hurting gameplay
    
    # Scan for legacy ender dragon
    scan-for-legacy-ender-dragon: true
    
    # Skeleton horse thunder spawn chance
    skeleton-horse-thunder-spawn-chance: default
    
    # Slime spawn height
    slime-spawn-height:
      slime-chunk:
        maximum: 40.0
      surface-biome:
        maximum: 70.0
        minimum: 50.0
    
    # Spawn limits
    spawn-limits:
      ambient: -1
      axolotls: -1
      creature: -1
      monster: -1
      underground_water_creature: -1
      water_ambient: -1
      water_creature: -1
    # -1 = use bukkit.yml values
    # Set here to override per-world
    
    # Ticks per spawn
    ticks-per-spawn: -1
    # -1 = use bukkit.yml values
    
    # Wandering trader spawn settings
    wandering-trader:
      spawn-chance-failure-increment: 25
      spawn-chance-max: 75
      spawn-chance-min: 25
      spawn-day-length: 24000
      spawn-minute-length: 1200
    
    # Wateranimal spawn height
    wateranimal-spawn-height:
      maximum: default
      minimum: default
  
  # Tracking range
  tracking-range-y:
    animal: default
    display: default
    enabled: false
    misc: default
    monster: default
    other: default
    player: default

# Environment
environment:
  # Disable explosion knockback
  disable-explosion-knockback: false
  
  # Disable ice and snow
  disable-ice-and-snow: false
  
  # Disable teleportation suffocation check
  disable-teleportation-suffocation-check: false
  
  # Disable thunder
  disable-thunder: false
  
  # Fire tick delay
  fire-tick-delay: 30
  # Higher = fire spreads slower
  # Lower = fire spreads faster
  
  # Frosted ice settings
  frosted-ice:
    delay:
      max: 40
      min: 20
    enabled: true
  
  # Generate flat bedrock
  generate-flat-bedrock: false
  
  # Grass spread tick delay
  grass-spread-tick-rate: 1
  # Higher = slower grass spread
  
  # Nether ceiling void damage
  nether-ceiling-void-damage-height: disabled
  # Set to 127 to damage players on nether roof
  # Prevents nether roof travel/farms
  
  # Optimize explosions
  optimize-explosions: true
  # CRITICAL: Enable this
  # Much faster explosion calculations
  # No gameplay impact
  
  # Portal creation
  portal-create-radius: 16
  portal-search-radius: 128
  portal-search-vanilla-dimension-scaling: true
  
  # Treasure maps
  treasure-maps:
    enabled: true
    find-already-discovered:
      loot-tables: default
      villager-trade: true
  
  # Water over lava flow speed
  water-over-lava-flow-speed: 5

# Fishing
fishing-time-range:
  maximum: 600
  minimum: 100

# Game mechanics
game-mechanics:
  disable-end-credits: false
  disable-relative-projectile-velocity: false
  disable-sprint-interruption-on-attack: false
  disable-unloaded-chunk-enderpearl-exploit: true
  fix-curing-zombie-villager-discount-exploit: true
  nerf-pigmen-from-nether-portals: false
  pillager-patrols:
    disable: false
    spawn-chance: 0.2
    spawn-delay:
      per-player: false
      ticks: 12000
    start:
      day: 5
      per-player: false
  scan-for-legacy-ender-dragon: true
  shield-blocking-delay: 5
  show-sign-click-command-failure-msgs-to-player: true
  snowball-and-egg-can-knockback-itemframes: true

# Hopper
hopper:
  cooldown-when-full: true
  # Hoppers don't check when full
  # Significant performance improvement
  
  disable-move-event: false
  # Set to true if no plugins use HopperMoveItemEvent
  # Performance gain
  
  ignore-occluding-blocks: false

# Lootables
lootables:
  auto-replenish: false
  max-refills: -1
  refresh-max: 2d
  refresh-min: 12h
  reset-seed-on-fill: true
  restrict-player-reloot: true

# Maps
maps:
  item-frame-cursor-limit: 128
  item-frame-cursor-update-interval: 10

# Misc
misc:
  disable-end-credits: false
  fix-climbing-bypassing-cramming-rule: true
  light-queue-size: 20
  max-leash-distance: 10.0
  redstone-implementation: VANILLA
  # VANILLA or ALTERNATE_CURRENT
  # ALTERNATE_CURRENT is faster but may break some redstone
  
  update-pathfinding-on-block-update: true

# Scoreboards
scoreboards:
  allow-non-player-entities-on-scoreboards: false
  use-vanilla-world-scoreboard-name-coloring: false

# Spawn
spawn:
  allow-using-signs-inside-spawn-protection: false
  keep-spawn-loaded: true
  keep-spawn-loaded-range: 10

# Tick rates
tick-rates:
  behavior:
    villager:
      validatenearbypoi: 60
      acquirepoi: 120
  # CRITICAL: Villager optimization
  # validatenearbypoi: how often villagers check their POI
  # acquirepoi: how often villagers look for new POI
  # Default is -1 (every tick)
  # Setting higher dramatically improves performance
  # 60/120 is good balance
  # Increase more if villagers cause lag
  
  container-update: 1
  # How often containers update
  # Higher = better performance
  # May cause visual delays
  
  grass-spread: 4
  # How often grass spreads
  # Higher = slower spread
  
  mob-spawner: 2
  # How often spawners attempt to spawn
  # Higher = slower spawning
  # Good for nerfing spawner farms
  
  sensor:
    villager:
      secondarypoisensor: 80
      # How often villagers sense secondary POIs
      # Higher = better performance
      # 80 is good balance
```

**Critical Optimizations:**
- Enable **per-player-mob-spawns**
- Set **armor-stands.tick: false**
- Configure **entity-per-chunk-save-limit** for all projectiles
- Enable **alt-item-despawn-rate** for common items
- Increase **tick-rates** for villagers if they cause lag
- Enable **optimize-explosions**
- Set **hopper.cooldown-when-full: true**

---

## Pufferfish Configuration

### pufferfish.yml

Pufferfish adds advanced entity optimizations.

```yaml
# Pufferfish Configuration

# Version
info:
  version: '1.0'

# Sentry DSN (error tracking - optional)
sentry-dsn: ''

# Enable books
enable-books: true
# Set to false to prevent book exploits
# Players need pufferfish.usebooks permission when false

# Suffocation optimization
enable-suffocation-optimization: true
# CRITICAL: Enable this
# Rate limits suffocation checks
# Massive performance gain with many entities
# No gameplay impact

# Async mob spawning
enable-async-mob-spawning: true
# CRITICAL: Enable this
# Offloads mob spawning to async threads
# Significant performance improvement

# Projectile settings
projectile:
  max-loads-per-tick: 10
  # How many chunks a projectile can load per tick
  # Lower = better performance
  # Too low = enderpearls/tridents may not work properly
  
  max-loads-per-projectile: 8
  # Total chunks a projectile can load in lifetime
  # Lower = better performance
  # 8 is good balance

# DAB (Dynamic Activation of Brain)
# CRITICAL: This is Pufferfish's main optimization
dab:
  enabled: true
  # ALWAYS enable this
  # Throttles entity AI based on distance from players
  # Massive performance improvement
  # Minimal gameplay impact
  
  start-distance: 12
  # Distance (blocks) where DAB starts to apply
  # Entities beyond this tick slower
  # 12 is good balance
  
  max-tick-freq: 20
  # Maximum tick interval for far entities
  # 20 = entities tick every 20 ticks (1 second)
  # Higher = better performance
  # Too high = very slow entity behavior
  
  activation-dist-mod: 7
  # Controls how quickly DAB effect increases with distance
  # Lower = more aggressive (better performance)
  # Don't go below 6
  # 7-8 is optimal for most servers
  
  blacklisted-entities: []
  # Entities that should not be affected by DAB
  # Add entities here if DAB breaks their behavior
  # Example: [minecraft:villager, minecraft:iron_golem]

# Inactive goal selector throttle
inactive-goal-selector-throttle: true
# CRITICAL: Enable this
# Prevents inactive entities from selecting new goals
# Performance improvement
# May affect some farms

# Miscellaneous
misc:
  disable-method-profiler: true
  # Disable vanilla method profiler
  # Performance improvement
  # No downside
  
  disable-out-of-order-chat: false
  # Set to true if using proxy

# Entity timeouts
entity_timeouts:
  # How long entities can exist before being removed
  # -1 = no timeout (vanilla)
  # Set values for specific entities to prevent buildup
  SNOWBALL: -1
  EGG: -1
  ENDER_PEARL: -1
  ARROW: 1200
  # Example: arrows timeout after 60 seconds
  TRIDENT: 1200
  SPECTRAL_ARROW: 1200

# Flare (profiler - requires subscription)
flare:
  url: https://flare.airplane.gg

# Online utilities
online-utilities:
  api-endpoint: https://api.airplane.gg
```

**Key Points:**
- **DAB is crucial** - this is Pufferfish's main feature
- Enable all optimization toggles
- Configure entity_timeouts for projectiles
- Adjust dab.activation-dist-mod if villagers/iron farms lag

---

## Purpur Configuration

### purpur.yml

Purpur offers extensive customization options. Only change what you need.

```yaml
# Purpur Configuration

# Version
version: 22
verbose: false

# Settings
settings:
  # Server MOTD name
  server-mod-name: 'Purpur'
  
  # TPS catchup
  tps-catchup: true
  # Attempts to catch up if server falls behind
  # Usually beneficial
  
  # Lagging threshold (TPS)
  lagging-threshold: 19.0
  # Below this TPS, server considers itself lagging
  # Used for some conditional optimizations
  
  # Don't send useless entity packets
  dont-send-useless-entity-packets: true
  # CRITICAL: Enable this
  # Reduces network traffic
  # No downside
  
  # Allow water placement in the End
  allow-water-placement-in-the-end: true
  
  # Use alternate keepalive
  use-alternate-keepalive: true
  # Better for players with unstable connections
  # Incompatible with TCPShield
  # Sends keepalive every second, kicks only if none responded in 30s
  
  # Disable give dropping
  disable-give-dropping: false
  
  # Block settings
  blocks:
    disable-note-block-updates: false
    disable-chorus-plant-updates: false
    disable-mushroom-updates: false
    
    # Custom block mechanics
    barrel:
      rows: 3
    ender_chest:
      six-rows: false
      use-permissions-for-rows: false
    crying_obsidian:
      valid-for-portal-frame: false
  
  # Broadcasts
  broadcasts:
    advancement:
      only-broadcast-to-affected-player: false
    death:
      only-broadcast-to-affected-player: false
  
  # Logger settings
  logger:
    suppress-init-legacy-material-errors: false
    suppress-ignored-advancement-warnings: false
    suppress-unrecognized-recipe-errors: false
    suppress-setblock-in-far-chunk-errors: false
  
  # Network
  network:
    max-joins-per-second: 5
    # Prevents login spam
  
  # Packets
  packets:
    disable-give-dropping: false
  
  # Seed
  seed:
    structure:
      buried-treasure: -1
      # Set different seeds for structures
      # Helps prevent seed cracking
      mineshaft: -1
  
  # Timings
  timings:
    url: https://timings.purpurmc.org

# World settings
world-settings:
  default:
    # Gameplay mechanics
    gameplay-mechanics:
      # Disable various mechanics for performance/preference
      disable-mob-spawner-spawn-egg-transformation: false
      
      # Mob spawning
      entities-can-use-portals: true
      # Set to false to prevent entities from using portals
      # Prevents entities from loading chunks via portals
      # May break some farm designs
      
      fix-climbing-bypassing-cramming-rule: true
      
      # Infinity bow requirement
      infinity-bow-requires-arrows: true
      
      # Player abilities
      player:
        idle-timeout:
          kick-if-idle: false
          # Kick idle players
          count-as-sleeping: false
          # Count idle as sleeping for phantom spawning
          update-tab-on-status-change: false
        
        # Invulnerable while accepting resource pack
        invulnerable-while-accepting-resource-pack: false
        
        # Teleport if outside border
        teleport-if-outside-border: false
      
      # Rideable settings
      ridable:
        # Enable riding various entities
        # Set to false for vanilla behavior
        # May be fun for custom servers
        skeleton: false
        zombie: false
        piglin: false
        etc: false
      
      # Silk touch spawners
      silk-touch:
        enabled: false
        # Allow silk touch to pick up spawners
        tools:
        - minecraft:iron_pickaxe
        - minecraft:diamond_pickaxe
        - minecraft:netherite_pickaxe
      
      # Shift right-click repairs
      shift-right-click-repairs-mending-points: 0
      
      # Void damage
      void-damage-height: -64.0
      void-damage-deals: 4.0
    
    # Mobs settings
    mobs:
      # Global mob settings
      aggressive-towards-villager-when-lagging: false
      # Zombies won't target villagers when server lags
      # Good for performance
      
      disable-mob-spawner-spawn-egg-transformation: false
      
      # Iron golem settings
      iron-golem:
        ridable: false
        can-spawn-in-air: false
        can-swim: false
      
      # Villager settings
      villager:
        ridable: false
        allow-trading: true
        can-breed: true
        clerics-farm-warts: false
        
        # Brain tick settings
        brain-ticks: 1
        # How often villager AI updates
        # Higher = better performance
        # Too high = slow villager behavior
        # 2-3 is good for laggy servers
        
        # Lobotomize settings
        lobotomize:
          enabled: false
          # Lobotomizes villagers that can't pathfind
          # Strips their AI until they can path again
          # Good for stuck villagers
          # Enable if villagers cause severe lag
          check-interval: 100
      
      # Zombie settings
      zombie:
        ridable: false
        aggressive-towards-villager-when-lagging: false
        # Zombies stop targeting villagers when lagging
        # Reduces pathfinding load
      
      # Cow settings
      cow:
        ridable: false
        breeding-delay-ticks: 6000
      
      # Chicken settings
      chicken:
        ridable: false
        breeding-delay-ticks: 6000
      
      # And many more mob-specific settings...
      # Only change what you need
      # Most should stay at defaults
    
    # Blocks
    blocks:
      # Anvil settings
      anvil:
        use-mini-message: false
        allow-colors: false
      
      # Beacon settings
      beacon:
        effect-range:
          level-1: 20
          level-2: 30
          level-3: 40
          level-4: 50
      
      # Bed settings
      bed:
        explode: true
        explode-on-villager-sleep: false
      
      # Dispenser settings
      dispenser:
        apply-cursed-to-armor-slots: true
      
      # Ender chest settings
      ender_chest:
        six-rows: false
      
      # Farmland settings
      farmland:
        gets-moist-from-below: false
        use-alpha-farmland: false
        # Alpha farmland doesn't need water
        # Set to true for convenience
      
      # Magma block settings
      magma-block:
        damage-when-sneaking: false
        damage-boots: false
      
      # Spawner settings
      spawner:
        deactivate-by-redstone: false
        # Redstone can disable spawners
        fix-mc-238526: false
    
    # Entity collision optimizations
    entity:
      # Shared random (use with caution)
      shared-random: true
      # Can cause issues
      # Disable if you experience problems
```

**Purpur Optimization Notes:**
- Most features are disabled by default
- Only enable what you actually need
- **dont-send-useless-entity-packets: true** is crucial
- **use-alternate-keepalive** helps with unstable connections
- Villager **brain-ticks** and **lobotomize** are powerful for lag
- **aggressive-towards-villager-when-lagging** reduces pathfinding load
- Don't enable ridable mobs on survival servers

---

## Anti-Xray Configuration

Anti-Xray is one of Paper's best features. It prevents X-ray hacks from seeing ores underground.

### Understanding Engine Modes

**Engine Mode 1:**
- Replaces hidden ores with stone/deepslate/netherrack
- Lightweight and fast
- Only hides ores completely surrounded by solid blocks
- Ores exposed to air (caves) remain visible
- Minimal performance impact
- Recommended for most servers

**Engine Mode 2:**
- Replaces ores with random fake ores
- Adds fake ores everywhere
- Hides even ores exposed to air (if air is in hidden-blocks)
- Higher performance impact
- Better protection against X-ray
- Can cause client FPS drops for some players

**Engine Mode 3:**
- Similar to Mode 2
- Randomizes per-chunk-layer instead of per-block
- Reduces network load
- Better chunk compression
- Experimental

### Recommended Configuration

#### For Overworld (Mode 1 - Performance)

**config/paper-world-defaults.yml** or **world/paper-world.yml:**

```yaml
anticheat:
  anti-xray:
    enabled: true
    engine-mode: 1
    max-block-height: 64
    # Ores above this height aren't hidden
    # Adjust based on ore distribution
    # 64 is good for most ores
    # Increase to 90 for all ores in 1.18+
    
    update-radius: 2
    # How many blocks around update when mining
    # 2 is good balance
    # Higher = more bandwidth, better accuracy
    
    lava-obscures: false
    # Whether lava hides ores
    # Usually false
    
    use-permission: false
    # If true, players with paper.antixray.bypass aren't affected
    # Set to false for security
    
    hidden-blocks:
    # Blocks to hide/obfuscate
    - chest
    - coal_ore
    - deepslate_coal_ore
    - copper_ore
    - deepslate_copper_ore
    - raw_copper_block
    - diamond_ore
    - deepslate_diamond_ore
    - emerald_ore
    - deepslate_emerald_ore
    - gold_ore
    - deepslate_gold_ore
    - iron_ore
    - deepslate_iron_ore
    - raw_iron_block
    - lapis_ore
    - deepslate_lapis_ore
    - redstone_ore
    - deepslate_redstone_ore
    # Add any other blocks you want to hide
    
    replacement-blocks:
    # In mode 1, only the first block is used
    - stone
    - deepslate
```

#### For Overworld (Mode 2 - Maximum Protection)

```yaml
anticheat:
  anti-xray:
    enabled: true
    engine-mode: 2
    max-block-height: 64
    update-radius: 2
    lava-obscures: false
    use-permission: false
    
    hidden-blocks:
    # Ores to hide AND blocks to add as fake ores
    - air
    # Adding air hides ores in caves too
    # May cause FPS drops for some clients
    - copper_ore
    - deepslate_copper_ore
    - raw_copper_block
    - diamond_ore
    - deepslate_diamond_ore
    - gold_ore
    - deepslate_gold_ore
    - iron_ore
    - deepslate_iron_ore
    - raw_iron_block
    - lapis_ore
    - deepslate_lapis_ore
    - redstone_ore
    - deepslate_redstone_ore
    
    replacement-blocks:
    # Blocks that will appear as fake ores
    # More variety = better obfuscation
    - chest
    - amethyst_block
    - andesite
    - budding_amethyst
    - calcite
    - coal_ore
    - deepslate_coal_ore
    - deepslate
    - diorite
    - dirt
    - emerald_ore
    - deepslate_emerald_ore
    - granite
    - gravel
    - oak_planks
    - smooth_basalt
    - stone
    - tuff
```

#### For Nether

Add this at the bottom of **world_nether/paper-world.yml**:

```yaml
anticheat:
  anti-xray:
    enabled: true
    engine-mode: 1
    max-block-height: 128
    # Nether height
    update-radius: 2
    lava-obscures: false
    use-permission: false
    
    hidden-blocks:
    - ancient_debris
    - nether_gold_ore
    - nether_quartz_ore
    
    replacement-blocks:
    - netherrack
```

#### For The End

Add this to **world_the_end/paper-world.yml**:

```yaml
anticheat:
  anti-xray:
    enabled: false
    # No ores in the End
```

### Anti-Xray Best Practices

1. **Test your configuration** - Use X-ray texture packs to verify it works
2. **Adjust max-block-height** based on ore distribution in your version
3. **Mode 1 is usually sufficient** for survival servers
4. **Mode 2 if X-ray is a serious problem** on your server
5. **Monitor performance** - Mode 2 can impact TPS on large servers
6. **Update block lists** when new ores are added
7. **Don't give bypass permission** to regular players

### Limitations

Anti-Xray cannot prevent:
- **Seed cracking** - Use different structure seeds in purpur.yml
- **Ores exposed to air** - Use Mode 2 with 'air' in hidden-blocks (FPS cost)
- **100% protection** - It's obfuscation, not encryption

For caves and exposed ores, consider the **RayTraceAntiXray** plugin, which uses ray tracing to hide only visible ores in Mode 1.

---

## Performance Monitoring

### Spark - The Essential Tool

**Spark** is the modern replacement for Timings. It provides detailed performance profiling without the massive overhead of Timings.

**Installation:**
1. Download from https://spark.lucko.me/download
2. Place in plugins folder
3. Restart server

**Usage:**
```
/spark profiler start
# Let it run during typical server activity (5-10 minutes)
/spark profiler stop
# Generates a link to view results
```

**What to look for:**
- **Entity ticking** - Too high? Reduce spawns, optimize farms
- **Chunk loading** - Spikes? Pre-generate world, optimize view distance
- **Plugin overhead** - Which plugins use most CPU?
- **Tile entities** - Too many hoppers/furnaces/etc?

### Other Monitoring Tools

**Plan** - Overall server statistics and performance tracking
**VillagerOptimiser** - If villagers are a problem
**FarmLimiter** - Limits entities in farms
**FarmControl** - Advanced farm management

### Avoid These

❌ **ClearLagg** - Band-aid solution, doesn't fix underlying issues
❌ **LagAssist** - Intrusive, often causes more problems
❌ **Plugins that claim to "remove lag"** - Usually ineffective

---

## Common Issues and Solutions

### High Entity Count

**Symptoms:** High entity tick time, TPS drops
**Solutions:**
- Lower spawn-limits in bukkit.yml
- Enable per-player-mob-spawns
- Configure alt-item-despawn-rate
- Limit mob grinders with FarmLimiter
- Check for item overflow with /minecraft:kill @e[type=item]

### Villagers Causing Lag

**Symptoms:** High entity tick time, specifically villagers
**Solutions:**
- Increase villager brain-ticks in purpur.yml (2-3)
- Enable lobotomize for stuck villagers
- Increase POI validation intervals in paper-world-defaults.yml
- Set tick-inactive-villagers: false in spigot.yml
- Limit trading halls to necessary size
- Use VillagerOptimiser plugin

### Chunk Loading Lag

**Symptoms:** TPS drops when exploring, slow chunk loading
**Solutions:**
- Lower view-distance and simulation-distance
- Pre-generate the world
- Increase delay-chunk-unloads-by
- Lower mob-spawn-range
- Check if plugins are force-loading chunks

### Redstone Lag

**Symptoms:** TPS drops near redstone contraptions
**Solutions:**
- Increase hopper transfer/check intervals
- Use RedstoneLimiter plugin
- Limit observer clocks
- Check for lag machines
- Consider switching to ALTERNATE_CURRENT redstone implementation

### Mob Farm Issues

**Symptoms:** Farms not working after optimization
**Solutions:**
- Check entity-activation-range (especially for iron farms)
- Verify mob-spawn-range is adequate
- Ensure per-player-mob-spawns is configured correctly
- Check if DAB is affecting farm (add to blacklist if needed)
- Verify tick-inactive-villagers setting for iron farms

### Memory Issues

**Symptoms:** Out of memory errors, frequent GC pauses
**Solutions:**
- Allocate more RAM (but not too much)
- Use optimized GC flags
- Lower region-file-cache-size
- Reduce entity limits
- Check for memory leaks with Spark
- Restart server regularly (once per day/week)

---

## Performance Checklist

Use this checklist to verify your optimization:

### Java & Startup
- [ ] Using Java 21 (for 1.20.5+)
- [ ] Using Adoptium or Amazon Corretto
- [ ] Using optimized GC flags
- [ ] Xms = Xmx (same value)
- [ ] Adequate RAM allocated (not too much)

### Server Software
- [ ] Using Paper, Pufferfish, or Purpur
- [ ] Latest stable version
- [ ] Auto-update enabled

### World
- [ ] World pre-generated
- [ ] World border set
- [ ] Unused worlds removed

### server.properties
- [ ] view-distance: 8 or less
- [ ] simulation-distance: 6 or less
- [ ] sync-chunk-writes: false
- [ ] network-compression-threshold: 256

### bukkit.yml
- [ ] spawn-limits reduced
- [ ] ticks-per configured
- [ ] save-user-cache-on-stop-only: true

### spigot.yml
- [ ] entity-activation-range optimized
- [ ] tick-inactive-villagers: false
- [ ] mob-spawn-range: 6 or less
- [ ] merge-radius optimized
- [ ] max-tick-time: 1000/1000
- [ ] max-entity-collisions: 2

### paper-global.yml
- [ ] timings.enabled: false

### paper-world-defaults.yml
- [ ] per-player-mob-spawns: true
- [ ] optimize-explosions: true
- [ ] armor-stands.tick: false
- [ ] alt-item-despawn-rate enabled
- [ ] entity-per-chunk-save-limit configured
- [ ] tick-rates.behavior.villager optimized
- [ ] hopper.cooldown-when-full: true
- [ ] Anti-Xray configured

### pufferfish.yml (if using)
- [ ] enable-suffocation-optimization: true
- [ ] enable-async-mob-spawning: true
- [ ] dab.enabled: true
- [ ] inactive-goal-selector-throttle: true

### purpur.yml (if using)
- [ ] dont-send-useless-entity-packets: true
- [ ] use-alternate-keepalive: true
- [ ] villager brain-ticks configured

### Monitoring
- [ ] Spark installed
- [ ] Regular profiling scheduled
- [ ] No timings usage
- [ ] Performance baseline established

---

## Final Notes

### Optimization Philosophy

Remember: **There is no "perfect" configuration.** Every server is different. Your player count, game mode, hardware, and community all affect what settings work best.

**Key Principles:**
1. **Start conservative** - Don't immediately set everything to minimum
2. **Change incrementally** - Adjust one thing at a time
3. **Monitor results** - Use Spark to verify improvements
4. **Balance performance and gameplay** - Don't sacrifice fun for TPS
5. **Communicate with players** - Explain why farms might work differently

### Maintenance Schedule

**Daily:**
- Monitor TPS and MSPT
- Check for errors in logs
- Watch for player complaints

**Weekly:**
- Run Spark profiler
- Review entity counts
- Check for chunk errors
- Restart server

**Monthly:**
- Update server software
- Review and adjust configs
- Audit plugins for updates
- Clean up old backups

**When Issues Arise:**
- Profile FIRST before changing configs
- Check recent changes (plugins, configs, etc.)
- Review server logs
- Ask in Paper Discord with Spark report

### Getting Help

If you're still experiencing performance issues after following this guide:

1. **Run Spark** - Get a profile during the lag
2. **Check logs** - Look for errors or warnings
3. **Join Paper Discord** - Provide your Spark report
4. **Be specific** - "Server lags" doesn't help; "Entity tick time at 80% during peak hours" does
5. **Share your setup** - Player count, hardware, installed plugins

### Additional Resources

- **PaperMC Documentation**: https://docs.papermc.io
- **Paper Discord**: https://discord.gg/papermc
- **Purpur Documentation**: https://purpurmc.org/docs
- **Pufferfish Documentation**: https://docs.pufferfish.host
- **YouHaveTrouble's Guide**: https://github.com/YouHaveTrouble/minecraft-optimization
- **Paper Chan's Guide**: https://paper-chan.moe/paper-optimization
- **Spark Documentation**: https://spark.lucko.me/docs

---

## Conclusion

Optimizing a Minecraft server is an ongoing process, not a one-time task. With the configurations and best practices outlined in this guide, you should be able to achieve stable 20 TPS while maintaining an enjoyable player experience.

Remember:
- **Hardware matters**, but configuration matters more
- **Monitor, don't guess** - use Spark to identify actual issues
- **Less is more** - don't install every "anti-lag" plugin
- **Update regularly** - Paper, Purpur, and Pufferfish constantly improve
- **Backup before changing** - Always have a way to rollback

Your players will thank you for the smooth, lag-free experience!

---

**For pre-generated custom worlds optimized for performance, visit [worlds.renerverse.win](https://worlds.renerverse.win)**

**For detailed Java flag configurations and profiles optimized for your specific server type, see our [Java Flags Guide](https://renerverse.win/assets/guides/java-flags)**

---

*Last updated: February 2026*
*Compatible with Minecraft 1.20.5 - 1.21.11*
