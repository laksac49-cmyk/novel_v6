# Novel v6 — Requirements status (latest GitHub main)

Checked against `laksac49-cmyk/novel_v6` on 2026-08-17.

## Login /api/me

| Item | Status |
|------|--------|
| Guest login returns JWT | Done |
| `/api/me` with correct Bearer JWT | Works |
| Wrong token strings fail | Expected |

**How to test**
```bash
curl -X POST http://127.0.0.1:8000/api/auth/guest \
  -H "Content-Type: application/json" \
  -d "{\"device_id\":\"test-device-12345678\"}"

# Copy the "token" field, then:
curl http://127.0.0.1:8000/api/me \
  -H "Authorization: Bearer PASTE_TOKEN_HERE"
```
Do **not** use `JWT_SECRET` or Google Client IDs as the Bearer token.

---

## Backend requirements

| # | Requirement | Status | Notes |
|---|-------------|--------|-------|
| 1 | Comment 500 error | Done | Null-safe serializers, table ensure, try/except |
| 5 | Followers / Following | Done | `author_follows`, follow/unfollow, counts on `/api/me` |
| 6 | Auto-publish stories | Done | Default `status_text=Published` on create |
| 6 | Auto-unpublish after 3 reports | Done | `story_reports.py` sets `Unpublished` at count ≥ 3 |
| 6 | Admin reports + republish | Done | `GET /api/admin/reports`, `POST .../republish` |
| 7 | Genre dropdown + create new | Done | Create story UI + API |
| 7 | Admin hashtags | Done | `admin_tags` routes |
| — | DB scripts auto-run on start | Done | `startup_tasks.py` + migrations + seed |
| — | Seed with 3 chapters / novel | Done | Idempotent; unique bodies if you applied prior ZIP |

---

## Flutter / UI requirements

| # | Requirement | Status | Where |
|---|-------------|--------|-------|
| 2 | Author profile picture in reader | Done | `chapter_reader_screen.dart` — CircleAvatar + resolve from profile |
| 3 | Next Chapter button | Done | Bottom of chapter body |
| 3 | Scroll to top on next | Done | `_goNext()` → `jumpTo(0)` |
| 4 | Light / Dark / System toggle | Done | More screen → Appearance (`ThemeController`) |
| 7 | Book cover preview on create | Done | Live preview + upload on `create_story_screen` |
| 7 | Chapter cover (book cover at start) | Done | Top of every chapter |
| 7 | Mid-chapter ad banner | Done | Between content halves |
| 7 | Ad near Next Chapter | Done | Above Next button |
| 7 | Share chapter | Done | `share_plus` + clipboard fallback |
| 7 | Home New / Author section | Done | Discover “New” tab rails + `_AuthorsStrip` |

---

## What you should do now

1. If not already applied: unzip **novel_v6_login_seed_update.zip** (previous deliverable) into project root and restart backend.
2. Restart backend and confirm:
   ```bash
   curl http://127.0.0.1:8000/api/health
   ```
3. Test guest → `/api/me` with the real JWT.
4. Run the Flutter app and spot-check reader (avatar, next, ads, share) and More → Appearance.
5. Push any local changes you made.

## No large Flutter code ZIP this round

All listed UI features are already present on GitHub main. Shipping another full-screen overwrite would risk regressions without a reported bug.

If something specific still fails on your device (e.g. avatar never shows, Next does nothing, theme does not apply), reply with:
- screen name
- steps to reproduce
- expected vs actual

Then the next ZIP will target only those files.
