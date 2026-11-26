# Product specification

Draft 0.2 — 2026-11-26

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
| Gallery failure | Loading stops around the same point after a refresh; the page stays responsive |
| Older images | Images beyond the gallery stopping point still open from their containing chats |
| Native download capability | User downloaded a selection of 500 images in Chrome, producing individual download events |
| Current work | Specification and approach selection only |

## Scope

Include accessible generated images, including edits and historical generations that remain available in the account. The feasibility investigation must determine how older image-generation systems, alternate conversation branches, and archived or project chats affect coverage.

User-uploaded reference images are excluded by default. An item whose origin cannot be determined is retained as an unresolved candidate; it must not be silently discarded or treated as safely archived.

Do not generate new images, delete accounts, or modify unrelated account settings. Mobile-only controls may inform the investigation, but the required workflow must run from the Linux host.

## User workflow

1. Choose an account, workspace if applicable, a destination directory, and a batch target. The proposed default is about 100 source images.
2. Sign in through the browser's normal controls, if the selected method needs a session.
3. Select a reachable batch and record its source identities before triggering any downloads. Show its scope and coverage gaps.
4. Use native batch downloading where supported, track each individual download, and retain progress across interruptions.
5. Verify the batch's files and show completed, failed, ambiguous, and missing items.
6. Prepare a concrete cleanup plan for the fully verified image/file targets or containing chats. The selected interface and actual deletion semantics determine which kind is available.
7. Start cleanup explicitly after reviewing its scope. Pause or stop at any time. Whether later batches require another confirmation or run under a single account-scoped authorization remains an interface decision.
8. Reconcile cleanup outcomes, refresh the source, and check whether previously unseen items are now reachable.
9. Repeat with another batch while the workflow makes progress. Show archive and cleanup results separately.

Acquisition and verification must finish for a batch before that batch's cleanup starts. The whole account need not be enumerated first. Keep each completed batch durable so a later batch or restarted run can retry unresolved items without repeating completed work.

The batch size counts distinct source-image occurrences, not just unique file contents or download events. About 100 is a proposed working target, not an image-service limit. If chat deletion is needed, include every generated image in each selected chat, even when the batch grows beyond the target. A large chat may be downloaded in smaller chunks, but it cannot be deleted until its entire required inventory is verified.

## Acquisition requirements

**R1 — Account isolation.** Bind each session and inventory to an explicit account and workspace. Verify the visible account identity before discovery and cleanup. Never carry a cleanup plan into another account or silently switch identities. An export or manually downloaded batch needs an explicit account assignment.

**R2 — Coverage.** Enumerate source items with stable identifiers where available. Support long, lazy-loaded or virtualized lists without assuming that the first screen or visible sidebar is the complete collection. Discovering the same item through multiple surfaces must not increase the source-item count. Report the source and strength of completeness evidence. Exhausting a scroll list alone does not prove that historical images elsewhere are absent.

The reported gallery stall is a known failure case. Track forward progress using newly discovered source items rather than scroll position, page height, or the number of currently rendered tiles. Persist discovered identifiers before any reload; use bounded recovery attempts and reconcile repeated items afterward. Distinguish discovery that is progressing, stalled, and complete for an explicitly supported scope. A responsive page with no new results is not evidence of completion.

The leading proposal uses bounded batches of currently reachable items, then refreshes after verified cleanup. Test whether this exposes older unseen items; do not assume deletion repairs the gallery. After a refresh, identify items again rather than reusing scroll offsets or selection positions from the previous batch. Conversation-based discovery is a fallback because older images remain accessible there, but a chat sidebar or keyword search must not be assumed to enumerate all conversations. A stalled gallery does not by itself prevent cleanup of a separately and fully verified target under R9, but it cannot justify account-wide coverage claims.

