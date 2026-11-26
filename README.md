# ChatGPT Image Archiver

Draft specification for recovering approximately 5,000–6,000 generated images across several personal ChatGPT accounts, then removing the archived images from ChatGPT. The target host is CachyOS Linux.

**Status:** specifications and limited browser feasibility testing. No downloader application has been implemented.

## Confirmed requirements

- Save original image files. Prompt exports and conversation backups are not required.
- Deleting the containing chats is acceptable. Preserving chats is not a requirement.
- Support several accounts without mixing their sessions or cleanup targets.
- Recover from interruptions without losing progress or repeatedly downloading completed images.
- Verify the local archive before remote cleanup.
- Do not depend on receiving an account export: an EU account export was requested more than seven days ago and no email arrived.
- The image gallery stops loading around the same point after a refresh while the page remains responsive. Older images still open from their containing chats.
- The user has downloaded a selection of 500 images through Chrome, with a preconfigured destination folder and 500 individual download events.
- A Firefox run has finished according to the user. Its Space image selection was lost while the images remained listed. A read-only local inventory found 703 image files and no browser partial files; source-to-file correspondence is not yet established.
- First target: the ChatGPT app's connected in-app browser. Chrome and other mainstream browsers are longer-term targets.

## Working direction

The leading proposal is a repeated batch workflow targeting about 100 images: record the selected source items durably, use native downloading, verify and match the files, reselect the verified source items, remove those targets, refresh, and repeat. Native batch downloading has been demonstrated in Chrome and Firefox. Batch size is configurable; 500 and roughly 700 are observed runs, not established limits.

Live inspection found 50 distinct `data-library-selection-id` values that matched their Space file links and survived refresh. A two-item selection was restored exactly by those IDs after another refresh. This supports selection recovery for the tested cases; matching source items to verified local downloads is the next feasibility priority. Also establish Delete's actual effects and whether cleanup exposes older items.

The first version is an agent-assisted workflow using the connected in-app browser. Keep archive state, validation, and cleanup rules separate from a browser adapter so Chrome and Firefox can follow. Access to the current browser tools is available in this app session; standalone programmatic access and unattended execution still need validation. Full-account discovery need not precede each batch, but every deletion target must be fully accounted for.

A custom extension and an external controller are candidates for later browser adapters. Human-like browser actions do not provide a guarantee against account restrictions; the relevant terms and technical limitations are recorded in the evidence notes.

## Documents

- [Product specification](docs/product-spec.md): scope, workflow, recovery, cleanup rules, and acceptance criteria.
- [Options and evidence](docs/options-and-evidence.md): current official documentation, alternatives, limitations, and unresolved facts.
- [Feasibility plan](docs/feasibility-plan.md): the proposed investigation before implementation and the decisions it must settle.

The next feasibility milestone is correlating saved file identities with completed native downloads and verified local files. Application implementation remains future work.
