# Product specification

Draft 0.1 — 2026-11-26

## Purpose

Recover the user's generated ChatGPT images onto a CachyOS Linux host, verify that the files are usable and complete for the selected scope, and then remove the corresponding content from ChatGPT. The estimated starting collection is 5,000–6,000 images across several accounts; the actual inventory determines completion.

## Confirmed user decisions

| Topic | Requirement |
| --- | --- |
| Deliverable | Original image files |
| Prompts and conversations | No backup required |
| Chat deletion | Acceptable when needed to remove archived images |
| Accounts | Several user-owned ChatGPT accounts |
| Platform | CachyOS Linux |
| Existing export attempt | No email after more than seven days; EU account |
| Current work | Specification and approach selection only |

## Scope

Include accessible generated images, including edits and historical generations that remain available in the account. The feasibility investigation must determine how older image-generation systems, alternate conversation branches, and archived or project chats affect coverage.

User-uploaded reference images are excluded by default. An item whose origin cannot be determined is retained as an unresolved candidate; it must not be silently discarded or treated as safely archived.

Do not generate new images, delete accounts, or modify unrelated account settings. Mobile-only controls may inform the investigation, but the required workflow must run from the Linux host.

## User workflow

1. Choose an account, workspace if applicable, and a destination directory.
2. Sign in through the browser's normal controls, if the selected method needs a session.
3. Discover available images and show the scope and any known coverage gaps.
4. Download originals, retaining progress across interruptions.
5. Verify local files and show a report of completed, failed, ambiguous, and missing items.
6. Prepare a concrete cleanup plan listing the chats and separately saved files eligible for removal.
7. Start that cleanup plan explicitly after reviewing it. Pause or stop at any time.
8. Show archive results and cleanup results separately.

Archive acquisition must finish for the selected run before its cleanup phase starts. A later acquisition run may retry unresolved items without repeating completed work.

## Acquisition requirements

**R1 — Account isolation.** Bind each session and inventory to an explicit account and workspace. Verify the visible account identity before discovery and cleanup. Never carry a cleanup plan into another account or silently switch identities. An export or manually downloaded batch needs an explicit account assignment.

**R2 — Coverage.** Enumerate source items with stable identifiers where available. Support long, lazy-loaded or virtualized lists without assuming that the first screen or visible sidebar is the complete collection. Discovering the same item through multiple surfaces must not increase the source-item count. Report the source and strength of completeness evidence. Exhausting a scroll list alone does not prove that historical images elsewhere are absent.

**R3 — Originals.** Preserve the bytes delivered by the source's original download mechanism. Do not substitute screenshots, canvas captures, or gallery thumbnails. Do not resize, recompress, strip embedded metadata, or convert formats. Establish how each supported source identifies the original variant; unknown variants remain unresolved. Matching dimensions or successfully decoding a file does not establish that it is the original.

**R4 — Durable progress.** Save progress after each completed file. Resume after process termination, browser closure, network loss, expired sessions, or host restart. Use bounded retries for transient acquisition failures and retain reasons for unresolved items.

**R5 — File validation.** Write to a temporary path, wait for completion, validate the actual file type, fully decode supported formats, calculate a cryptographic checksum, and publish the completed file atomically. Preserve unsupported formats but mark them unverified. A nonempty HTTP error page must never pass as an image. Validate animated files without discarding frames.

**R6 — Duplicates and collisions.** Preserve every distinct image. Exact byte duplicates may share storage within an account if each source occurrence remains tracked. Do not collapse edited images, visually similar images, or differing encodings based on appearance. Never overwrite a different file because its supplied filename is the same.

**R7 — Minimal retained information.** The user-facing archive contains images. Keep only the operational information needed for resume, validation, and safe cleanup: account alias, source references, file paths, sizes, checksums, validation evidence, and action status. Do not retain prompts, chat transcripts, or chat titles in logs or filenames. Existing metadata inside original files remains intact.