**R3 — Originals.** Preserve the bytes delivered by the source's original download mechanism. Do not substitute screenshots, canvas captures, or gallery thumbnails. Do not resize, recompress, strip embedded metadata, or convert formats. Establish how each supported source identifies the original variant; unknown variants remain unresolved. Matching dimensions or successfully decoding a file does not establish that it is the original.

**R4 — Durable progress.** Save progress after each completed file. Resume after process termination, browser closure, network loss, expired sessions, or host restart. Use bounded retries for transient acquisition failures and retain reasons for unresolved items.

**R5 — File validation.** Write to a temporary path, wait for completion, validate the actual file type, fully decode supported formats, calculate a cryptographic checksum, and publish the completed file atomically. Preserve unsupported formats but mark them unverified. A nonempty HTTP error page must never pass as an image. Validate animated files without discarding frames.

**R6 — Duplicates and collisions.** Preserve every distinct image. Exact byte duplicates may share storage within an account if each source occurrence remains tracked. Do not collapse edited images, visually similar images, or differing encodings based on appearance. Never overwrite a different file because its supplied filename is the same.

**R7 — Minimal retained information.** The user-facing archive contains images. Keep only the operational information needed for resume, validation, and safe cleanup: account alias, source references, file paths, sizes, checksums, validation evidence, and action status. Do not retain prompts, chat transcripts, or chat titles in logs or filenames. Existing metadata inside original files remains intact.

**R13 — Native batch accounting.** Track the expected source items and their corresponding downloads individually. A download starting is different from completing, and completion is different from passing local verification. Never infer the source-to-file mapping from event order, equal counts, or filenames alone. Reconcile interrupted downloads and browser restarts, ignore unrelated downloads, and block cleanup where the mapping remains ambiguous. An extension download monitor is one candidate; the integration and local file access mechanism remain undecided.

## Cleanup requirements

**R8 — Exact targets.** Cleanup acts only on explicit targets in a reviewed plan. Establish what any native selection-based deletion actually removes before using it. Deleting a containing conversation is acceptable, including loss of its text and other contents; surface that consequence in the plan. Never select a chat by its title alone. Account-wide “Delete all chats” is outside the initial cleanup scope.

**R9 — Archive before deletion.** Each target requires a verified source-to-file mapping and original-source and local-integrity checks. A conversation is eligible only when its generated-image inventory is complete for that conversation and every required image has passed those checks. Unknown outputs, unresolved branches, missing originals, and failed files block that conversation. Other fully verified targets may appear in a plan with the exclusions clearly reported.

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
| The gallery stops at the same point after a refresh while older images still open in chats | Discovery reports the stall, retains its inventory, and assesses conversation-based discovery without claiming full-account completion |
| A virtualized list replaces old tiles without increasing page height | Newly discovered identifiers still count as progress; repeated items do not |
| A native batch starts 100 downloads, but one is interrupted | It remains incomplete; equal event and selection counts do not permit cleanup |
| Download events complete in a different order from the selection | Source-to-file mappings remain correct |
| A selected chat contains more than the batch target | All its required images are verified before that chat can be deleted |
| Cleanup finishes but the refreshed gallery exposes no unseen items | Report lack of progress; do not claim that the account is empty |
| An interrupted download leaves a partial file | It is never reported as complete or used to justify deletion |
| The same completed run is repeated | Existing verified files are reused; distinct files are not overwritten |
| A file is corrupted after acquisition | Reverification fails and dependent cleanup is blocked |
| An image variant cannot be shown to be original | It remains unresolved and blocks dependent cleanup |
| A chat gains a new image after planning | Its cleanup entry is invalidated |
| A delete succeeds but its confirmation is lost | Reconciliation avoids a blind second destructive action |
| A separately saved copy survives chat deletion | It remains a distinct outstanding cleanup target |
| The service challenges or blocks the browser | Automation pauses without bypassing the challenge |

Successful archiving and successful cleanup are independent outcomes. The product must be useful for downloading even when a particular cleanup action is unavailable.
