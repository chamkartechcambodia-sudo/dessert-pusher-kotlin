# 🍰 DessertPusher — Activity Lifecycle

A hands-on practice app for learning **Android Activity Lifecycle** — upgraded to use **Gradle Kotlin DSL + TOML Version Catalog + AGP 9.0.1**.

---

## 🎮 About the App

**DessertPusher** is a simple mini-game:
- Tap the dessert image → earn money
- Sell enough desserts → automatically unlock a more expensive one
- Learning goal: observe the **Activity Lifecycle** callbacks through Logcat

| Screen | Description |
|---|---|
| Tap to sell desserts | Each tap adds revenue and increases count |
| Dessert auto-upgrades | From Cupcake → Donut → ... → Oreo |
| Share results | Share button in the action bar |

---

## 🛠️ Tech Stack

| Component | Version |
|---|---|
| Android Gradle Plugin (AGP) | 9.0.1 |
| Gradle | 9.1.0 |
| Kotlin | *(embedded in AGP 9.x)* |
| `compileSdk` / `targetSdk` | 36 |
| `minSdk` | 30 |
| JVM target | 17 |
| AndroidX AppCompat | 1.7.1 |
| ConstraintLayout | 2.2.1 |
| Lifecycle | 2.10.0 |
| Timber | 5.0.1 |

### Gradle Structure (Kotlin DSL + TOML)

```
dessert-pusher-kotlin/
├── gradle/
│   ├── libs.versions.toml       ← Centralized version declarations
│   └── wrapper/
│       └── gradle-wrapper.properties
├── settings.gradle.kts          ← Project settings (Kotlin DSL)
├── build.gradle.kts             ← Root build file (Kotlin DSL)
└── app/
    └── build.gradle.kts         ← App module build file (Kotlin DSL)
```

> **Why Kotlin DSL + TOML?**
> - **Kotlin DSL** (`.kts`): better autocomplete in Android Studio, type-safe, easier to refactor
> - **TOML Version Catalog**: all versions in one place (`libs.versions.toml`), no duplication, easy to upgrade

---

## 📋 Prerequisites

- **Android Studio** Ladybug (2024.2.x) or later
- **JDK 17** (File → Project Structure → SDK Location → Gradle JDK)
- **Git** installed

> ⚠️ If Android Studio reports a JDK error, go to:
> `File → Settings → Build, Execution, Deployment → Build Tools → Gradle → Gradle JVM` → select **JDK 17**

---

## 📂 Branch Structure

Each lesson step has **2 branches**: `Exercise` (your task) and `Solution` (the answer).

```
master
├── Step.01-Exercise-Logging-a-callback           ← Exercise Step 1
├── Step.01-Solution-Logging-a-callback           ← Solution Step 1
├── Step.02-Exercise-Timber-for-logging           ← Exercise Step 2
├── Step.02-Solution-Timber-for-logging           ← Solution Step 2
├── Step.03-Exercise-Setup-and-tear-down          ← Exercise Step 3
├── Step.03-Solution-Setup-and-tear-down          ← Solution Step 3
├── Step.04-Exercise-Add-the-lifecycle-library    ← Exercise Step 4
├── Step.04-Solution-Add-the-lifecycle-library    ← Solution Step 4
├── Step.05-Exercise-Implement-onSaveInstanceState  ← Exercise Step 5
└── Step.05-Solution-Implement-onSaveInstanceState  ← Solution Step 5
```

### Step Details

#### 🔵 Step 01 — Logging a Callback
**Goal:** Understand when `onCreate()` and `onStart()` are called.

| TODO | Location | Task |
|---|---|---|
| TODO 01 | `MainActivity.kt` | Add `Log.i()` inside `onCreate()` |
| TODO 02 | `MainActivity.kt` | Override `onStart()` and add a log statement |

```kotlin
// Example:
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    Log.i("MainActivity", "onCreate called")
}
```

---

#### 🔵 Step 02 — Timber for Logging
**Goal:** Replace `Log` with the **Timber** library — a more professional logging approach.

| TODO | Location | Task |
|---|---|---|
| TODO 02 | `PusherApplication.kt` | Create the Application class |
| TODO 03 | `AndroidManifest.xml` | Register the Application class |
| TODO 04 | `PusherApplication.kt` | Initialize Timber: `Timber.plant(Timber.DebugTree())` |
| TODO 05 | `MainActivity.kt` | Override all remaining lifecycle methods and log with Timber |

```kotlin
// With Timber:
Timber.i("onCreate called")
// Instead of:
Log.i("MainActivity", "onCreate called")
```

---

#### 🔵 Step 03 — Setup and Tear Down
**Goal:** Use `onStart()` / `onStop()` to start and stop the `DessertTimer`.

| TODO | Location | Task |
|---|---|---|
| TODO 02 | `MainActivity.kt` | Create an instance of `DessertTimer` |
| TODO 03 | `MainActivity.kt` | Call `timer.startTimer()` in `onStart()` and `timer.stopTimer()` in `onStop()` |

> **Lesson:** Resources should be initialized in `onStart()` and released in `onStop()` to avoid memory leaks.

---

#### 🔵 Step 04 — Add the Lifecycle Library
**Goal:** Use **Lifecycle-Aware Components** — `DessertTimer` manages its own lifecycle without `MainActivity` calling it manually.

