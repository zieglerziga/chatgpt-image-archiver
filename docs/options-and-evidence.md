# Options and evidence

Reviewed 2026-11-26. Account-specific availability still needs observation. Source statements below are distinguished from project choices and unresolved questions.

## Official account export

OpenAI documents export through eligible account settings and the Privacy Portal. Exports can take up to seven days; settings-export download links expire after 24 hours. The guide describes chat history and other account data and recommends contacting Support if expected files are missing or delivery exceeds seven days. It lists plan and workspace restrictions, with no EU exclusion stated. It does not establish that this user's full historical collection of original image binaries will be present. [Exporting your ChatGPT history and data](https://help.openai.com/en/articles/7260999-exporting-your-chatgpt-history-and-data).

**User evidence:** an EU account's requested export produced no email after more than seven days. This establishes a delivery failure, not the contents of a received archive. The cause is unknown.

**Project choice:** export is optional. A Privacy Portal request or Support inquiry may recover this route, but neither is a prerequisite for selecting a downloader. If an export arrives, inspect its actual image contents and mappings before spending effort on an importer.

## Generated Images and saved files

The Images guide documents downloading an opened image on the web. It distinguishes mobile “Remove from library,” where the image stays in the chat, from deleting a chat, which removes its generated images from Images. A separately saved Library or Space copy can survive chat deletion. [Images in ChatGPT](https://help.openai.com/en/articles/11084440-images-in-chatgpt).

The Library guide documents selecting one or more files for download and selecting several for deletion on the web. Generated images can appear as saved files; the article also directs users to Images for generated images. Library or Space availability varies by account. Deleting a saved file can affect projects using it while content may remain in chats. [Using Library to manage files in ChatGPT](https://help.openai.com/en/articles/20001052-using-library-to-manage-files-in-chatgpt).

**Unresolved:** these guides do not establish that native batch selection covers all of this user's generations, including older models and edits. Test the actual surfaces and batch behavior before treating native bulk download as sufficient.

**Project choice:** prefer native batches wherever they preserve original files and provide adequate coverage. Treat Images, saved files, and conversation contents as separate inventories linked by evidence, rather than assuming that their counts match.

## Observed gallery discovery failure

**User evidence:** scrolling loads more image rows until it stops. Refreshing and scrolling again stops around the same point. The page remains responsive. Images older than that stopping point can still be opened from their containing chats.

**Conclusion:** the gallery currently omits some images that remain accessible to this user. The root cause is unconfirmed; a failed subsequent load, a problematic item, or a service-side listing boundary are hypotheses. This observation does not establish a numeric limit or prove that all older image downloads work.

OpenAI documents finding older chats through search even when they are no longer visible in the sidebar. Regular and archived chats can appear in results; search matches words in titles or messages. This offers a way to locate known older chats, not a documented exhaustive inventory of all chats or images. [Finding your chats, projects, and files in ChatGPT](https://help.openai.com/en/articles/10056348-finding-your-chats-projects-and-files-in-chatgpt).

**Fallback:** conversation-based discovery can reach known older images if the proposed batch loop cannot advance. Establish whether normal history navigation can enumerate conversations beyond the image-gallery boundary and whether each conversation exposes all its image outputs. Search can recover known items but cannot establish completeness without further evidence.

## Native batch download and the proposed loop

**User evidence:** with the download folder configured beforehand, selecting 500 images in Chrome triggered 500 individual downloads. A later Firefox run was reported as 704 selected; its screenshot shows 703 currently selected at `chatgpt.com/space?category=images`. The Space list visibly offers native Download and Delete controls. Several browser download entries were marked complete. The earlier Chrome surface has not been separately identified.

The user subsequently reported that the Firefox run finished, its selection disappeared, and the images remained in the list. A read-only inspection of the local downloads directory found 703 image files totaling 1,717,714,337 bytes, with no `.part` or `.crdownload` files. That count agrees with the screenshot's visible selection count. It does not prove the exact trigger-time selection, original-file integrity, or source-to-file correspondence. The collection's previous folder contents were not recorded.

**Observed failure:** the native workflow loses the record connecting a completed download batch to its source selection. Native downloading is available; a later live check established sample selection recovery as described below. Matching archived files to that saved source selection remains unresolved. No maximum batch size has been established.

**User proposal:** work in batches of about 100 images, delete the finished batch, and iterate.

**Leading approach:** save a manifest before triggering native downloads, match and verify each file, then restore exact verified cleanup targets from that manifest. A smaller batch reduces the amount to reconcile but does not solve selection loss. Test whether removing a verified batch and refreshing exposes older items without deep scrolling. If it does, exhaustive account discovery is not a prerequisite for processing each batch. That replenishment behavior is still a hypothesis.

Chrome's extension downloads API can monitor downloads using its `downloads` permission. It distinguishes a download's creation from later state changes, including completion and interruption, and exposes download identifiers and filenames. This supports evaluating an extension monitor; it does not establish how ChatGPT source items map to downloads or provide local image validation by itself. [Chrome downloads API](https://developer.chrome.com/docs/extensions/reference/api/downloads).

Firefox also documents a WebExtensions downloads API with download-manager queries and creation/change events. Chrome and Firefox are candidates for later adapters; native success does not prove that an automated integration behaves identically in each. [Mozilla downloads API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/downloads).

The screenshot confirms that native Space selection offers Delete. Its confirmation text, actual saved-file effects, and behavior after refresh still need observation. If cleanup requires deleting containing chats, batch sizing must include whole verified chats.

## Live identifier and selection check

An authorized feasibility check in the connected in-app browser on 2026-11-26 found 50 rendered Space items, each with a distinct `data-library-selection-id`. Each value matched the identifier in that item's normal `/space/file/libfile_...` link. All 50 identifier, filename, and link associations remained unchanged after refresh. The separate `data-library-selection-key` changed for all 50.

The check then selected two items by their saved file identities, refreshed, confirmed that selection was cleared, and reselected exactly the same two identities. It finished with both test checkboxes cleared. The check performed inspection and temporary selection changes, with no downloads or deletion.

**Result:** the current Space DOM exposes a usable identity candidate, and exact selection recovery worked for the tested sample. This does not establish full-account enumeration, behavior after deep scrolling or reordering, compatibility with other accounts, download-to-file mapping, or deletion effects. No account identifiers, file titles, or raw DOM captures are needed in the public specification.

## Browser target decision

**User decision:** target the ChatGPT app's connected in-app browser first; support Chrome and other mainstream browsers over time.

**Initial shape:** an agent-assisted workflow in the existing app, using normal browser controls and a persistent local batch record. Keep browser-specific inspection, selection, download observation, and cleanup actions behind an adapter. Archive verification and cleanup eligibility remain independent of the browser. Chrome follows the initial target; Firefox is also a priority because the user already uses it. Other Chromium-based browsers can be evaluated afterward.

**Integration limit:** successful inspection through this chat's browser tools establishes access in this session. It does not establish a public standalone attachment interface, background-service support, persistent control across app restarts, or uninterrupted long runs. Test those separately and honor the browser host's confirmation and takeover requirements.

## Chat deletion

OpenAI documents individual chat deletion and a native “Delete all chats” control. Deleted chats cannot be restored through ChatGPT or Support. Removal from the account UI and permanent backend deletion are different events, with retention exceptions described by OpenAI. [Deleting and archiving chats in ChatGPT](https://help.openai.com/en/articles/8809935-deleting-and-archiving-chats-in-chatgpt).

The retention guide says chat deletion may offer an “Also delete files uploaded or created in this chat” checkbox. Selecting it moves eligible files to Trash; separately saved files remain if the option is absent or cleared. Inspect the actual dialog and target scope. [Chat and file retention in ChatGPT](https://help.openai.com/en/articles/8983778-chat-and-file-retention-in-chatgpt).

**Project choice:** targeted conversation deletion is an acceptable cleanup route. Native selection-based image/file cleanup may be simpler if its controls and actual effects are established. Native account-wide deletion could reduce work later, but requires an explicit account-wide scope and stronger coverage evidence than a successful subset download. It is excluded from the first version.

## Browser automation and account risk

The Europe Terms of Use prohibit automated or programmatic extraction of data or output and bypassing restrictions or protective measures. This is a material limitation for both an extension and an external browser controller; visible operation and human-like timing do not establish an exemption or guarantee protection from account restrictions. [Europe Terms of Use, “Using our Services”](https://openai.com/policies/eu-terms-of-use/).

**Project implication:** browser automation remains a technical candidate with a documented account risk. Do not describe it as a supported export mechanism or market it as ban-proof. Standard UI use, bounded operation, and stopping on access challenges are design choices, not evidence that automated extraction is permitted.

## CachyOS and browser tooling

Playwright supports Python browser automation and visible Chromium operation, but its published Linux requirements list Debian and Ubuntu, not CachyOS. Browser binary versions and host dependencies need validation on the actual host. [Playwright installation](https://playwright.dev/python/docs/intro), [Playwright browsers](https://playwright.dev/python/docs/browsers).

**Project implication:** do not promise native compatibility before a smoke check. An extension in the user's existing browser is another candidate. A Linux container with a supported userspace is a fallback to evaluate only if native browser tooling becomes a blocker; requiring a different host OS is outside scope.

## Comparison of candidate approaches

The following is an engineering assessment, not a claim that any method has already been proven on the user's accounts.

| Approach | Potential benefit | Main unresolved issue | Current position |
| --- | --- | --- | --- |
| Native batch download, followed by local verification and repeated cleanup | Reuses the demonstrated Chrome and Firefox workflows | Restoring selection from source identities, original variants, download mapping, deletion semantics, and whether older items become reachable | Leading candidate; Firefox Space controls observed and 703 local image files inventoried |
| Connected in-app browser adapter | Already supports direct DOM inspection and sample selection recovery in this chat | Download-to-file mapping, durable continuation, cleanup outcomes, host confirmations | Chosen initial browser target |
| Custom extension in Chrome or Firefox | Uses the browser session and could capture native download results | Browser-specific permissions, durable progress, UI changes, account isolation, terms risk | Later adapter candidate |
| External visible browser controller | Could coordinate a standalone browser workflow | CachyOS compatibility, integration, login behavior, UI changes, terms risk | Later candidate if standalone control is needed; no framework selected |
| Export recovery plus local import | Offline processing after a single official download | Previous delivery failure and unproven original-image coverage | Optional fallback |
| Existing third-party extension | Could avoid building acquisition features | Must verify source, permissions, file quality, coverage, maintenance, and cleanup behavior | No extension selected or audited |

Undocumented API scraping and stealth tooling are outside the proposed scope. They do not resolve the requirements for originals, completeness, or verified deletion.

## Decisions still open

1. How can the observed `libfile_...` identities be reliably correlated with native download results and verified local files?
2. Does removing a verified batch and refreshing expose previously unreachable older items?
3. Can the chosen in-app browser support durable batch runs, and which integration should later Chrome and Firefox adapters use?
4. Which identifiers and variant mappings establish the original image and its containing chat?
5. Can discovery establish sufficient completeness for automatic conversation cleanup when needed, including alternate branches, and can conversation history serve as a fallback beyond the gallery boundary?
6. What continuation, user interaction, and packaging are needed beyond the initial agent-assisted in-app workflow?

The initial browser target is selected. Language, framework, standalone integration, and packaging beyond the initial agent-assisted workflow remain open.
