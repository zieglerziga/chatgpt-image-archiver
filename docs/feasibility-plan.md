# Feasibility plan

Draft investigation plan — 2026-11-26. These are proposed future checks, not work already performed or authorization to operate accounts during specification drafting.

## Decision to reach

Choose the least complex approach that can recover the accessible original images reliably on CachyOS, then support correctly scoped cleanup. Establish the limits before building for thousands of files.

## Stage 1 — Account and interface inventory

For each account, record a local alias, plan or workspace type, approximate date range, and available Images, Library, or Space surfaces. Identify whether older generations, project chats, archived chats, or alternate branches need another discovery path. Record the installed browser and how the user normally switches accounts.

Inspect normal web controls for individual download, multi-select download, individual chat deletion, and saved-file deletion. Do not infer a bulk control's behavior from its label alone. No deletion is needed at this stage.

**Output:** a capability table per account, with unknown coverage called out explicitly. Export recovery is a parallel option and must not delay the browser assessment.

### Priority investigation — Repeated native batches

The user reports that selecting 500 images in Chrome triggered individual downloads into a preconfigured folder. The proposed workflow targets about 100 images per batch, verifies the batch, removes the corresponding remote targets, refreshes, and repeats. This may avoid the gallery's repeatable scrolling boundary; its ability to expose older items after deletion remains unproven.

1. Identify the exact interface used for the 500-item selection and what selection-based deletion, if any, actually removes.
2. Establish how selected source identities map to individual downloads despite different start/completion order, duplicate names, and unrelated browser downloads.
3. Validate native file quality and establish completion and local-verification signals. Do not treat the number of started downloads as proof that the batch is archived.
4. During a later authorized controlled cleanup test, verify whether previously unseen items become reachable after removal and refresh. Perform deletion tests only after their required files are verified. Do not remove valuable content merely to diagnose scrolling.
5. If chat deletion is required, establish all generated outputs in each selected chat, including edits and alternate branches. Treat about 100 as a target that can expand to include whole chats.
6. Define checkpoints between selection, downloading, verification, cleanup, and refresh. A restarted run must reconcile its current batch before treating items as new.
7. Evaluate conversation-based discovery if the loop stops advancing. Older images are known to open from their chats, but reliable traversal of conversation history still needs evidence.

**Decision:** prefer the native batch loop if source mapping, verification, cleanup semantics, and replenishment are demonstrated. Otherwise assess conversation-based discovery. Selecting another browser framework is not proof that either discovery problem is solved.

## Stage 2 — Original download and coverage sample

Proposed sample: approximately 20–30 generated images, adjusted to cover the actual collection's source types. Include recent and old images, images from different generation systems if present, several outputs in one chat, edits, duplicate filenames, and any available alternate branches.

Use the native download of each sampled image as the reference. Check candidate batch or export files against those bytes where comparable. A matching checksum is strong evidence for that exact sample; a resolution match alone is insufficient. Record unexplained mismatches, missing variants, and formats the intended verifier cannot decode.

Inspect how source identifiers connect gallery items, file downloads, saved copies, and conversations. A successful sample qualifies only the tested cases; it does not prove full-account completeness. Any rule generalized to other items must retain its evidence and scope.

**Output:** a mapping of supported source types to original-download mechanisms, a list of uncovered cases, and an observed time and file-size range.

## Stage 3 — Choose the acquisition approach

| Finding | Proposed decision |
| --- | --- |
| Native batches cover originals and preserve enough source mapping | Build only local verification, inventory, and cleanup planning around those downloads |
| Verified batch cleanup and refresh expose unseen older items | Use the repeated batch loop without requiring full-account discovery first |
| Conversation history reaches older content that the gallery omits | Prefer conversation-based discovery if history traversal and per-chat image coverage can be established |
| Native batches leave gaps, but an extension can drive the required UI and track native downloads reliably | Prefer the extension if its permissions, browser lifecycle, and durable state are manageable |
| An external controller handles the required UI and works on CachyOS | Prefer it if local orchestration and recovery are simpler than the extension |
| A recovered export contains the necessary originals and mappings | Consider local import as the simpler acquisition path |
| Images can be downloaded but completeness cannot be established for a chat | Support archival; keep that chat out of automatic cleanup |
| Neither route covers historical cases | Document the shortfall and decide whether limited coverage is useful before implementing |

If Playwright is shortlisted, first validate browser launch, normal manual sign-in, one original download, and restart on the actual CachyOS host. Prefer no extra infrastructure unless the host check shows a need.

**Output:** a short decision record selecting the integration, language/runtime, user interface, and any necessary host setup. Include the reasons and remaining limits.

## Stage 4 — Define the implementation slices

Only after approach selection, prepare implementation work in this order:

1. Local file verification and durable inventory using synthetic fixtures.
2. One-account discovery and original acquisition, without cleanup.
3. Interruption recovery, exact duplicates, and reconciliation of missing items.
4. Several isolated accounts and per-account reporting.
5. Cleanup plan generation, initially without executing it.
6. Targeted cleanup and uncertain-outcome recovery, with archive checks enforced.
7. Scale and usability validation against the acceptance criteria.

The image archive is the deliverable. A small operational catalog may track source items, files, runs, and cleanup actions; its storage format is an implementation choice. Prompts and transcripts are unnecessary. Do not commit to a public file format or command interface before selecting the workflow.

## Future validation

Use synthetic fixtures for failures that should not be tested on valuable account data: truncated files, HTML error downloads, duplicate names, interrupted writes, changed hashes, disk exhaustion, identity mismatches, and crashes around deletion acknowledgments. If ZIP input is selected, include corrupt archives and unsafe member paths.

Use a controlled test chat created for that purpose before any real deletion test. Verify that all its generated outputs are local, that cleanup removes the intended target, and that a deliberately unresolved output blocks deletion. Saved-file cleanup requires a separate test because its effects differ from chat deletion.

Run a synthetic inventory of at least 10,000 items to assess progress persistence, bounded resource use, and correct accounting. An actual collection pilot supplies realistic navigation and throughput evidence. Passing synthetic checks alone does not establish real account coverage.

## Facts not yet established

- Exact image count and distribution across accounts.
- Installed browser and preferred account-switching workflow.
- Coverage of other accounts and of Library or Space beyond the observed gallery failure.
- Whether conversation history can be enumerated reliably beyond the known gallery stopping point. The gallery is already known to omit some accessible older images.
- Maximum native batch size (500 has been observed), output format, and original-image coverage.
- Which interface supplied the observed 500-item download and whether it supports selection-based deletion.
- Whether cleanup and refresh expose unseen items beyond the gallery boundary.
- Whether stable source identifiers are exposed by the normal UI.
- Behavior of hidden branches and separately saved copies.
- Which integration works best on the user's CachyOS host.

These unknowns affect the approach, so they remain explicit instead of being replaced by assumptions in the requirements.
