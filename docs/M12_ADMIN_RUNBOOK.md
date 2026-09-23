# M12 ADMIN & OPERATOR RUNBOOK

## 1. Starting the Application Server
Run the SIH live demonstration launcher script or launch Uvicorn directly:
```powershell
.venv\Scripts\python.exe scripts/start_sih_demo.py
```

---

## 2. Accessing the Admin Audit Cockpit UI
1. Open your web browser to `http://127.0.0.1:8000/static/admin.html`.
2. Click **Set Token** in the top right header.
3. Enter the Admin Secret Password or use the dev token `admin_token_sih2026_golden_secure_master`.
4. Click **Login as Admin**.

---

## 3. Viewing Detection History & Consistency Warnings
- Use the **Class** dropdown to filter by `GENUINE`, `SYNTHETIC`, `MANIPULATED`, or `UNCERTAIN`.
- Use the **Consistency** dropdown to filter by `CONTRADICTORY` or `CROSS_MODEL_VARIATION`.
- Click any table row to open the **Detection Event Detail** modal, showing SHA-256 audio hash, model provenance, and consistency breakdown.

---

## 4. Exporting Metadata
- Click **Export JSON** or **Export CSV** in the controls bar to download `m12_detection_export.json` / `.csv`.

---

## 5. Running Automated Verification Scripts
```powershell
# Run M12 database benchmark harness
.venv\Scripts\python.exe scripts/run_m12_database_benchmark.py

# Run M12 PyTest test suite
.venv\Scripts\pytest.exe tests/test_m12_secure_database.py -v
```
