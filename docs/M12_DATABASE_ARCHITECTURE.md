# M12 DATABASE ARCHITECTURE & PERSISTENCE SPECIFICATION

## 1. Overview
The M12 storage architecture decouples detection persistence from real-time audio inference using an abstract repository pattern (`DetectionRepository`).

```text
Audio Input Stream / REST Request
             │
             ▼
   Baseline AASIST Detector
             │
             ▼
   Platt Scaling Calibrator
             │
             ▼
 Multi-Class Decision & Prevention Engine
             │
             ├─────────────────────────────────────────┐
             ▼                                         ▼
   Live Response Frame                        DetectionRecord
 (WebSocket / REST Response)                           │
                                                       ▼
                                             ConsistencyAnalyzer
                                                       │
                                                       ▼
                                            DetectionRepository (ABC)
                                                       │
                                        ┌──────────────┴──────────────┐
                                        ▼                             ▼
                           SQLiteDetectionRepository     SupabaseDetectionRepository
                           (data/runtime/detections.db)   (Online PostgreSQL / Fallback)
```

---

## 2. Key Architecture Principles

1. **Repository Abstraction**: All application routes interact solely through `DetectionRepository`. Persistence providers (SQLite or Supabase) can be swapped without modifying ML or streaming code.
2. **Database Failure Independence**: Persistence errors or database locks log warnings without crashing or slowing down real-time audio detection (`DATABASE FAILURE != DETECTOR FAILURE`).
3. **Non-Blocking Persistence**: Asynchronous bounded queue worker handles database insertion in the background.
4. **Privacy-by-Default**: Zero raw PCM microphone audio bytes persisted to disk or database. Only derived evidence (exact SHA-256 file hash, quality metrics, calibrated probabilities, risk state) is stored.
