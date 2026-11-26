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

## Chat deletion

OpenAI documents individual chat deletion and a native “Delete all chats” control. Deleted chats cannot be restored through ChatGPT or Support. Chat deletion does not remove separately saved Library or Space files. Removal from the account UI and permanent backend deletion are different events, with retention exceptions described by OpenAI. [Deleting and archiving chats in ChatGPT](https://help.openai.com/en/articles/8809935-deleting-and-archiving-chats-in-chatgpt).

**Project choice:** targeted conversation deletion is acceptable to the user and is the initial cleanup unit. Native account-wide deletion could reduce work later, but requires an explicit account-wide scope and stronger coverage evidence than a successful subset download. It is excluded from the first version.

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
| Native Library or Space batch download, followed by local verification | Smallest amount of custom acquisition work | Coverage of generated images, original variants, batch limits, and mapping to cleanup targets | First capability to check |
| Custom extension in the existing browser | Uses the current browser session and could capture native download results | Browser-specific permissions, durable progress, UI changes, account isolation, terms risk | Candidate |
| External visible browser controller | Can coordinate navigation, file verification, progress, and cleanup | CachyOS compatibility, login behavior, UI changes, terms risk | Candidate; Playwright is one possible tool |
| Export recovery plus local import | Offline processing after a single official download | Previous delivery failure and unproven original-image coverage | Optional fallback |
| Existing third-party extension | Could avoid building acquisition features | Must verify source, permissions, file quality, coverage, maintenance, and cleanup behavior | No extension selected or audited |

Undocumented API scraping and stealth tooling are outside the proposed scope. They do not resolve the requirements for originals, completeness, or verified deletion.

## Decisions still open

1. Which surfaces contain the full accessible historical image collection on each account?
2. Can native batch downloads cover enough of the collection to avoid a custom downloader?
3. If automation is needed, is an extension or an external controller the better fit for the installed browser and CachyOS environment?
4. Which identifiers and variant mappings establish the original image and its containing chat?
5. Can discovery establish sufficient completeness for automatic conversation cleanup, including alternate branches?
6. Does the user prefer a CLI or a small local interface once the feasible workflow is known?

These decisions belong to the feasibility stage. No language, framework, browser integration, or packaging format is committed yet.
