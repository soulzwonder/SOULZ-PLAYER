# SOULZ PLAYER Updater Specification

## Goal
A validated SOULZ PLAYER Mac build can update the installed PLAYER without manually replacing files.

## Safety gates
The app must only install when all of these are true:
1. The official HTTPS manifest loads from this repository.
2. release_available is true.
3. Remote build is newer than the running build.
4. download_url uses HTTPS.
5. The downloaded package SHA-256 exactly matches sha256.
6. The package contains the expected SOULZ PLAYER .app bundle.
7. The current app can be backed up before replacement.

## Rollback
Before replacement, preserve the previous app bundle as a rollback copy. If replacement or relaunch fails, restore the previous app.

## Publication rule
Do not set release_available=true until the Mac build and physical PLAYER checks pass. Development ZIP existence alone is not sufficient.

## Manifest
latest.json is the only stable update pointer. Release assets may change by version, but PLAYER always checks latest.json.

## Current status
The official manifest endpoint is connected. No installable release is published yet, so release_available remains false.


## FIX28 staging milestone
The PLAYER candidate now has a staged-download path:
- Download only from the HTTPS URL published in latest.json.
- Stream and verify SHA-256 before accepting the package.
- Reject mismatches without installation.
- Store verified packages under the user's Application Support/SOULZ PLAYER/Updates directory.
- Automatic replacement remains disabled until the separate rollback-safe installer helper passes physical Mac testing.


## FIX32 development auto-update milestone
Development builds use `dev-latest.json` as a separate channel from stable.

When `release_available=true`, `package_type=app_zip`, `auto_install=true`, and the SHA-256 matches:
1. PLAYER downloads the app ZIP while running.
2. PLAYER verifies SHA-256 before any installation.
3. PLAYER launches the bundled updater helper.
4. PLAYER closes itself automatically.
5. The helper validates the new .app and matching bundle identifier.
6. The current app is backed up.
7. The bundle is swapped and relaunched.
8. Relaunch is checked; failure triggers rollback.
9. The newest three rollback app backups are retained.

This path is implemented in source but is not yet physically validated on the user's Mac. Do not publish a live dev app asset until that test is complete.
