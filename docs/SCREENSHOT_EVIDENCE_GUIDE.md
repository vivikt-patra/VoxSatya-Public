# Screenshot Evidence Guide (`docs/SCREENSHOT_EVIDENCE_GUIDE.md`)

This guide establishes the naming conventions, capture protocols, and organizational structure for maintaining visual proof-of-work screenshots in the VoxSatya repository.

---

## 📸 Screenshot Repository Location

All verified screenshots must be organized under:
```text
docs/screenshots/
```

---

## 🏷️ Naming Convention

Use descriptive, lowercase-with-underscores filenames indicating what the screenshot proves:

| Filename Pattern | Contents |
|:---|:---|
| `voxsatya_light_theme.png` | Full Android dashboard in White / Light Theme |
| `voxsatya_defense_ui.png` | Call Defense HUD during active monitoring session |
| `voxsatya_call_active.png` | Ongoing call screen with threat verdict overlay |
| `voxsatya_forensic_export.png` | CFCFRMS / 1930 incident JSON export confirmation |
| `voxsatya_amber_alert.png` | Amber Coercion Alert banner triggering during vocal stress detection |
| `backend_swagger.png` | FastAPI Swagger UI at `/docs` showing all API endpoints |
| `pytest_passing.png` | Terminal output showing green test suite completion |
| `gradle_build_success.png` | Gradle assembleDebug BUILD SUCCESSFUL terminal output |

---

## 📱 Android Device Capture Protocol

Capture with ADB (provides exact device pixel resolution):
```powershell
# Connect your Vivo phone via USB, then:
$adb = "voxsatya-android\.tooling\android-sdk\platform-tools\adb.exe"
& $adb shell screencap -p /sdcard/voxsatya_screenshot.png
& $adb pull /sdcard/voxsatya_screenshot.png docs/screenshots/voxsatya_<FEATURE>.png
& $adb shell rm /sdcard/voxsatya_screenshot.png
```

---

## 🖥️ Desktop / API Capture Protocol

Use browser screenshot tools (F12 → Device Toolbar → screenshot) for web cockpit captures, or Windows Snipping Tool (`Win + Shift + S`) for terminal evidence.

---

## 📝 Embedding in README.md

Screenshots must be embedded using relative markdown paths to ensure GitHub renders them directly in the repository view:
```markdown
<p align="center">
  <img src="docs/screenshots/voxsatya_light_theme.png" alt="VoxSatya Light Theme UI" width="300" />
</p>
```