## Cleanup requirements

**R8 — Exact targets.** Cleanup acts only on explicit targets in a reviewed plan. Deleting a containing conversation is acceptable, including loss of its text and other contents; surface that consequence in the plan. Never select a chat by its title alone. Account-wide “Delete all chats” is outside the initial cleanup scope.

**R9 — Archive before deletion.** A conversation is eligible only when its generated-image inventory is complete for that conversation and every required image has passed original-source and local-integrity checks. Unknown outputs, unresolved branches, missing originals, and failed files block that conversation. Other fully verified conversations may appear in a plan with the exclusions clearly reported.

**R10 — Revalidation.** Before each destructive action, recheck account identity, target identity, local file checksums, and whether the remote target has acquired new image outputs since discovery. A changed target invalidates its plan entry. If a source cannot provide sufficient current coverage evidence, leave that target out of automated cleanup.

**R11 — Separate saved copies.** Treat a conversation and a separately saved Library or Space file as distinct targets. A saved copy needs its own image mapping and verification. Removing an entry from a gallery is not automatically equivalent to deleting the underlying file. Show any known project impact before removing a saved file.

**R12 — Uncertain outcomes.** Record an attempted deletion durably before acting. After a timeout or crash, inspect the target before retrying; do not assume the action failed. Absence from a partial list, a login redirect, or an error page is insufficient evidence of deletion. Report confirmed UI removal separately from uncertain removal and from any provider retention period.

## Browser behavior

If browser automation is selected, use normal visible UI controls and a session isolated for each account. Login, MFA, and access challenges remain user actions. Stop on access denial, rate-limit messages, account warnings, or unexpected destructive dialogs. Provide a clear resume path after the user resolves the interruption.

The project does not require stealth plugins, fingerprint spoofing, proxy rotation, simulated identity changes, CAPTCHA bypass, or private endpoint replay. Pace operations for reliability and react to explicit service signals. No timing pattern can promise avoidance of account restrictions; see [the evidence notes](options-and-evidence.md#browser-automation-and-account-risk).

## Operational expectations

- Process at least 10,000 source items in a synthetic scale check, leaving headroom above the estimated collection.
- Keep memory bounded by the current page, file, and image-decoding workload rather than loading the entire image collection into memory.
- Estimate disk needs from observed files and check available space before writing. Stop cleanly on disk exhaustion.
- Keep operational state and browser credentials private to the local user. Images, exports, profiles, and runtime logs must stay outside the source repository.
- Display counts for source items, distinct files, bytes saved, failures, unresolved coverage, and cleanup outcomes. Deduplication must not obscure missing source items.
- Measure actual transfer speed during a pilot; do not promise a completion time from the image count alone.

## Acceptance criteria

| Scenario | Expected result |
| --- | --- |
| Multiple accounts contain similar filenames or chat titles | Files and cleanup targets remain correctly isolated |
| A chat contains several generations and edits | Every required original is accounted for before that chat is eligible |
| A historical image is absent from the main gallery | The coverage report identifies the gap or discovers it through another supported surface |
| An interrupted download leaves a partial file | It is never reported as complete or used to justify deletion |
| The same completed run is repeated | Existing verified files are reused; distinct files are not overwritten |
| A file is corrupted after acquisition | Reverification fails and dependent cleanup is blocked |
| An image variant cannot be shown to be original | It remains unresolved and blocks dependent cleanup |
| A chat gains a new image after planning | Its cleanup entry is invalidated |
| A delete succeeds but its confirmation is lost | Reconciliation avoids a blind second destructive action |
| A separately saved copy survives chat deletion | It remains a distinct outstanding cleanup target |
| The service challenges or blocks the browser | Automation pauses without bypassing the challenge |

Successful archiving and successful cleanup are independent outcomes. The product must be useful for downloading even when a particular cleanup action is unavailable.
