# ⚙️ RapidFox: Zen/Firefox Optimization Guide (v7.0)

A highly structured and comprehensive performance guide for Firefox/Zen Browser users seeking peak responsiveness, efficiency and memory management optimized for 4GB, 8GB and 16GB RAM systems.

**🧪 Tested Performance Improvement:** [Speedometer 3.1](https://browserbench.org/Speedometer3.0/) benchmark on 2016 ASUS laptop (i5-6300HQ, 8GB RAM, GTX 960M, 70 Mbps) improved from **6.90 → 10.10 (+47%)** with these optimizations.

**🎯 Hardware Note:** If you have dual GPUs and experience laggy scrolling, set the Integrated GPU (Intel) as default for Zen in Windows Graphics Settings and NVIDIA Control Panel.

---

## Risk Classification System

| Symbol | Risk Level | Description |
|:------:|:-----------|:------------|
| 🟢 | **Safe** | Recommended settings with minimal risk, tested extensively |
| 🟡 | **Moderate** | Generally safe but may cause minor issues on some systems |
| 🟠 | **Caution** | Use with care, test thoroughly, may break some functionality |
| 🔴 | **Advanced** | Only for experienced users, significant trade-offs or risks |
| ⚡ | **Essential** | Core settings that unlock performance improvements |
| 🔋 | **Power** | Settings that may increase battery consumption/power usage |
| 🐧 | **Linux** | Linux-specific optimizations |
| 🪟 | **Windows** | Windows-specific optimizations |
| 🍎 | **macOS** | macOS-specific optimizations |

---

## Table of Contents
1. [Core Principles & Prerequisites](#1-core-principles--prerequisites)
2. [Profile Preparation](#2-profile-preparation)
3. [Network Performance & Connection Management 🌐](#3-network-performance--connection-management-)
4. [Memory Management & Caching 🧠](#4-memory-management--caching-)
5. [JavaScript Engine & Content Processing ⚙️](#5-javascript-engine--content-processing-️)
6. [GPU Acceleration & Rendering Engine 🖼️](#6-gpu-acceleration--rendering-engine-️)
7. [UI Responsiveness & Interactivity 🎯](#7-ui-responsiveness--interactivity-)
8. [Process Architecture & Tab Management 🧵](#8-process-architecture--tab-management-)
9. [Media Playback & Codec Optimization 🎥](#9-media-playback--codec-optimization-)
10. [Security, Privacy & Tracking Protection 🔒](#10-security-privacy--tracking-protection-)
11. [Platform-Specific Optimizations 💻](#11-platform-specific-optimizations-)
12. [Zen Browser Exclusive Features 🦊](#12-zen-browser-exclusive-features-)
13. [External Tools & System Integration 🛠️](#13-external-tools--system-integration-️)
14. [User.js Configuration](#14-userjs-configuration)
15. [Maintenance, Diagnostics & Troubleshooting 🔧](#15-maintenance-diagnostics--troubleshooting-)

---

## 1. Core Principles & Prerequisites

### Performance Philosophy
> Rust-based rendering (Project Quantum) in Zen/Firefox emphasizes security and correctness over performance, but with proper tuning you can approach Chrome-like speed. These tweaks focus on performance: faster page loads, smoother media/video, efficient multitasking, and controlled RAM/bandwidth usage optimized for modern hardware configurations.

### System Requirements & RAM Optimization
- **4GB RAM Systems**: Conservative settings, disk cache enabled, limited processes
- **8GB RAM Systems** (Primary target): Balanced approach with moderate memory caching
- **16GB+ RAM Systems**: Aggressive memory caching, disabled disk cache for maximum speed
- **GPU**: Discrete or integrated with OpenGL 3.0+ support
- **Storage**: SSD recommended for optimal cache performance
- **Power**: High Performance power plan enabled
- **Windows 11**: Consider enabling Hardware Accelerated GPU Scheduling (HAGS)

### Content-Heavy Website Performance
These optimizations significantly improve performance on multimedia-heavy content by optimizing media caching, GPU acceleration and connection management. Settings are continuously evolving as Zen developers improve the browser over time.

---

## 2. Profile Preparation

### ⚠️ MANDATORY FIRST STEP
**CRITICAL**: This setting MUST be changed before applying any other optimizations:

| Setting | Value | Purpose |
|:--------|:------|:--------|
| `browser.preferences.defaultPerformanceSettings.enabled` | `false` | ⚡ **ESSENTIAL** - Unlocks manual control of performance settings. Without this, all other optimizations will be ignored. |

### Backup and Safety
1. Navigate to `about:support → Open Profile Folder` and create full backup
2. Test changes gradually in sections
3. Restart Zen after each major configuration group
4. Document your changes for easy rollback

---

## 3. Network Performance & Connection Management 🌐

### 3.1 Connection Pool Optimization
Maximizes parallel connections and optimizes DNS/cache to speed up loading, especially beneficial for multimedia content and multiple tabs.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `network.http.max-connections` | `1200` | 🟢 | **Total simultaneous connections** Zen can make. Essential for multimedia-heavy browsing and many tabs. Default is usually 256. Higher values use more RAM but significantly improve loading speed for content-rich sites including YouTube. |
| `network.http.max-persistent-connections-per-server` | `8` | 🟢 | **Keep-alive connections per host**. Improves connection reuse efficiency and reduces overhead when loading multiple resources from the same domain. Default is 6. |
| `network.http.max-urgent-start-excessive-connections-per-host` | `5` | 🟢 | **Extra connections for high-priority content** (videos, dynamic content, critical resources). Allows Zen to burst above normal limits for immediate user needs. |
| `network.http.request.max-start-delay` | `5` | 🟢 | **Maximum delay before HTTP request dispatch**. Prevents excessive queuing delays by limiting how long requests wait before being sent to network stack. Lower values = faster response. |

### 3.2 Request Pacing & Burst Control
Controls how aggressively Zen sends requests to optimize for different network conditions.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `network.http.pacing.requests.enabled` | `false` | 🟡 | **Disables request pacing mechanism**. Sends requests in larger bursts rather than gradual pacing. Increases speed on fast, low-latency connections where network capacity is abundant. ⚠️ **Risk of congestion on slow networks or with many devices**. |
| `network.http.pacing.requests.burst` | `32` | 🟢 | **HTTP requests per burst**. When pacing is enabled, controls burst size. Use `16` on slower networks, `32` for fast connections. Boosts throughput without excessive congestion. |
| `network.http.pacing.requests.min-parallelism` | `10` | 🟢 | **Minimum active connections before pacing applies**. Maintains consistent parallelism while enabling intelligent pacing control. |

### 3.3 DNS Resolution & Caching
Optimizes domain name resolution for faster page loads and reduced latency.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `network.dnsCacheExpiration` | `600` | 🟢 | **DNS cache timeout (10 minutes)**. Reduces redundant DNS lookups, decreases CPU/network overhead, and provides quicker domain resolution for revisited sites. |
| `network.dnsCacheExpirationGracePeriod` | `120` | 🟢 | **Use expired DNS entries while refreshing**. Continue using cached DNS entries up to 2 minutes past expiration while fetching updates in background. Smooths out latency spikes. |
| `network.dnsCacheEntries` | `10000` | 🟢 | **Number of DNS entries cached**. Dramatically reduces lookup latency during large browsing sessions. Default is typically 400. Higher values beneficial for heavy browsing. |
| `network.dns.disableIPv6` | `true` | 🟡 | **Disable IPv6 DNS lookups**. Can speed up DNS resolution if your ISP has poor IPv6 implementation. Test this - some networks perform better with IPv6 enabled. |

### 3.4 TLS & Security Caching
Optimizes secure connection establishment and resumption.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `network.ssl_tokens_cache_capacity` | `32768` | 🟢 | **TLS session ticket cache size**. Enables faster HTTPS connection resumption by caching session tickets. Minimal RAM impact with significant speed improvement for HTTPS sites. |

### 3.5 Prefetch & Speculation Control
Disables various forms of predictive loading to conserve bandwidth and improve privacy.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `network.http.speculative-parallel-limit` | `0` | 🟢 | **Disable speculative connections**. Prevents Zen from pre-connecting to servers for links you might click. Saves bandwidth and system resources. |
| `network.dns.disablePrefetch` | `true` | 🟢 | **Stop DNS prefetching for unclicked links**. Prevents preemptive DNS resolution of link targets, improving privacy and conserving bandwidth. |
| `network.dns.disablePrefetchFromHTTPS` | `true` | 🟢 | **Prevent DNS prefetching from secure sources**. Reinforces privacy protection and efficiency by blocking prefetch from HTTPS pages. |
| `network.prefetch-next` | `false` | 🟢 | **Disable automatic next-page prefetching**. Stops loading of pagination "next" links, reducing wasted bandwidth on content you may never view. |
| `network.predictor.enabled` | `false` | 🟢 | **Disable predictive network prefetching**. Stops the predictor system that guesses and preloads content based on browsing patterns. |
| `network.predictor.enable-prefetch` | `false` | 🟢 | **Block all predictor module prefetching**. Comprehensive disable of speculative prefetching from the predictor subsystem. |
| `browser.urlbar.speculativeConnect.enabled` | `false` | 🟢 | **Stop address bar suggestion connections**. Prevents connecting to servers when typing in address bar before navigation. |
| `browser.places.speculativeConnect.enabled` | `false` | 🟢 | **Block speculative connects from history/bookmarks**. Stops pre-connecting when hovering over history or bookmark items. |

---

## 4. Memory Management & Caching 🧠

### 4.1 JavaScript Memory Controls
Optimizes JavaScript heap allocation and memory pressure handling.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `javascript.options.mem.high_water_mark` | `128` | 🟢 | **Memory pressure threshold (128MB)**. Point at which JavaScript garbage collection becomes more aggressive to prevent excessive memory usage. |

### 4.2 Browser Cache Configuration (RAM-Specific Recommendations)
Controls how Zen stores and retrieves cached content optimized for different RAM configurations.

#### For 16GB+ RAM Systems (Maximum Speed):
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `browser.cache.disk.enable` | `false` | 🟡⚡ | **Disable disk cache (RAM only)**. Eliminates cache write operations for maximum speed. ⚠️ **Requires 16GB+ RAM**. All cached data lost on browser close. Note: This setting affects YouTube and content-heavy websites by eliminating I/O bottlenecks. |
| `browser.cache.disk.capacity` | `0` | 🟢 | **Set disk cache size to 0** when disk cache is disabled. Must be used in conjunction with disk cache disable. |
| `browser.cache.memory.capacity` | `131072` | 🟢 | **Memory cache size (128MB)** for 16GB+ systems. Aggressive caching for maximum performance. |

#### For 8GB RAM Systems (Balanced Approach):
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `browser.cache.disk.enable` | `true` | 🟢 | **Enable disk cache** for balanced RAM usage. Recommended for 8GB systems. |
| `browser.cache.disk.capacity` | `2097152` | 🟢 | **Disk cache size (2GB)** for 8GB RAM systems. Balanced performance and storage usage. |
| `browser.cache.memory.capacity` | `65536` | 🟢 | **Memory cache size (64MB)** for 8GB systems. Optimal balance for most users. |

#### For 4GB RAM Systems (Conservative Settings):
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `browser.cache.disk.enable` | `true` | 🟢 | **Enable disk cache** essential for low-RAM systems. |
| `browser.cache.disk.capacity` | `1048576` | 🟢 | **Disk cache size (1GB)** for 4GB RAM systems. Conservative but functional. |
| `browser.cache.memory.capacity` | `32768` | 🟢 | **Memory cache size (32MB)** for 4GB systems. Minimal RAM impact. |

#### Universal Cache Settings:
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `browser.cache.disk.smart_size.enabled` | `false` | 🟢 | **Disable dynamic cache size adjustment**. Prevents automatic cache size changes based on available disk space, ensuring consistent performance. |
| `browser.cache.memory.max_entry_size` | `32768` | 🟢 | **Maximum single cache entry (32MB)**. Allows caching of larger resources like high-resolution images and media files. Set to `-1` for unlimited. |
| `browser.cache.disk.metadata_memory_limit` | `16384` | 🟢 | **Disk cache metadata memory pool (16MB)**. Size of RAM used to store metadata about disk cache entries for faster lookup. |
| `browser.cache.max_shutdown_io_lag` | `100` | 🟢 | **Maximum shutdown I/O wait (100ms)**. Limits how long Zen waits during shutdown to flush cache operations to disk. |

### 4.3 Image Processing & Memory
Optimizes image loading, decoding, and memory management for visual content.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `image.mem.max_decoded_image_kb` | `512000` | 🟢 | **Maximum decoded image memory (500MB)**. Prevents excessive RAM usage from high-resolution images while allowing reasonable caching of decoded image data. |
| `image.cache.size` | `10485760` | 🟢 | **Total image cache size (10MB)**. Memory allocated specifically for cached image data. Speeds up image-heavy pages and galleries. |
| `image.mem.decode_bytes_at_a_time` | `65536` | 🟢 | **Image decode chunk size (64KB)**. Larger chunks = faster image decoding but higher temporary CPU usage. Balance between speed and responsiveness. |
| `image.mem.shared.unmap.min_expiration_ms` | `120000` | 🟢 | **Minimum image memory unmap time (2 minutes)**. How long unused decoded image memory stays mapped before being released. Improves performance for image-heavy workflows. |

### 4.4 Media Caching & Buffering (YouTube Optimized)
Specialized caching for audio/video content to prevent rebuffering and improve streaming performance.

#### RAM-Specific Media Cache Settings:

#### For 16GB+ RAM Systems:
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `media.memory_cache_max_size` | `1048576` | 🟢 | **Media RAM cache size (1GB)**. Aggressive media buffering for 16GB+ systems. Dramatically improves YouTube and streaming performance. |
| `media.memory_caches_combined_limit_kb` | `4194304` | 🟢 | **Total media cache limit (4GB)**. High limit for heavy multimedia usage with multiple video tabs. |

#### For 8GB RAM Systems:
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `media.memory_cache_max_size` | `524288` | 🟢 | **Media RAM cache size (512MB)**. Balanced media caching for 8GB systems. Good YouTube performance without excessive RAM usage. |
| `media.memory_caches_combined_limit_kb` | `2097152` | 🟢 | **Total media cache limit (2GB)**. Reasonable limit for multiple video tabs on 8GB systems. |

#### For 4GB RAM Systems:
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `media.memory_cache_max_size` | `262144` | 🟢 | **Media RAM cache size (256MB)**. Conservative media caching for 4GB systems. Basic YouTube performance improvement. |
| `media.memory_caches_combined_limit_kb` | `1048576` | 🟢 | **Total media cache limit (1GB)**. Limited but functional for basic video streaming. |

#### Universal Media Settings:
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `media.cache_readahead_limit` | `600` | 🟢 | **Media pre-buffer duration (10 minutes)**. How much media content to buffer ahead of current playback position. Essential for smooth YouTube playback. |
| `media.cache_resume_threshold` | `300` | 🟢 | **Resume threshold (5 minutes)**. Minimum buffered content required before resuming playback after buffering pause. Prevents frequent buffering interruptions. |

### 4.5 Session & Storage Management
Controls browser session persistence and storage allocation. Note: dom.caches.enabled and dom.storage.enabled are force-enabled by default and must remain enabled for compatibility.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `dom.storage.default_quota` | `20480` | 🟢 | **Local storage quota per origin (20MB)**. Reasonable limit prevents websites from consuming excessive storage while allowing functional web apps. |
| `browser.sessionstore.interval` | `60000` | 🟢 | **Session save interval (1 minute)**. Reduces frequency of session data writes to disk, improving I/O performance. Balance between crash recovery and performance. |
| `browser.sessionhistory.max_total_viewers` | `10` | 🟢 | **Cached pages for back/forward navigation**. Number of previously visited pages kept in memory for instant back/forward navigation. Higher values use more RAM. |
| `browser.sessionstore.max_tabs_undo` | `10` | 🟢 | **Recently closed tabs memory limit**. Number of closed tabs Zen remembers for "Undo Close Tab" feature. Helps control memory consumption. |
| `browser.sessionstore.max_entries` | `10` | 🟢 | **Per-tab session history limit**. Number of back/forward history steps stored per tab. Reduces memory usage per tab for heavy browsing sessions. |

---

## 5. JavaScript Engine & Content Processing ⚙️

### 5.1 Content Parsing & Tokenization (RAM-Optimized Presets)
Optimizes how Zen processes and renders web content, with different presets for various RAM configurations.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `content.maxtextrun` | `8191` | 🟢 | **Maximum text buffer parse size**. Ensures very long text segments are parsed as single units, preventing weird breaks in JavaScript-heavy pages or streaming logs. |
| `content.interrupt.parsing` | `true` | 🟢 | **Allow parsing interruption for UI tasks**. Permits the browser to pause content parsing to handle UI interactions, preventing tab freezes during heavy script loads. |
| `content.notify.ontimer` | `true` | 🟢 | **Use timer-based content notifications**. Enabled by default - placed higher in table for clarity. Enables more predictable and efficient content update scheduling. |

#### Content Processing Presets by RAM:

#### For 16GB+ RAM Systems (Aggressive Processing):
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `content.notify.interval` | `50000` | 🟢 | **Content notification delay (0.05s)**. Faster content updates for high-RAM systems. More responsive but higher CPU usage. |
| `content.max.tokenizing.time` | `2000000` | 🟢 | **Maximum uninterrupted parsing time (2s)**. Longer parsing for faster content processing on powerful systems. |
| `content.switch.threshold` | `250000` | 🟢 | **UI responsiveness threshold (0.25s)**. Faster UI switching for high-performance systems. |

#### For 8GB RAM Systems (Balanced Processing):
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `content.notify.interval` | `100000` | 🟢 | **Content notification delay (0.1s)**. Balanced content updates for 8GB systems. Optimal for most users. |
| `content.max.tokenizing.time` | `1000000` | 🟢 | **Maximum uninterrupted parsing time (1s)**. Balanced parsing duration for responsive performance. |
| `content.switch.threshold` | `500000` | 🟢 | **UI responsiveness threshold (0.5s)**. Standard responsiveness for most hardware configurations. |

#### For 4GB RAM Systems (Conservative Processing):
| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `content.notify.interval` | `200000` | 🟢 | **Content notification delay (0.2s)**. Slower content updates to preserve RAM and CPU on constrained systems. |
| `content.max.tokenizing.time` | `500000` | 🟢 | **Maximum uninterrupted parsing time (0.5s)**. Shorter parsing to maintain responsiveness on slower systems. |
| `content.switch.threshold` | `750000` | 🟢 | **UI responsiveness threshold (0.75s)**. Longer threshold to prevent UI stuttering on low-RAM systems. |

### 5.2 Layout & Rendering Performance
Controls how Zen handles page layout calculations and visual updates.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `layout.frame_rate` | `-1` | 🟢 | **Automatic frame rate detection**. Set to `-1` for automatic monitor sync (recommended), `240` for high-refresh displays, `60` for consistency, or `0` for uncapped. Leaving at `-1` allows optimal performance without forcing high refresh rates. |
| `nglayout.initialpaint.delay` | `0` | 🟢 | **Initial paint delay**. Lowering nglayout.initialpaint.delay to 0 speeds up initial visual response, especially on broadband connections. Default was 250ms but the delay was changed to 5ms on desktop in Firefox 53. Setting to 0 provides immediate rendering. For users preferring stable initial render with fewer shifts, use higher values like 50-100ms. |

### 5.3 Font & Text Rendering
Optimizations for text rendering performance and quality.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `gfx.content.skia-font-cache-size` | `32` | 🟢 | **Font rendering cache in Skia (32MB)**. Increases font cache size to improve performance on text-heavy websites. Especially beneficial for sites with many font faces or complex typography. |

---

## 6. GPU Acceleration & Rendering Engine 🖼️

### 6.1 WebRender (Mozilla's Rust-based GPU Engine)
Modern GPU-accelerated rendering engine that offloads rendering tasks from CPU to GPU for significant performance improvements.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `gfx.webrender.all` | `true` | 🟢 | **Enable WebRender for all content types**. Forces GPU-accelerated rendering for all page elements, not just whitelisted content. Major performance improvement for modern hardware. |
| `gfx.webrender.enabled` | `true` | 🟢 | **Enable WebRender engine**. Activates Mozilla's GPU-accelerated rendering engine written in Rust. Provides Chrome-like rendering performance with Firefox security model. |
| `gfx.webrender.compositor` | `true` | 🟢 | **Use WebRender compositor**. Enables GPU-based frame composition for smoother visual delivery and reduced CPU overhead during animations and scrolling. |
| `gfx.webrender.precache-shaders` | `true` | 🟢 | **Precompile GPU shaders**. WebRender attempts to compile and cache GPU shaders before rendering, reducing 50-250ms delays during cold page loads. Cache persists across browser restarts. ⚠️ **Requires GPU support**. |
| `gfx.webrender.software` | `false` | 🟡 | **CPU fallback for WebRender**. Enable only if GPU is blacklisted or unavailable. Provides WebRender benefits through CPU rendering but with performance penalty. |

### 6.2 Hardware Acceleration & GPU Process
Advanced GPU utilization settings for maximum rendering performance. Note: layers.gpu-process.enabled, gfx.canvas.accelerated are already enabled by default but included for verification.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `layers.acceleration.force-enabled` | `true` | 🟡 | **Force GPU acceleration**. Bypasses hardware blacklists to enable GPU acceleration on all systems. ⚠️ **May cause visual glitches on unsupported hardware**. Only enable with decent GPU. |

### 6.3 Canvas & 2D Graphics Optimization
Specialized settings for 2D graphics performance and WebGL content with enhanced options.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `gfx.canvas.accelerated.cache-items` | `32768` | 🟢 | **Maximum GPU-cached canvas items**. Number of canvas rendering operations that can be cached on GPU memory for reuse. Higher values improve performance for canvas-heavy applications. |
| `gfx.canvas.accelerated.cache-size` | `4096` | 🟢 | **Canvas cache size (4MB GPU memory)**. Total GPU memory allocated for caching canvas render operations. Improves performance for WebGL games and interactive graphics. |
| `gfx.canvas.max-size` | `16384` | 🟢 | **Maximum canvas dimension (16384px)**. Allows larger canvas elements for high-resolution graphics work and professional applications. |
| `webgl.max-size` | `16384` | 🟢 | **Maximum WebGL texture size**. Enables high-resolution 3D textures for professional graphics applications and games. |

### 6.4 Modern Graphics APIs
Cutting-edge graphics API support for next-generation web applications.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `dom.webgpu.enabled` | `true` | 🟢 | **Enable WebGPU API**. Provides advanced, low-level GPU access for high-performance graphics and computation. Essential for modern games, scientific visualization, and compute-heavy web applications. **Default in recent Zen versions**. |
| `webgl.force-enabled` | `true` | 🟡 | **Force WebGL activation**. Enables WebGL even on blacklisted hardware. Essential for 3D web content but may cause issues on very old graphics hardware. |

---

## 7. UI Responsiveness & Interactivity 🎯

### 7.1 Interface Timing & Responsiveness
Core settings that control how quickly the browser interface responds to user interactions.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `ui.submenuDelay` | `0` | 🟢 | **Instant sub-menu opening**. Eliminates delay when opening context menus and sub-menus for immediate response to user interactions. |
| `browser.uidensity` | `0` | 🟢 | **UI density setting**. `0` = Normal spacing, `1` = Compact (saves vertical space), `2` = Large (accessibility). Use Compact for tight screen real estate. |

### 7.2 Animation & Visual Effects
Controls browser animations and visual feedback for optimal user experience.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `dom.element.animate.enabled` | `true` | 🟢 | **Enable Web Animations API**. Activates modern animation system that allows browser to optimize CSS/JS animations natively, providing smoother performance than legacy animation methods. |

### 7.3 Scrolling Performance & Control
Advanced scrolling optimizations for smooth page navigation - the perfect scrolling configuration based on extensive testing.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `general.smoothScroll` | `true` | 🟢 | **Enable smooth scrolling**. Primary smooth scroll control - must be enabled first before other smooth scroll settings take effect. |
| `general.smoothScroll.msdPhysics.enabled` | `false` | 🟢 | **Disable problematic smooth scroll physics**. **TOP PRIORITY** - Removes mass-spring-damper physics model that causes overshooting and unpredictable scroll behavior. Results in much better feeling scrolling. |
| `general.smoothScroll.currentVelocityWeighting` | `0` | 🟢 | **Eliminate velocity smoothing**. **INSANELY IMPORTANT** - Setting to 0 provides immediate, responsive scroll input without velocity weighting. The feeling is dramatically better and more precise. |
| `apz.overscroll.enabled` | `false` | 🟢 | **Disable overscroll effect**. **MUCH BETTER when disabled** - Eliminates bouncy overscroll animation that can feel sluggish and interfere with precise scrolling control. Essential for optimal scrolling experience. |
| `general.smoothScroll.stopDecelerationWeighting` | `1` | 🟢 | **Immediate scroll stopping**. Eliminates scroll momentum continuation when input stops, providing precise control over scroll ending. Perfect for accurate positioning. |
| `general.smoothScroll.mouseWheel.durationMaxMS` | `150` | 🟢 | **Maximum scroll animation duration**. Faster scroll animation (150ms) provides responsive feeling without being jarring. |
| `general.smoothScroll.mouseWheel.durationMinMS` | `50` | 🟢 | **Minimum scroll animation duration**. Prevents instantaneous scrolling while maintaining responsiveness for small scroll movements. |
| `mousewheel.min_line_scroll_amount` | `18` | 🟢 | **Optimal scroll distance per wheel tick**. Tested value of 18 provides ideal balance between precision and coverage for most content types. |
| `mousewheel.scroll_series_timeout` | `10` | 🟢 | **Scroll event grouping timeout (10ms)**. Groups rapid scroll events together for smoother animation and reduced CPU overhead during fast scrolling. |

### 7.4 Input & Interaction Optimization
Note: browser.tabs.remote.force-enable is already default and the norm, so this section is removed as requested.

---

## 8. Process Architecture & Tab Management 🧵

### 8.1 Multiprocess Configuration
Core process management settings that determine how Zen handles multiple tabs and content isolation with enhanced RAM-specific recommendations.

#### Process Count Recommendations by RAM:
| Setting | 4GB RAM | 8GB RAM | 16GB+ RAM | Risk | Purpose |
|:--------|:--------|:--------|:----------|:-----|:--------|
| `dom.ipc.processCount` | `2` | `4` | `8` | 🟢 | **Number of content processes**. Determines worker processes for parallel tab handling. More processes = better parallelism but higher RAM usage. Use `-1` for unlimited (auto-scaling). |
| `dom.ipc.keepProcessesAlive.web` | `1` | `2` | `4` | 🟢 | **Keep content processes alive**. Maintains process pool for faster new tab creation and switching. Higher values improve responsiveness but use more RAM. |

### 8.2 Process Priority & Resource Management
Advanced process scheduling and resource allocation controls.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `dom.ipc.processPriorityManager.backgroundUsesEcoQoS` | `false` | 🟢 | **Disable Windows 11 EcoQoS for background tabs**. Prevents Windows from forcing background tabs into low-priority "Efficiency Mode," ensuring background tabs remain responsive when switching back. |
| `accessibility.force_disabled` | `1` | 🟢 | **Disable accessibility services**. Turns off accessibility features (screen readers, etc.) to save CPU and memory resources. ⚠️ **Only disable if you don't need accessibility features**. |

### 8.3 Site Isolation & Security
Controls how different websites are isolated from each other for security and performance. Note: When disabling Fission, testing showed ~20% RAM decrease but security trade-offs must be considered, especially without external protection like ESET antivirus.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `fission.autostart` | `false` | 🔴 | **Disable site isolation (Fission)**. **ADVANCED USERS ONLY**. Disables per-site process isolation to reduce memory usage by ~20%. ⚠️ **Significant security trade-off** - sites can potentially access each other's data. Only consider with strong external security (ESET, NextDNS, etc.) and if extremely RAM-constrained. |

---

## 9. Media Playback & Codec Optimization 🎥

### 9.1 Codec Configuration & Performance
Advanced codec settings for optimal video/audio playback quality and compatibility. Note: Hardware video decoding, GPU process decoder, and RDD process are already enabled by default and Firefox automatically detects GPU availability.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `dom.media.webcodecs.h265.enabled` | `true` | 🟢 | **Enable H.265/HEVC in WebCodecs API**. Enables hardware-accelerated H.265 decoding for modern video content with better compression efficiency than H.264. |
| `media.wmf.hevc.enabled` | `true` | 🟢 | **Enable HEVC/H.265 codec support (Windows)**. 🪟 Enables hardware-accelerated H.265 video decoding on Windows systems with compatible hardware, essential for modern high-efficiency video content. |

### 9.2 Picture-in-Picture & Advanced Features
Controls for modern video features and user experience enhancements.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `media.videocontrols.picture-in-picture.enable-when-switching-tabs.enabled` | `true` | 🟢 | **Auto-PiP when switching tabs**. Automatically activates Picture-in-Picture when switching away from video tabs, maintaining video visibility during multitasking. Popular feature for video consumption. |

### 9.3 Platform-Specific Media Optimization
Specialized settings for different operating systems' media capabilities.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `media.ffmpeg.vaapi.enabled` | `true` | 🟢 | **Enable VAAPI hardware decoding (Linux)**. 🐧 Activates Video Acceleration API on Linux systems for GPU-accelerated video decoding, reducing CPU usage and improving battery life on laptops. |
| `media.hardware-video-decoding.force-enabled` | `true` | 🟡 | **Force hardware video decoding**. 🐧 Forces hardware acceleration even on systems where it might be disabled by default. Useful for Linux systems with proper drivers. |

---

## 10. Security, Privacy & Tracking Protection 🔒

### 10.1 Enhanced Tracking Protection
Modern privacy features that block trackers while maintaining performance and compatibility.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `privacy.trackingprotection.enabled` | `true` | 🟢 | **Enable Firefox tracking protection**. Activates built-in tracker blocking system that identifies and blocks known tracking domains and scripts. |
| `privacy.query_stripping.enabled` | `true` | 🟢 | **Strip tracking parameters from URLs**. Automatically removes tracking query strings (utm_source, fbclid, etc.) during navigation, making shared links cleaner and improving privacy. |
| `privacy.query_stripping.enabled.pbmode` | `true` | 🟢 | **Strip tracking parameters in Private Browsing**. Extends query parameter stripping to Private Browsing mode for enhanced privacy during sensitive browsing sessions. |

### 10.2 Referrer Policy & Cross-Site Controls
Advanced privacy controls that prioritize compatibility and performance over strict privacy to prevent website breakage.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `network.http.referer.XOriginPolicy` | `0` | 🟢 | **Compatibility-first referrer policy**. Sends full referrer information to ensure website functionality. Prioritizes performance and compatibility over strict privacy. Use `2` for stricter privacy if willing to accept potential website breakage. |
| `network.http.referer.XOriginTrimmingPolicy` | `0` | 🟢 | **Full referrer for compatibility**. Sends complete referrer information including path and query to prevent website functionality issues. Use `2` to strip path/query for enhanced privacy. |

### 10.3 Network State & Resource Sharing
Privacy vs performance trade-offs for network resource management.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `privacy.partition.network_state` | `false` | 🟡 | **Share network cache globally**. Disables per-site network cache partitioning, allowing shared DNS/HTTP/TLS caches across all sites for performance improvement. ⚠️ **Reduces privacy protection** - acceptable if using external DNS filtering (NextDNS) or ad blockers. |

### 10.4 Safe Browsing & Download Protection
Security features that protect against malicious content.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `browser.safebrowsing.downloads.remote.enabled` | `false` | 🟡 | **Disable Google remote download scanning**. Stops additional cloud-based scanning of downloaded executables by Google servers. **Local Safe Browsing checks still run**. Slightly improves download speeds with minimal security trade-off. |

---

## 11. Platform-Specific Optimizations 💻

### 11.1 Windows-Specific Performance 🪟
Optimizations tailored for Windows operating system features and behaviors with enhanced theme customization.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `config.trim_on_minimize` | `true` | ⚡ | **Aggressive memory trimming on minimize**. When Zen is minimized, Windows aggressively frees memory and restores it on maximize. **Gaming optimization** - frees RAM for other applications when browser is minimized. Windows only. |
| `timer.auto_increase_timer_resolution` | `true` | 🟢 | **High-resolution Windows timer**. Temporarily increases system timer resolution to ~1ms precision when needed for animations and media. Improves smoothness with negligible CPU cost (default Windows timer is 15.6ms). |
| `widget.windows.mica` | `false` | 🟢 | **Disable Windows 11 Mica effect**. When disabled, browser theme becomes darker. Combine with `widget.windows.mica.toplevel-backdrop = 3` for even darker appearance. Ideal for users wanting full dark theme. |
| `widget.windows.mica.toplevel-backdrop` | `3` | 🟢 | **MicaAlt backdrop (darkest variant)**. `0` = Auto, `1` = Mica, `2` = Acrylic, `3` = MicaAlt (darkest). When Mica is disabled, this creates the darkest possible theme. |

### 11.2 Linux-Specific Optimizations 🐧
Performance and compatibility improvements for Linux distributions with additional useful settings.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `media.ffmpeg.vaapi.enabled` | `true` | 🟢 | **Enable VAAPI hardware video decoding**. Activates Video Acceleration API for GPU-accelerated video decoding on Linux systems. Significantly reduces CPU usage and improves battery life during video playback. |
| `widget.wayland.opaque-region.enabled` | `true` | 🟢 | **Optimize Wayland rendering**. Improves rendering performance on Wayland compositors by providing opacity information to the compositor. |
| `widget.wayland.fractional-scale.enabled` | `true` | 🟢 | **Enable fractional scaling on Wayland**. Provides better HiDPI support on Wayland systems with non-integer scaling factors. |

### 11.3 macOS-Specific Features 🍎
Apple platform optimizations and native feature integration with additional settings.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `zen.haptic-feedback.enabled` | `true` | 🟢 | **Enable macOS haptic feedback**. Provides tactile vibration feedback through MacBook trackpads during UI interactions like tab dragging or theme changes. Enhances user experience on supported hardware. |
| `widget.macos.titlebar-blend` | `true` | 🟢 | **Enable macOS titlebar blending**. Provides native macOS window appearance with proper titlebar integration and transparency effects. |

---

## 12. Zen Browser Exclusive Features 🦊

### 12.1 Visual Theme & Appearance
Zen Browser's unique customization options for personalized browsing experience with enhanced gradient control.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `zen.theme.accent-color` | `#ffffff90` | 🟢 | **Primary accent color with transparency**. Sets UI accent color with alpha channel support (90 = 90% opacity). Customize to match your preferred color scheme and transparency level. |
| `zen.theme.border-radius` | `8` | 🟢 | **UI element corner roundness**. Controls corner radius of buttons, tabs, and interface elements. `0` = sharp corners (classic), `8` = smooth rounded corners (modern), higher values = more rounded. |
| `zen.theme.content-element-separation` | `8` | 🟢 | **Spacing between UI elements**. Controls gaps and borders between interface components. `0` = no separation (compact), `8` = visible spacing (readable), higher = more separated. |
| `zen.theme.gradient` | `false` | 🟢 | **Disable gradient theme**. When disabled, removes all gradient effects from Zen theme while keeping Mica effects intact. Actually improves Mica appearance by making it more vibrant. For non-Mica users, creates even darker theme. |
| `zen.theme.gradient.show-custom-colors` | `true` | 🟢 | **Show gradient customization UI**. Enables advanced color picker in theme editor for creating custom gradient combinations with transparency support. |
| `zen.theme.acrylic-elements` | `true` | 🟢 | **Enable acrylic (blur) effects**. Activates translucent blur effects on toolbars, tabs, and UI elements for modern glass-like appearance. May impact performance on older hardware. |

### 12.2 Navigation & URL Bar Behavior
Zen's innovative approach to address bar and navigation.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `zen.urlbar.replace-newtab` | `true` | 🟢 | **Floating URL bar instead of new tab page**. When opening new tab, shows floating URL bar overlay instead of traditional new tab page. Saves vertical space and provides faster navigation. |

### 12.3 Workspace & Tab Management
Advanced tab organization features unique to Zen Browser.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `zen.workspaces.open-new-tab-if-last-unpinned-tab-is-closed` | `true` | 🟢 | **Auto-create tab when workspace empties**. Automatically opens new tab when the last unpinned tab in a workspace is closed, preventing empty workspaces. Set to `false` to allow empty workspaces. |

### 12.4 Reading & Content Features
Enhanced content consumption features in Zen Browser.

| Setting | Value | Risk | Purpose |
|:--------|:------|:-----|:--------|
| `reader.parse-on-load.enabled` | `false` | 🟢 | **Disable automatic Reader Mode parsing**. Stops automatic scanning for Reader Mode eligibility on every page load. Saves DOM re-parsing overhead for faster page loads and reduced memory usage. Manual Reader Mode still available via button. |

---

## 13. External Tools & System Integration 🛠️

### 13.1 Privacy & Security Tools
External applications that complement browser optimizations for enhanced privacy and security.

**NextDNS - Advanced DNS Protection**: Cloud-based DNS service with intelligent ad/tracker blocking, malware protection, and geo-optimized routing. Provides faster DNS resolution than ISP defaults and reduces bandwidth from blocked content. Works seamlessly with browser optimizations, especially when `privacy.partition.network_state = false`.

**Ghostery - Lightweight Privacy Protection**: Privacy-focused ad blocker optimized for performance over feature completeness. Lower memory usage than uBlock Origin on RAM-constrained systems, making it ideal for systems with less than 16GB RAM.

**Consent-O-Matic - Cookie Consent Automation**: Automatically handles cookie consent banners by selecting privacy-friendly options. Unlike simple cookie blockers, actually responds to consent dialogs without breaking functionality, eliminating manual consent clicking and preventing layout shifts.

### 13.2 System Optimization Tools
Windows utilities that enhance overall system performance for better browser experience.

**Quick CPU - Advanced Power Management**: Provides granular control over CPU frequency scaling, power states, and performance profiles. Prevents CPU throttling during intensive browsing sessions. Configure performance governor during browsing with frequency scaling during idle.

**MemReduct - Intelligent Memory Management**: Real-time memory optimization with aggressive trimming capabilities. Frees unused memory during intensive browsing sessions. Most beneficial on systems with 8-16GB RAM during heavy multitasking. Set conservative auto-cleaning thresholds to avoid performance hiccups.

---

## 14. User.js Configuration

### Understanding User.js Files
A user.js file automatically applies Firefox/Zen preferences on every browser startup, eliminating the need to manually configure settings through about:config.

**Location**: Place the user.js file directly in your Firefox/Zen profile folder (find via `about:support` → "Open Profile Folder").

**Important**: Settings apply after restarting the browser. Remove or rename the file to revert to defaults.

---

### 🖥️ High-End System (16GB+ RAM, Dedicated GPU)
**Best for**: Gaming rigs, workstations, content creators
**Features**: Maximum performance, disabled disk cache, aggressive media caching

```javascript
user_pref("browser.preferences.defaultPerformanceSettings.enabled", false);
user_pref("network.http.max-connections", 1200);
user_pref("network.http.max-persistent-connections-per-server", 8);
user_pref("network.dnsCacheExpiration", 600);
user_pref("network.dnsCacheEntries", 10000);
user_pref("network.ssl_tokens_cache_capacity", 32768);
user_pref("browser.cache.disk.enable", false);
user_pref("browser.cache.disk.capacity", 0);
user_pref("browser.cache.disk.smart_size.enabled", false);
user_pref("browser.cache.memory.capacity", 131072);
user_pref("browser.cache.memory.max_entry_size", 32768);
user_pref("javascript.options.mem.high_water_mark", 128);
user_pref("image.mem.max_decoded_image_kb", 512000);
user_pref("media.memory_cache_max_size", 1048576);
user_pref("media.memory_caches_combined_limit_kb", 4194304);
user_pref("content.notify.interval", 50000);
user_pref("content.max.tokenizing.time", 2000000);
user_pref("content.switch.threshold", 250000);
user_pref("dom.ipc.processCount", 8);
user_pref("dom.ipc.keepProcessesAlive.web", 4);
user_pref("gfx.webrender.all", true);
user_pref("layers.acceleration.force-enabled", true);
user_pref("gfx.canvas.accelerated.cache-items", 32768);
user_pref("dom.webgpu.enabled", true);
user_pref("general.smoothScroll.msdPhysics.enabled", false);
user_pref("general.smoothScroll.currentVelocityWeighting", 0);
user_pref("apz.overscroll.enabled", false);
user_pref("general.smoothScroll.stopDecelerationWeighting", 1);
user_pref("general.smoothScroll.mouseWheel.durationMaxMS", 150);
user_pref("general.smoothScroll.mouseWheel.durationMinMS", 50);
user_pref("mousewheel.min_line_scroll_amount", 18);
user_pref("mousewheel.scroll_series_timeout", 10);
user_pref("config.trim_on_minimize", true);
user_pref("dom.media.webcodecs.h265.enabled", true);
user_pref("media.wmf.hevc.enabled", true);
user_pref("media.ffmpeg.vaapi.enabled", true);
```

---

### 💻 Mainstream System (8GB RAM, Integrated/Dedicated GPU)
**Best for**: Most users, general browsing, multimedia consumption
**Features**: Balanced performance, moderate caching, stable configuration

```javascript
user_pref("browser.preferences.defaultPerformanceSettings.enabled", false);
user_pref("network.http.max-connections", 1200);
user_pref("network.http.max-persistent-connections-per-server", 8);
user_pref("network.dnsCacheExpiration", 600);
user_pref("network.dnsCacheEntries", 10000);
user_pref("browser.cache.disk.enable", true);
user_pref("browser.cache.disk.capacity", 2097152);
user_pref("browser.cache.disk.smart_size.enabled", false);
user_pref("browser.cache.memory.capacity", 65536);
user_pref("browser.cache.memory.max_entry_size", 32768);
user_pref("javascript.options.mem.high_water_mark", 128);
user_pref("image.mem.max_decoded_image_kb", 512000);
user_pref("media.memory_cache_max_size", 524288);
user_pref("media.memory_caches_combined_limit_kb", 2097152);
user_pref("content.notify.interval", 100000);
user_pref("content.max.tokenizing.time", 1000000);
user_pref("content.switch.threshold", 500000);
user_pref("dom.ipc.processCount", 4);
user_pref("dom.ipc.keepProcessesAlive.web", 2);
user_pref("gfx.webrender.all", true);
user_pref("layers.acceleration.force-enabled", true);
user_pref("general.smoothScroll.msdPhysics.enabled", false);
user_pref("general.smoothScroll.currentVelocityWeighting", 0);
user_pref("apz.overscroll.enabled", false);
user_pref("general.smoothScroll.stopDecelerationWeighting", 1);
user_pref("general.smoothScroll.mouseWheel.durationMaxMS", 150);
user_pref("general.smoothScroll.mouseWheel.durationMinMS", 50);
user_pref("mousewheel.min_line_scroll_amount", 18);
user_pref("mousewheel.scroll_series_timeout", 10);
user_pref("config.trim_on_minimize", true);
user_pref("dom.media.webcodecs.h265.enabled", true);
user_pref("media.wmf.hevc.enabled", true);
user_pref("media.ffmpeg.vaapi.enabled", true);
```

---

### 📱 Budget System (4GB RAM, Integrated GPU)
**Best for**: Older laptops, budget PCs, limited resources
**Features**: Conservative settings, minimal resource usage, maximum compatibility

```javascript
user_pref("browser.preferences.defaultPerformanceSettings.enabled", false);
user_pref("network.http.max-connections", 800);
user_pref("network.http.max-persistent-connections-per-server", 6);
user_pref("network.dnsCacheExpiration", 600);
user_pref("network.dnsCacheEntries", 5000);
user_pref("browser.cache.disk.enable", true);
user_pref("browser.cache.disk.capacity", 1048576);
user_pref("browser.cache.disk.smart_size.enabled", false);
user_pref("browser.cache.memory.capacity", 32768);
user_pref("browser.cache.memory.max_entry_size", 16384);
user_pref("javascript.options.mem.high_water_mark", 64);
user_pref("media.memory_cache_max_size", 262144);
user_pref("media.memory_caches_combined_limit_kb", 1048576);
user_pref("content.notify.interval", 200000);
user_pref("content.max.tokenizing.time", 500000);
user_pref("content.switch.threshold", 750000);
user_pref("dom.ipc.processCount", 2);
user_pref("dom.ipc.keepProcessesAlive.web", 1);
user_pref("gfx.webrender.all", true);
user_pref("general.smoothScroll.msdPhysics.enabled", false);
user_pref("general.smoothScroll.currentVelocityWeighting", 0);
user_pref("apz.overscroll.enabled", false);
user_pref("general.smoothScroll.stopDecelerationWeighting", 1);
user_pref("general.smoothScroll.mouseWheel.durationMaxMS", 150);
user_pref("general.smoothScroll.mouseWheel.durationMinMS", 50);
user_pref("mousewheel.min_line_scroll_amount", 18);
user_pref("mousewheel.scroll_series_timeout", 10);
user_pref("config.trim_on_minimize", true);
user_pref("media.wmf.hevc.enabled", true);
user_pref("media.ffmpeg.vaapi.enabled", true);
```

---

### 🎮 Gaming-Optimized (Any RAM, Focus on Performance)
**Best for**: Gamers who minimize browser while gaming
**Features**: Aggressive memory trimming, maximum performance when active

```javascript
user_pref("browser.preferences.defaultPerformanceSettings.enabled", false);
user_pref("network.http.max-connections", 1200);
user_pref("network.http.max-persistent-connections-per-server", 8);
user_pref("network.dnsCacheExpiration", 600);
user_pref("network.dnsCacheEntries", 10000);
user_pref("browser.cache.disk.enable", false);
user_pref("browser.cache.disk.capacity", 0);
user_pref("browser.cache.memory.capacity", 65536);
user_pref("javascript.options.mem.high_water_mark", 128);
user_pref("media.memory_cache_max_size", 524288);
user_pref("dom.ipc.processCount", 4);
user_pref("dom.ipc.keepProcessesAlive.web", 2);
user_pref("gfx.webrender.all", true);
user_pref("layers.acceleration.force-enabled", true);
user_pref("general.smoothScroll.msdPhysics.enabled", false);
user_pref("general.smoothScroll.currentVelocityWeighting", 0);
user_pref("apz.overscroll.enabled", false);
user_pref("general.smoothScroll.stopDecelerationWeighting", 1);
user_pref("general.smoothScroll.mouseWheel.durationMaxMS", 150);
user_pref("general.smoothScroll.mouseWheel.durationMinMS", 50);
user_pref("mousewheel.min_line_scroll_amount", 18);
user_pref("mousewheel.scroll_series_timeout", 10);
user_pref("config.trim_on_minimize", true);
user_pref("dom.ipc.processPriorityManager.backgroundUsesEcoQoS", false);
```

### Installation Instructions
1. **Find Profile Folder**: Go to `about:support` → Click "Open Profile Folder"
2. **Create File**: Create new text file named exactly `user.js` (not user.js.txt)
3. **Copy Settings**: Copy one of the above configurations
4. **Restart Browser**: Close and restart Zen/Firefox completely
5. **Verify**: Check `about:config` to confirm settings are applied

**Removal**: Delete or rename the user.js file and restart to revert all changes.

---

## 15. Maintenance, Diagnostics & Troubleshooting 🔧

### 15.1 Post-Optimization Maintenance
Essential maintenance tasks to ensure optimizations remain effective and databases stay healthy.

#### Database Integrity Verification
After applying optimizations, verify browser databases are functioning correctly:

1. **Navigate to `about:support`** (Troubleshooting Information page)
2. **Scroll to "Places Database" section**
3. **Click "Verify Integrity"** - This checks and repairs bookmarks/history database (places.sqlite)
4. **Look for "OK" status** - If corrupt, this will rebuild the database automatically
5. **Restart browser** if any repairs were performed

#### Startup Cache Clearing
Clear internal component caches to ensure new settings take effect:

1. **In `about:support`, find "Application Basics" section**
2. **Click "Clear startup cache"** (near the top)
3. **Restart Firefox/Zen Browser immediately**
4. **This clears Firefox's internal UI and component cache**
5. **Ensures preference changes are recognized cleanly**

**Important**: These maintenance steps do NOT delete browsing data, bookmarks, passwords, or extensions.

### 15.2 Performance Monitoring & Validation
Built-in tools for monitoring the effectiveness of your optimizations.

#### Essential Diagnostic URLs
| URL | Purpose | When to Use |
|:----|:--------|:------------|
| `about:memory` | **Detailed memory analysis per process/module**. Shows exact RAM usage breakdown and provides "Minimize" button for memory cleanup. | Monitor memory usage patterns, identify memory leaks, verify cache size limits are working. |
| `about:performance` | **Real-time task manager** showing CPU/memory impact per tab and extension. Identifies performance bottlenecks. | Identify resource-heavy tabs or extensions, monitor process efficiency after optimizations. |
| `about:processes` | **Low-level per-process inspection**. Shows process IDs, memory usage, and process relationships. | Debug process architecture changes, verify process count settings are active. |
| `about:gpu` | **GPU diagnostics and acceleration status**. Shows WebRender status, hardware acceleration state, and graphics driver information. | Verify GPU optimizations are active, troubleshoot graphics acceleration issues. |
| `about:support` | **Comprehensive configuration summary**. Shows active features, modified preferences, crash information, and system details. | Verify settings are applied correctly, troubleshoot configuration conflicts. |

### 15.3 Troubleshooting Common Issues
Solutions for problems that may arise after applying optimizations.

#### Memory Issues
**Symptoms**: Browser using excessive RAM, system slowdown, out-of-memory errors
**Solutions**:
1. Reduce process count settings for your RAM configuration
2. Decrease media cache limits by 50%
3. Enable disk cache if disabling it caused issues
4. Use `about:memory` → "Minimize memory usage" to test if issue is cache-related

#### Graphics/Rendering Issues
**Symptoms**: Visual glitches, slow animations, WebRender crashes
**Solutions**:
1. Verify graphics drivers are up to date
2. Disable `layers.acceleration.force-enabled` if experiencing crashes
3. Check `about:gpu` for hardware acceleration status and errors
4. Test with software rendering as fallback

#### Website Compatibility Issues
**Symptoms**: Sites not loading correctly, missing functionality, login issues
**Solutions**:
1. Temporarily disable tracking protection on problematic sites
2. Re-enable `privacy.partition.network_state = true` if cross-site functionality breaks
3. Check if site requires specific codecs or WebGL functionality
4. Test with a clean profile to isolate the problematic setting

---

## Conclusion

Key improvements in this version include RAM-specific configurations, enhanced smooth scrolling optimizations, YouTube and content-heavy website performance improvements, comprehensive user.js guidance, and removal of settings that are already enabled by default in modern Firefox/Zen versions.

Remember that browser updates may change default values, so revisit these settings after major version upgrades. All settings are subject to change as Zen developers continue improving the browser over time. Start with the safer optimizations and gradually implement advanced settings based on your comfort level and specific needs.

**Performance Results**: With these optimizations properly applied and configured for your RAM capacity, users can expect 30-50% improvements in benchmark scores and significantly better real-world browsing responsiveness, memory efficiency, and multimedia performance. 
