# 🍰 DessertPusher — Activity Lifecycle

Ứng dụng thực hành cho bài học **Android Activity Lifecycle** — được nâng cấp lên **Gradle Kotlin DSL + TOML Version Catalog + AGP 9.0.1**.

---

## 🎮 Giới thiệu ứng dụng

**DessertPusher** là một mini-game đơn giản:
- Nhấn vào hình ảnh món tráng miệng → kiếm tiền
- Bán đủ số lượng → tự động mở khóa món đắt hơn
- Mục tiêu học tập: quan sát **vòng đời (Lifecycle)** của một Activity thông qua Logcat

| Màn hình | Mô tả |
|---|---|
| Nhấn để bán dessert | Mỗi lần nhấn cộng tiền và tăng số lượng |
| Dessert tự động nâng cấp | Từ Cupcake → Donut → ... → Oreo |
| Chia sẻ kết quả | Nút Share trên thanh menu |

---

## 🛠️ Tech Stack

| Thành phần | Phiên bản |
|---|---|
| Android Gradle Plugin (AGP) | 9.0.1 |
| Gradle | 9.1.0 |
| Kotlin | — *(nhúng trong AGP 9.x)* |
| `compileSdk` / `targetSdk` | 36 |
| `minSdk` | 30 |
| JVM target | 17 |
| AndroidX AppCompat | 1.7.1 |
| ConstraintLayout | 2.2.1 |
| Lifecycle | 2.10.0 |
| Timber | 5.0.1 |

### Cấu trúc Gradle (Kotlin DSL + TOML)

```
dessert-pusher-kts/
├── gradle/
│   ├── libs.versions.toml       ← Khai báo tất cả versions tập trung
│   └── wrapper/
│       └── gradle-wrapper.properties
├── settings.gradle.kts          ← Cấu hình project (Kotlin DSL)
├── build.gradle.kts             ← Root build file (Kotlin DSL)
└── app/
    └── build.gradle.kts         ← App module build file (Kotlin DSL)
```

> **Tại sao dùng Kotlin DSL + TOML?**
> - **Kotlin DSL** (`.kts`): autocomplete tốt hơn trong Android Studio, type-safe, dễ refactor
> - **TOML Version Catalog**: quản lý tất cả versions ở một chỗ (`libs.versions.toml`), tránh duplicate, dễ nâng cấp

---

## 📋 Yêu cầu cài đặt

- **Android Studio** Ladybug (2024.2.x) trở lên
- **JDK 17** (File → Project Structure → SDK Location → Gradle JDK)
- **Git** đã cài đặt

> ⚠️ Nếu Android Studio báo lỗi JDK, vào:
> `File → Settings → Build, Execution, Deployment → Build Tools → Gradle → Gradle JVM` → chọn **JDK 17**

---

## 📂 Cấu trúc Branches

Mỗi bước học có **2 branches**: `Exercise` (bài tập) và `Solution` (đáp án).

```
master
├── Step.01-Exercise-Logging-a-callback      ← Bài tập Step 1
├── Step.01-Solution-Logging-a-callback      ← Đáp án Step 1
├── Step.02-Exercise-Timber-for-logging      ← Bài tập Step 2
├── Step.02-Solution-Timber-for-logging      ← Đáp án Step 2
├── Step.03-Exercise-Setup-and-tear-down     ← Bài tập Step 3
├── Step.03-Solution-Setup-and-tear-down     ← Đáp án Step 3
├── Step.04-Exercise-Add-the-lifecycle-library   ← Bài tập Step 4
├── Step.04-Solution-Add-the-lifecycle-library   ← Đáp án Step 4
├── Step.05-Exercise-Implement-onSaveInstanceState  ← Bài tập Step 5
└── Step.05-Solution-Implement-onSaveInstanceState  ← Đáp án Step 5
```

### Chi tiết từng Step

#### 🔵 Step 01 — Logging a Callback
**Mục tiêu:** Hiểu `onCreate()` và `onStart()` là gì, khi nào chúng được gọi.

| TODO | Vị trí | Nhiệm vụ |
|---|---|---|
| TODO 01 | `MainActivity.kt` | Thêm `Log.i()` trong `onCreate()` |
| TODO 02 | `MainActivity.kt` | Override `onStart()` và thêm log |

```kotlin
// Ví dụ:
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    Log.i("MainActivity", "onCreate called")
}
```

---

#### 🔵 Step 02 — Timber for Logging
**Mục tiêu:** Thay thế `Log` bằng thư viện **Timber** — cách logging chuyên nghiệp hơn.

| TODO | Vị trí | Nhiệm vụ |
|---|---|---|
| TODO 02 | `PusherApplication.kt` | Tạo Application class |
| TODO 03 | `AndroidManifest.xml` | Đăng ký Application class |
| TODO 04 | `PusherApplication.kt` | Khởi tạo Timber: `Timber.plant(Timber.DebugTree())` |
| TODO 05 | `MainActivity.kt` | Override tất cả lifecycle methods và log bằng Timber |

```kotlin
// Ví dụ dùng Timber:
Timber.i("onCreate called")
// Thay vì:
Log.i("MainActivity", "onCreate called")
```

---

#### 🔵 Step 03 — Setup and Tear Down
**Mục tiêu:** Dùng `onStart()` / `onStop()` để khởi động và dừng `DessertTimer`.

| TODO | Vị trí | Nhiệm vụ |
|---|---|---|
| TODO 02 | `MainActivity.kt` | Tạo instance của `DessertTimer` |
| TODO 03 | `MainActivity.kt` | Gọi `timer.startTimer()` trong `onStart()` và `timer.stopTimer()` trong `onStop()` |

