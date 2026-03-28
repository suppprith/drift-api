# Image Recognition Pipeline (Photo Proof Verification)

AI-powered system that verifies lore quest completions. When a user submits a photo claiming they completed a quest (e.g., "Spin all prayer wheels at Swayambhunath"), the system must verify the photo is legitimate.

```
User submits photo + quest_id + lat/lng
         │
         ▼
┌─ STEP 1: UPLOAD & METADATA ───────────────────────────┐
│  • Upload photo to Cloudflare R2                        │
│  • Extract EXIF: GPS coords, timestamp, camera model   │
│  • Validate: photo taken within last 24 hours           │
│  • Validate: GPS coords within quest geofence radius    │
│  • Strip EXIF for public display (privacy)              │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ STEP 2: CONTENT MODERATION ──────────────────────────┐
│  • Google Cloud Vision SafeSearch API                   │
│    → adult, violence, racy, medical, spoof scores       │
│  • If any score = VERY_LIKELY → auto-reject             │
│  • If any score = LIKELY → flag for manual review       │
│  • If all UNLIKELY → proceed                            │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ STEP 3: LANDMARK & SCENE VERIFICATION ───────────────┐
│  • Gemini 2.5 Flash Vision API call:                    │
│    Input: photo + verification_prompt from quest        │
│    Prompt: "Analyze this photo. The user claims to be   │
│     at [quest location]. Verify:                        │
│     1. Does this photo contain [required_photo_subject]?│
│     2. Is this a real photograph (not a screen photo)?  │
│     3. Does the scene match [destination/location]?      │
│     4. Confidence score 0-1 for each check.             │
│     Return JSON: {landmark_match, is_real_photo,        │
│      scene_match, overall_confidence, reasoning}"       │
│                                                          │
│  • Decision matrix:                                      │
│    overall_confidence >= 0.85 → auto-verify              │
│    overall_confidence >= 0.50 → manual review queue      │
│    overall_confidence < 0.50  → auto-reject              │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ STEP 4: ANTI-FRAUD CHECKS ──────────────────────────┐
│  • Reverse image search (Gemini Vision + web context)   │
│    → Is this a stock photo or from Google Images?       │
│  • Photo hash comparison (perceptual hash via phash)    │
│    → Has this exact/similar photo been submitted before?│
│  • Rate limiting: max 10 quest submissions per day       │
│  • Device fingerprint checks (optional V2)               │
└────────────────────────────────────────────────────────┘
         │
         ▼
┌─ STEP 5: REWARD GRANTING ─────────────────────────────┐
│  • If verified: grant XP → check level-up → notify     │
│  • Update Redis sorted set leaderboard                   │
│  • Check achievement triggers                            │
│  • If bonus reward → create reward entry                 │
│  • Add to social feed                                    │
└────────────────────────────────────────────────────────┘
```

## Do We Need Custom ML?

No. Gemini 2.5 Flash Vision + Google Cloud Vision covers all needs:

| Task                                     | Service                        | Custom ML Needed?                      |
| ---------------------------------------- | ------------------------------ | -------------------------------------- |
| Landmark identification                  | Gemini Vision                  | No — zero-shot, any landmark worldwide |
| Scene classification                     | Gemini Vision                  | No — prompted classification           |
| NSFW/content moderation                  | Google Cloud Vision SafeSearch | No — purpose-built API                 |
| Photo authenticity (real vs screenshot)  | Gemini Vision                  | No — prompted analysis                 |
| Photo-location matching                  | Gemini Vision + EXIF GPS       | No — combine geo + visual              |
| Perceptual hashing (duplicate detection) | Simple algorithm (dHash)       | No — ~20 lines of code                 |
| OCR (receipts, tickets)                  | Gemini Vision                  | No — built-in OCR                      |

## When to Consider Custom ML (V3+)

- Specific food dish recognition ("is this actually buff momo?") — would need a fine-tuned image classifier
- User behavior fraud detection (unusual completion patterns) — anomaly detection model
- Photo quality scoring (is this a "good" travel photo for the feed?) — aesthetic scoring model