| TODO | Location | Task |
|---|---|---|
| TODO 04 | `MainActivity.kt` | Pass `this.lifecycle` into `DessertTimer` |
| TODO 05 | `MainActivity.kt` | Remove the manual `startTimer()` / `stopTimer()` calls |

```kotlin
// Before (Step 03):
dessertTimer = DessertTimer()
// After (Step 04):
dessertTimer = DessertTimer(this.lifecycle)
```

> **Lesson:** `LifecycleObserver` allows a component to observe lifecycle events on its own → cleaner code, fewer bugs.

---

#### 🔵 Step 05 — Implement onSaveInstanceState
**Goal:** Save and restore UI state when the screen rotates or the app is killed by the system.

| TODO | Location | Task |
|---|---|---|
| TODO 01 | `MainActivity.kt` | Override `onSaveInstanceState()` and `onRestoreInstanceState()` |
| TODO 02 | `MainActivity.kt` | Save `revenue`, `dessertsSold`, and `timerSeconds` into the Bundle |
| TODO 03 | `MainActivity.kt` | Read the data back from the Bundle inside `onCreate()` |

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putInt(KEY_REVENUE, revenue)
    outState.putInt(KEY_DESSERT_SOLD, dessertsSold)
    super.onSaveInstanceState(outState)
}
```

> **Lesson:** UI data is lost on screen rotation unless saved to `savedInstanceState`.

---

## 🚀 How to Use This Repository

### 1. Clone the repo

```bash
git clone https://github.com/chamkartechcambodia-sudo/dessert-pusher-kotlin.git
cd dessert-pusher-kotlin
```

### 2. Open in Android Studio

```
File → Open → select the dessert-pusher-kotlin folder
```

Wait for the Gradle sync to complete.

### 3. Check out the exercise branch

```bash
# Example: work on Step 1 exercise
git checkout Step.01-Exercise-Logging-a-callback
```

### 4. Find and complete the TODOs

In Android Studio, open the **TODO panel**:
> `View → Tool Windows → TODO`

TODOs are numbered — complete them in order from top to bottom.

### 5. Compare with the solution

```bash
# View the solution for Step 1
git checkout Step.01-Solution-Logging-a-callback

# Or compare the diff locally
git diff Step.01-Exercise-Logging-a-callback Step.01-Solution-Logging-a-callback
```

### 6. Observe the Lifecycle in Logcat

After running the app, open **Logcat** in Android Studio and filter by tag:

```
Tag: MainActivity
```

Try these actions and observe the logs:

| Action | Callbacks triggered |
|---|---|
| Open the app | `onCreate` → `onStart` → `onResume` |
| Press Home | `onPause` → `onStop` |
| Return to app | `onRestart` → `onStart` → `onResume` |
| Rotate screen | `onPause` → `onStop` → `onDestroy` → `onCreate` → ... |
| Close the app | `onPause` → `onStop` → `onDestroy` |

---

## 📊 Activity Lifecycle Diagram

```
              App launched
              (savedInstanceState = null)
                   │
                   ▼
            ┌────────────┐
            │ onCreate() │◄─────────────────────────────────────┐
            └─────┬──────┘                                      │
                  ▼                                             │ screen rotated /
            ┌────────────┐◄──────────────────┐                  │ activity recreated
            │ onStart()  │                   │                  │
            └─────┬──────┘                   │                  │
                  ▼                    ┌─────┴──────┐           │
            ┌────────────┐             │onRestart() │           │
     ┌─────►│ onResume() │             └─────┬──────┘           │
     │      └─────┬──────┘                   │                  │
     │            │ App running              │ user returns     │
     │            ▼                          │ (process alive)  │
     │      ┌────────────┐                   │                  │
     │      │ onPause()  │                   │                  │
     │      └─────┬──────┘                   │                  │
     └────────────┘ user returns             │                  │
      (partially covered)                    │                  │
                  │ fully hidden             │                  │
                  ▼                          │                  │
            ┌────────────┐                   │                  │
            │  onStop()  │◄──────────────────┘                  │
            │            │                                      │
            │onSaveInstance                                     │
            │  State()   │──── process killed by system ────────┘
            └─────┬──────┘     (no onDestroy called!)
                  │                          user returns →  onCreate(savedInstanceState)
                  ▼ finish() / system
            ┌────────────┐
            │onDestroy() │
            └────────────┘
```

> **Key insight for Step 05:** When the system kills the process (low memory), `onDestroy()` is **never called**.
> `onSaveInstanceState()` is the only way to preserve UI state across process death.

---

## 📁 Source Code Structure

```
app/src/main/java/com/example/android/dessertpusher/
├── MainActivity.kt        ← Main screen: game logic + lifecycle callbacks
├── DessertTimer.kt        ← Timer integrated with LifecycleObserver
└── PusherApplication.kt  ← Timber logging initialization
```

---

## 💡 Tips for Students

1. **Read the TODO comments carefully** before writing code — they explain exactly what to do
2. **Run the app after each TODO** to see the result in Logcat immediately
3. **Do not look at the Solution** before finishing the Exercise on your own
4. If you get stuck, use `git diff` to compare with the Solution branch
5. **Each branch is independent** — changes on one branch do not affect others

---

## 🐛 Report Issues

Found a bug or issue in this project? Please open a [GitHub Issue](https://github.com/chamkartechcambodia-sudo/dessert-pusher-kotlin/issues).
