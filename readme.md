# 📳 Dynamic Haptics Manager (Unity / XR)

A **modular, enum-driven, runtime-safe Haptics Management System** for Unity XR projects.
Designed for **Meta Quest / XR controllers**, with clean abstraction for extending to other platforms.

---

## ✨ Features

* 🎮 Centralized **HapticManager** (Singleton)
* 📦 ScriptableObject-based **Haptic Library**
* 🧠 Enum-driven haptic events
* 🤲 Single-hand or dual-hand haptics
* ⏱️ Configurable duration, amplitude, frequency
* 🔁 Stop all haptics instantly
* 🔌 Platform-agnostic handler architecture
* 🧪 Safe to call from **any gameplay script**

---

## 📁 Project Structure

```
Haptics/
├── AbstractHapticHandler.cs
├── HapticManager.cs
├── QuestHapticHandler.cs
├── HapticLibrarySO.cs
├── Constants.cs
```

---

## 🧠 Architecture Overview

### 1️⃣ HapticManager (Singleton)

The **only class gameplay code talks to**.

* Persists across scenes (`DontDestroyOnLoad`)
* Looks up haptic data from `HapticLibrarySO`
* Delegates execution to a platform-specific handler

```csharp
HapticManager.Instance.Play(HapticType.GunShoot);
```

---

### 2️⃣ HapticLibrarySO (ScriptableObject)

Defines **all haptic patterns** in one place.

Each haptic entry includes:

* `HapticType` (enum)
* Duration
* Frequency
* Amplitude
* One-hand / both-hands

📌 **Create via:**

```
Right Click → Create → Haptics → Haptic Library
```

---

### 3️⃣ AbstractHapticHandler

Base class that allows **platform-specific implementations**.

```csharp
public abstract class AbstractHapticHandler
{
    public abstract void Init();
    public abstract void Play(HapticData data);
    public abstract void StopAll();
}
```

---

### 4️⃣ QuestHapticHandler (XR Implementation)

Concrete implementation using **Unity XR InputDevices**.

* Automatically detects left & right controllers
* Uses `SendHapticImpulse`
* Supports:

  * Right hand only
  * Both hands
* Gracefully handles missing devices

---

## 🎮 How to Use (Gameplay Scripts)

### ▶️ Play a Haptic Event

```csharp
HapticManager.Instance.Play(HapticType.GunShoot);
```

---

### 🤲 Dual-Hand Haptics

Enable **Both Hands** in the `HapticLibrarySO` entry.

No code changes required.

---

### ⏹️ Stop All Haptics

Useful for:

* Scene transitions
* Pause menus
* Player death

```csharp
HapticManager.Instance.StopAllHaptics();
```

---

## 📋 Available Haptic Events

Defined in `Constants.cs`:

```csharp
public enum HapticType
{
    JetpackStart,
    JetpackSustain,
    GunShoot,
    GunReload,
    WebAttach,
    WebSwing,
    PlayerHitLight,
    PlayerHitHeavy,
    EnemyHit,
    GrabObject,
    ThrowCharge,
    ObjectImpact,
    LowHealth,
    CriticalHealth,
    DeathPulse
}
```

You can safely add more at any time.

---

## 🧪 Example Use Cases

### 🔫 Gunfire

```csharp
HapticManager.Instance.Play(HapticType.GunShoot);
```

### 🚀 Jetpack

```csharp
HapticManager.Instance.Play(HapticType.JetpackStart);
```

### 🕸️ Web Swing

```csharp
HapticManager.Instance.Play(HapticType.WebAttach);
```

### ❤️ Player Damage

```csharp
HapticManager.Instance.Play(HapticType.PlayerHitHeavy);
```

---

## ⚙️ Performance & Safety

✔ No `Update()` loops
✔ No allocations per call
✔ No coroutines required
✔ Safe null checks
✔ Device validation before sending impulses
✔ Scene-safe singleton

---

## 🔌 Extending to Other Platforms

You can easily support:

* PlayStation controllers
* Xbox controllers
* Custom haptic devices

### Example:

```csharp
public class GamepadHapticHandler : AbstractHapticHandler
{
    public override void Init() { }
    public override void Play(HapticData data) { }
    public override void StopAll() { }
}
```

Then swap the handler in `HapticManager.InitHandler()`.

---

## 🧠 Best Practices

* Keep **all tuning** in `HapticLibrarySO`
* Trigger haptics from **gameplay events**, not Update loops
* Use **short pulses** for frequent actions
* Reserve **long pulses** for critical feedback

---

## ✅ Requirements

* Unity 2021+
* XR Plugin Management enabled
* XR Interaction Toolkit (recommended)
* Meta Quest / XR controller with haptic support

---

## 📜 License

Free to use, modify, and extend in personal or commercial projects.

---

## 🙌 Credits

Built as a **production-ready, extensible haptics framework** designed for:

* VR games
* XR prototypes
* Interaction feedback systems

