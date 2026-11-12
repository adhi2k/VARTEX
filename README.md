<p align="center">
  <img src="assets/vortex-banner.jpg" alt="VORTEX Banner" width="100%" />
</p>

<h1 align="center">VORTEX</h1>
<p align="center">
  <strong>ENTER THE VORTEX • PREMIUM CONTROL CENTER</strong>
</p>

<p align="center">
  <a href="https://github.com/adhi2k/VARTEX/releases/latest/download/VORTEX.apk">
    <img src="https://img.shields.io/badge/Download-VORTEX%20v1.0.4-dc2626?style=for-the-badge&logo=android&logoColor=white" alt="Download VORTEX APK" />
  </a>
  <a href="https://t.me/+TzeZYAS3I61hYjJl">
    <img src="https://img.shields.io/badge/Telegram-Support%20%26%20Keys-eab308?style=for-the-badge&logo=telegram&logoColor=black" alt="Telegram Support" />
  </a>
</p>

---

## Overview

**VORTEX** (`com.one.shoot`) is a rebranded Android loader/container APK built around an embedded multi-process virtualization container (`top.bienvenido.mundo`), a dark crimson-and-gold UI layer, floating overlay controls, and a GitHub-backed manual update checker.

---

## APK Static Analysis (`VORTEX.apk`)

### 1. Build & Package Metadata

| Property | Value |
| :--- | :--- |
| **Application Label** | `VORTEX` |
| **Package ID** | `com.one.shoot` |
| **Version** | `1.0.4-beta` (`versionCode: 5`) |
| **SDK Levels** | `minSdkVersion: 24` (Android 7.0) • `targetSdkVersion: 28` (Android 9.0) • `compileSdkVersion: 36` |
| **Supported ABI** | `arm64-v8a` only |
| **APK SHA-256** | `D9F688026E42CC2883AA950CF6DEF5721F3015104F6E99763E13981E16389CE2` |
| **Signing Scheme** | APK Signature Scheme v2 & v3 (`CN=VORTEX, O=VORTEX, C=IN`) |

### 2. Core Components & Architecture

- **Application Class:** `com.pubg.ViralCheatApp` — initializes the app runtime, loads `libplugin_manager.so`, and registers target package references (`com.pubg.imobile`).
- **UI Activity Flow:**
  1. `com.pubg.activity.SplashActivity` (`MAIN` / `LAUNCHER`)
  2. `com.pubg.activity.PermissionGateActivity`
  3. `com.pubg.activity.LoginActivity` (Access Key login & Telegram support links)
  4. `com.pubg.activity.HomeActivity` (VORTEX Control Center dashboard, system status, changelog, and update checker)
- **Background Services:**
  - `com.pubg.Component.HxcFloatingService` — foreground service (`specialUse` / `floating_menu_overlay`) rendering the floating overlay controls.
  - `com.pubg.Component.FilterVpnService` — local Android `VpnService` (`BIND_VPN_SERVICE`).
- **Virtualization Layer (`top.bienvenido.mundo`):**
  - Embeds the Mundo virtual container (`MundoSupervisorProvider`, `MundoSplashLauncher`, `MundoIntermediary` with `FBLoginAbility` URI schemes `fbconnect` / `imobile1450`, `MundoReceiver`, and 24 multi-process stub slots `STUB1`–`STUB24` / `STUB1H`–`STUB24H` across `:stub1`–`:stub24`).
- **Third-Party Libraries:**
  - `androidx.*`, `kotlinx.*`, `okio.*`
  - `com.molihuan.pathselector` & `com.blankj.molihuan.utilcode` (file chooser and utility framework)

### 3. Native Libraries (`lib/arm64-v8a/`)

| Library | Size | Purpose |
| :--- | ---: | :--- |
| `libplugin_manager.so` | 3,502,168 bytes | Primary native core loaded by `ViralCheatApp` and `com.pubg.UrlProvider` (`getApkUrl`, `getLibraryUrl`, `getManifestUrl`, `getVersionUrl`) |
| `libXloxMundo.so` | 866,552 bytes | Mundo Android virtual container native runtime |
| `libmsaoaidsec.so` | 685,960 bytes | MSA OAID (Open Anonymous Device Identifier) SDK |
| `libmmkv.so` | 458,448 bytes | Tencent MMKV high-performance key-value storage |

### 4. Permissions & Security Observations

- **Core Operational Permissions:** `INTERNET`, `ACCESS_NETWORK_STATE`, `ACCESS_WIFI_STATE`, `SYSTEM_ALERT_WINDOW`, `FOREGROUND_SERVICE`, `FOREGROUND_SERVICE_SPECIAL_USE`, `REQUEST_INSTALL_PACKAGES`, `QUERY_ALL_PACKAGES` (manifest `<queries>` for `com.pubg.imobile` and `com.facebook.katana`), `PACKAGE_USAGE_STATS`, `KILL_BACKGROUND_PROCESSES`, `RECEIVE_BOOT_COMPLETED`.
- **Virtual-Container Stub Permissions:** `AndroidManifest.xml` declares a wide set of sensitive permissions (including SMS, Call Log, Contacts, Camera, Microphone, Fine/Background Location, Phone State, `DUMP`, and `MOUNT_UNMOUNT_FILESYSTEMS`), which is characteristic of multi-app virtualization wrappers passing host permissions down to sandboxed apps.
- **Manifest Security Posture:**
  - `android:usesCleartextTraffic="true"` permits non-HTTPS network requests.
  - `android:targetSdkVersion="28"` bypasses newer Android runtime and scoped-storage restrictions (also paired with `requestLegacyExternalStorage="true"`).
  - `android:allowBackup="true"` allows ADB backup extraction unless explicitly disabled.

### 5. VORTEX UI & OTA Update Module

Added `com.pubg.activity.Vortex*` helper classes (`VortexUiHelper`, `VortexUpdateClickListener`, `VortexUpdateCheckTask`, `VortexUpdateResultRunnable`, `VortexDialogLinkListener`, `VortexLinkClickListener`, `VortexToggleClickListener`):
- **Update Endpoint:** [`version.json`](https://raw.githubusercontent.com/adhi2k/VARTEX/main/version.json)
- **Download Endpoint:** [VORTEX.apk](https://github.com/adhi2k/VARTEX/releases/latest/download/VORTEX.apk)
- Compares remote `versionCode` against installed `PackageInfo.versionCode` (`5`) and prompts the user to open `apkUrl` in their browser when a newer release is published.

---

## Support & Releases

- **Latest Release APK:** [Download VORTEX.apk](https://github.com/adhi2k/VARTEX/releases/latest/download/VORTEX.apk)
- **Telegram Support & Access Keys:** [Join Telegram](https://t.me/+TzeZYAS3I61hYjJl)

### Publishing a New Release
1. Upload the signed APK named **`VORTEX.apk`** to a new [GitHub Release](https://github.com/adhi2k/VARTEX/releases).
2. Update `versionCode`, `versionName`, and `changelog` in [`version.json`](version.json) on `main`.
