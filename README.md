# Roman SP

**90-Day Digital Reset** — A system-level content blocker that helps you break free from explicit content. Install once, set a password, hand it to someone you trust. Your phone becomes a healing device.

Roman SP operates as a Device Owner — the highest privilege level on Android. Once active, it cannot be removed, bypassed, or disabled without the protection password. No root required.

---

## How It Works

Roman SP protects at **every layer**. There is no single point of bypass — you'd have to defeat all of them simultaneously.

### 1. DNS Protection
All internet traffic routes through CleanBrowsing's adult filter DNS. Millions of known explicit domains are blocked before they even load. The DNS setting is locked at the system level — it cannot be changed from device settings, and the Private DNS configuration page is restricted.

### 2. Browser Lockdown
Only Chrome and Samsung Browser are allowed. Every other browser is hidden and inaccessible — Firefox, Brave, Opera, Tor, DuckDuckGo, and 50+ others. New browser installs are detected and hidden instantly. DNS-over-HTTPS is disabled on allowed browsers to prevent DNS bypass.

### 3. Screen Guard
Real-time screen monitoring powered by an Accessibility Service. Scans all on-screen text across every app for 300+ NSFW keywords with leetspeak normalization — catches `p0rn`, `p.o.r.n`, `pr0n`, and hundreds of variations. When a threat is detected, a full-screen shield covers the display and the offending app is force-closed.

### 4. AI Scanner
On-device AI that scans your screen in real-time for explicit visual content using machine learning. Uses the `image-safety-classifier-xs` model (SwiftFormer architecture, 97.76% accuracy, ~20-40ms inference). Catches explicit content inside any app — Twitter, Reddit, Telegram, video players, galleries. 100% private — all processing happens on-device, nothing is uploaded.

### 5. Device Restrictions
Critical settings are locked down to prevent tampering:
- Factory reset is blocked from Settings
- Safe boot is disabled
- Network reset is blocked
- New user accounts cannot be added
- USB file transfer is disabled — files can only be shared via apps like WhatsApp

### 6. Backup Protection
All system backup and restore services are disabled. This prevents the bypass strategy of backing up data, factory resetting to remove protection, then restoring on a clean phone.
- Samsung Smart Switch — hidden and disabled
- Samsung Cloud — disabled
- Google backup/restore — disabled at the system level
- Factory reset without backup means losing everything — the cost of bypassing is losing all your photos, messages, apps, and data

### 7. Self-Protection
Roman SP protects itself from removal:
- Cannot be uninstalled — the uninstall button is disabled
- Cannot be disabled through device settings
- Samsung Pass is blocked to prevent autofill from exposing saved credentials on blocked sites
- Survives reboots — auto-starts on boot

### 8. AI Scanner (Visual)
On-device machine learning model scans screen frames in real-time:
- Scans every 750ms on high-risk apps (browsers, social media)
- Adaptive intervals based on app risk level — skips safe apps like calculator and settings
- Force-closes the app instantly when explicit content is detected
- Runs entirely on-device — zero data leaves your phone

---

## Setup

### Requirements
- Android 10+ device (tested on Samsung Galaxy A51, S22 Ultra)
- USB cable + computer with ADB
- Device must be factory reset or have no accounts added yet

### Step 1: Install the APK
Download the latest APK from [Releases](https://github.com/brookly255-lv/roman-sp/releases) or build from source:
```bash
cd android
JAVA_HOME=/path/to/jdk ./gradlew assembleDebug
```
The AI model is bundled inside the APK — no separate download needed.

### Step 2: Install and set as Device Owner
```bash
# Install the APK
adb install -r app/build/outputs/apk/debug/app-debug.apk

# Set as Device Owner (requires no accounts on device)
adb shell dpm set-device-owner com.romanos.app/.admin.RomanAdminReceiver

# Exempt from battery optimization
adb shell dumpsys deviceidle whitelist +com.romanos.app
```

### Step 3: Launch and set password
Open Roman SP from the app drawer. Set a **16+ character password** and give it to someone you trust — not yourself. This password is the only way to access settings or remove the protection.

### Step 4: Grant screen capture
A permission dialog will appear asking to start screen recording. Tap **"Start now"** — this enables the real-time AI scanner. This is a one-time prompt.

That's it. The device is now a healing device.

---

## Protection Status Cards

When you enter the protection password, you see 8 status cards:

| Card | What it monitors |
|------|-----------------|
| **Device Owner** | Roman SP has system-level admin control |
| **DNS Protection** | CleanBrowsing adult filter DNS is locked |
| **Browser Lockdown** | Only approved browsers allowed, all others hidden |
| **Screen Guard** | Real-time keyword detection across all apps |
| **Device Restrictions** | Factory reset, safe boot, USB transfer all blocked |
| **Backup Protection** | Smart Switch, Samsung Cloud, Google Restore all disabled |
| **Self-Protection** | App cannot be uninstalled or disabled |
| **AI Scanner** | On-device ML scanning screen for explicit visuals |

Each card shows ACTIVE/INACTIVE status with detailed explanations of what it does and why.

---

## Architecture

```
Roman SP
├── RomanAdminReceiver     — Device Owner policies (DNS, browsers, backup, restrictions)
├── ScreenGuardService     — Accessibility service orchestrating guards
│   ├── ScreenGuard        — AI visual scanner (MediaProjection → ONNX inference)
│   ├── KeywordGuard       — Real-time text monitoring across all apps
│   └── ForegroundAppTracker — App risk classification for scan intervals
├── RomanScannerService    — File scanner + watchdog + capture manager
├── BootReceiver           — Auto-start on boot
└── PackageWatcher         — Auto-block newly installed browsers
```

## AI Model

Uses **image-safety-classifier-xs** by OwenElliott — a SwiftFormer-XS model (3.5M parameters, 13MB) that classifies images as NSFL/NSFW/SFW. Inference runs at ~20-40ms on mid-range ARM CPUs. The model has preprocessing baked into the ONNX graph (ImageNet normalization), so raw 0-255 pixel values are fed directly.

## Who Is This For?

- People recovering from pornography or explicit content addiction
- Parents who want a locked-down device for their household
- Anyone who wants a 90-day reset to break a digital habit
- Organizations providing accountability devices

Roman SP is not a filter you can turn off when it gets inconvenient. It's a commitment device. The 16+ character password exists so you can give control to someone else — a spouse, a friend, an accountability partner. You choose to lock yourself in.

**Average healing period: 90 days to 5+ months.** Most users report significant improvement within the first 90 days.

## License

MIT
