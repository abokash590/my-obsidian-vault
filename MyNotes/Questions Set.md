Claude Account Manager — Extension
Install (developer mode, unpacked)
Unzip this folder somewhere permanent (don't delete it after installing — Chrome loads the extension live from this folder).
Go to `chrome://extensions`.
Turn on Developer mode (top-right toggle).
Click Load unpacked.
Select this `extension` folder (the one containing `manifest.json`).
Pin the extension icon (puzzle-piece icon in the toolbar → pin "Claude Account Manager").
First use
Click the extension icon → click Login.
A new tab opens at claude.ai's login page — log in normally (this step is always manual, the extension never touches your password).
Once you're logged in, the extension detects it automatically and adds the account to the list.
Repeat for each of your accounts.
Click the refresh icon in the popup to sync status for all accounts (checks usage limits + recent chats).
⚠️ Before this fully works: selectors need verifying
claude.ai has no public API. This extension reads its interface (DOM), and
a few CSS selectors were written as best-guesses since the live page
structure wasn't available while writing this code. They're all centralized
at the top of `content.js` in one `SELECTORS` object — marked
`VERIFY LIVE` — specifically:
`accountEmail` — where your logged-in email appears in the account/profile menu
`usageLimitBanner` / `usageLimitResetText` — the "you've hit your limit" message and its reset time
`sidebarChatItem` — each conversation entry in the left sidebar
`composerTextarea` — the message input box (for Bridge Context autofill)
`messageBubble` — individual chat messages (for building the Bridge Context summary)
To fix: open claude.ai, right-click the relevant element → Inspect, find
its actual `data-testid`/class, and update the matching line in
`SELECTORS`. This is the only maintenance this extension should ever need
unless Anthropic changes claude.ai's structure again later.
What's fully implemented vs. what depends on the selectors above
Feature	Status
Add / remove / pin / unpin accounts	✅ Fully working, no selector dependency
Switch & continue (cookie swap)	✅ Fully working, no selector dependency
Sync timer + lock UI	✅ Fully working
Toolbar badge count	✅ Fully working
Usage-limit detection	⚠️ Needs `usageLimitBanner` selector verified
Recent chat tracking	⚠️ Needs `sidebarChatItem` selector verified
Bridge context (extract + autofill)	⚠️ Needs `messageBubble` + `composerTextarea` verified
Login detection (email capture)	⚠️ Needs `accountEmail` selector verified
See `BUILD_SPEC.md` for the full behavior spec of every feature.