> **Bài học:** Tài nguyên nên được khởi tạo ở `onStart()` và giải phóng ở `onStop()` để tránh leak.

---

#### 🔵 Step 04 — Add the Lifecycle Library
**Mục tiêu:** Dùng **Lifecycle-Aware Components** — `DessertTimer` tự quản lý lifecycle mà không cần `MainActivity` gọi thủ công.

| TODO | Vị trí | Nhiệm vụ |
|---|---|---|
| TODO 04 | `MainActivity.kt` | Truyền `this.lifecycle` vào `DessertTimer` |
| TODO 05 | `MainActivity.kt` | Xóa các lệnh gọi `startTimer()` / `stopTimer()` thủ công |

```kotlin
// Trước (Step 03):
dessertTimer = DessertTimer()
// Sau (Step 04):
dessertTimer = DessertTimer(this.lifecycle)
```

> **Bài học:** `LifecycleObserver` giúp component tự lắng nghe lifecycle → code sạch hơn, ít bug hơn.

---

#### 🔵 Step 05 — Implement onSaveInstanceState
**Mục tiêu:** Lưu và khôi phục trạng thái khi xoay màn hình hoặc app bị kill.

| TODO | Vị trí | Nhiệm vụ |
|---|---|---|
| TODO 01 | `MainActivity.kt` | Override `onSaveInstanceState()` và `onRestoreInstanceState()` |
| TODO 02 | `MainActivity.kt` | Lưu `revenue`, `dessertsSold`, `timerSeconds` vào Bundle |
| TODO 03 | `MainActivity.kt` | Đọc lại dữ liệu từ Bundle trong `onCreate()` |

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putInt(KEY_REVENUE, revenue)
    outState.putInt(KEY_DESSERT_SOLD, dessertsSold)
    super.onSaveInstanceState(outState)
}
```

> **Bài học:** Dữ liệu UI sẽ mất khi xoay màn hình nếu không lưu vào `savedInstanceState`.

---

## 🚀 Hướng dẫn sử dụng

### 1. Clone repo

```bash
git clone https://github.com/chamkartechcambodia-sudo/dessert-pusher-kotlin.git
cd dessert-pusher-kotlin
```

### 2. Mở bằng Android Studio

```
File → Open → chọn thư mục dessert-pusher-kotlin
```

Đợi Gradle sync hoàn tất.

### 3. Chuyển sang branch bài tập

```bash
# Ví dụ: làm bài tập Step 1
git checkout Step.01-Exercise-Logging-a-callback
```

### 4. Tìm và hoàn thành TODOs

Trong Android Studio, mở **TODO panel**:
> `View → Tool Windows → TODO`

Các TODO được đánh số thứ tự — làm lần lượt từ trên xuống.

### 5. So sánh với đáp án

```bash
# Xem đáp án Step 1
git checkout Step.01-Solution-Logging-a-callback

# Hoặc so sánh diff
git diff Step.01-Exercise-Logging-a-callback Step.01-Solution-Logging-a-callback
```

### 6. Quan sát Lifecycle trong Logcat

Sau khi chạy app, mở **Logcat** trong Android Studio và filter theo tag:

```
Tag: MainActivity
```

Thử các hành động sau và quan sát log:
| Hành động | Callbacks được gọi |
|---|---|
| Mở app | `onCreate` → `onStart` → `onResume` |
| Nhấn Home | `onPause` → `onStop` |
| Quay lại app | `onRestart` → `onStart` → `onResume` |
| Xoay màn hình | `onPause` → `onStop` → `onDestroy` → `onCreate` → ... |
| Thoát app | `onPause` → `onStop` → `onDestroy` |

---

## 📊 Sơ đồ Activity Lifecycle

```
         ┌─────────────┐
         │   onCreate  │ ← App khởi động / xoay màn hình
         └──────┬──────┘
                ↓
         ┌─────────────┐
         │   onStart   │ ← Activity hiển thị
         └──────┬──────┘
                ↓
         ┌─────────────┐
    ┌──→ │   onResume  │ ← Người dùng tương tác được
    │    └──────┬──────┘
    │           ↓
    │    ┌─────────────┐
    │    │   onPause   │ ← Mất focus (dialog, chuyển app)
    │    └──────┬──────┘
    │           ↓
    │    ┌─────────────┐
    │    │   onStop    │ ← Activity không còn hiển thị
    │    └──────┬──────┘
    │           ↓
    │    ┌─────────────┐
    └────│  onRestart  │ ← Quay lại app
         └──────┬──────┘
                        ↓ (nếu bị destroy)
                 ┌─────────────┐
                 │  onDestroy  │ ← Giải phóng tài nguyên
                 └─────────────┘
```

---

## 📁 Cấu trúc Source Code

```
app/src/main/java/com/example/android/dessertpusher/
├── MainActivity.kt        ← Màn hình chính, xử lý game logic + lifecycle
├── DessertTimer.kt        ← Timer tích hợp LifecycleObserver
└── PusherApplication.kt  ← Khởi tạo Timber logging
```

---

## 💡 Lưu ý cho sinh viên

1. **Đọc kỹ comment TODO** trước khi code — chúng giải thích rõ phải làm gì
2. **Chạy app sau mỗi TODO** để thấy kết quả ngay trên Logcat
3. **Không xem Solution trước** khi tự làm xong Exercise
4. Nếu bị stuck, dùng lệnh `git diff` để so sánh với Solution
5. **Mỗi branch là độc lập** — không cần lo việc branch này ảnh hưởng branch kia

---

## 🐛 Báo lỗi

Nếu phát hiện lỗi trong project, vui lòng tạo [GitHub Issue](https://github.com/chamkartechcambodia-sudo/dessert-pusher-kotlin/issues).
