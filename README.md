# ChatGPT Image Archiver

Draft specification for recovering approximately 5,000–6,000 generated images across several personal ChatGPT accounts, then removing the archived images from ChatGPT. The target host is CachyOS Linux.

**Status:** requirements and approach selection only. No application, browser integration, or account operation has been implemented.

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

## Working direction

The leading proposal is a repeated batch workflow targeting about 100 images: record the selected source items durably, use native downloading, verify and match the files, reselect the verified source items, remove those targets, refresh, and repeat. Native batch downloading has been demonstrated in Chrome and Firefox. Batch size is configurable; 500 and roughly 700 are observed runs, not established limits.

The Firefox screenshot establishes Space's image-filtered list as a working surface with native Download and Delete controls. The first feasibility priority is identifying stable source references and matching each selected item to its saved file after the UI loses its selection. Also establish the actual effects of Delete and whether cleanup exposes previously unreachable items. Full-account discovery need not precede every batch, but each deletion target must be fully accounted for. If deletion requires removing a chat, include all its generated images even when this exceeds the target size. Conversation-based discovery and export recovery remain fallbacks.

Select the browser integration after checking the actual account UI. A custom extension and an external browser controller are both candidates. Human-like browser actions do not provide a guarantee against account restrictions; the relevant terms and technical limitations are recorded in the evidence notes.

## Documents

- [Product specification](docs/product-spec.md): scope, workflow, recovery, cleanup rules, and acceptance criteria.
- [Options and evidence](docs/options-and-evidence.md): current official documentation, alternatives, limitations, and unresolved facts.
- [Feasibility plan](docs/feasibility-plan.md): the proposed investigation before implementation and the decisions it must settle.

The next decision is which acquisition method can enumerate and download the user's historical images reliably. Implementation begins only after that choice is made.
