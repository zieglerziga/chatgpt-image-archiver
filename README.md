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

## Working direction

The leading proposal is a repeated batch workflow targeting about 100 images: select reachable items, use native downloading, verify the files, remove the verified remote targets, refresh, and repeat. The successful 500-image selection demonstrates that native batch downloading is available on at least one account; 500 is an observed size, not an established limit.

First verify which interface supplied that selection, what its deletion controls actually remove, and whether cleanup exposes previously unreachable items. Full-account discovery need not precede every batch, but each deletion target must be fully accounted for. If deletion requires removing a chat, the batch must include all its generated images even when this exceeds the target size. Conversation-based discovery remains a fallback if the batch loop cannot progress. Export and Privacy Portal recovery remain optional alternatives.

Select the browser integration after checking the actual account UI. A custom extension and an external browser controller are both candidates. Human-like browser actions do not provide a guarantee against account restrictions; the relevant terms and technical limitations are recorded in the evidence notes.

## Documents

- [Product specification](docs/product-spec.md): scope, workflow, recovery, cleanup rules, and acceptance criteria.
- [Options and evidence](docs/options-and-evidence.md): current official documentation, alternatives, limitations, and unresolved facts.
- [Feasibility plan](docs/feasibility-plan.md): the proposed investigation before implementation and the decisions it must settle.

The next decision is which acquisition method can enumerate and download the user's historical images reliably. Implementation begins only after that choice is made.
