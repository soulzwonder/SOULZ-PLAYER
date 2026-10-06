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
