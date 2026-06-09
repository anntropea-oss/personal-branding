## [2026-06-05 17:47] Thread Reader Argument Rejection
- Problem: The first attempt to read the referenced Codex thread failed with an invalid arguments response while trying to include truncated tool outputs.
- Root Cause: Unknown
- Solution: Located the thread through the app thread index, then retried `read_thread` with the required thread id and without the extra output options.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The thread was successfully read and its project lessons were incorporated into this website scaffold.

## [2026-06-05 17:48] Playwright Verification Dependency Missing
- Problem: Browser rendering verification through the Node REPL failed because the `playwright` package was not installed in the local REPL environment.
- Root Cause: The workspace has no Node project dependencies and the Node REPL module path did not include Playwright.
- Solution: Switched verification to the installed Google Chrome binary in headless screenshot mode.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: Google Chrome was found at `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome` and used for the follow-up rendering check.

## [2026-06-05 17:50] Hero Layout Overflow
- Problem: Visual QA showed the desktop hero pushed the action buttons to the bottom edge of the first viewport, and the mobile hero headline overflowed horizontally.
- Root Cause: The hero minimum height and responsive headline sizing were too large for the available viewport after accounting for the header and mobile padding.
- Solution: Reduced the hero minimum height and padding, tightened the desktop headline scale, set the hero content to full width, and added mobile-specific headline width and size rules.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Resolved
- Verification: Re-ran browser screenshot capture after the CSS change and confirmed the desktop screenshot was regenerated; follow-up mobile verification continued with an isolated capture after stopping the hung Chrome process.

## [2026-06-05 17:51] Headless Chrome Capture Hung
- Problem: The chained headless Chrome screenshot command hung after writing the desktop screenshot and did not proceed to the mobile screenshot or HTTP checks.
- Root Cause: Unknown; Chrome emitted updater and registration errors while running in headless mode.
- Solution: Identified and terminated only the headless capture shell and Chrome child processes, then continued verification with separate commands.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The capture session exited after terminating PIDs `13862` and `13864`, while the local preview server remained active.

## [2026-06-05 17:52] Missing Timeout Utility
- Problem: Time-bounding screenshot commands with `timeout` failed because the command is not available in this macOS shell.
- Root Cause: GNU `timeout` is not installed by default on this system.
- Solution: Used a Perl `alarm` wrapper to bound individual Chrome screenshot commands.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The Perl-wrapped commands wrote desktop and mobile screenshot files before the alarm stopped Chrome.

## [2026-06-05 17:55] DevTools WebSocket Unavailable
- Problem: A Chrome DevTools Protocol mobile screenshot attempt failed because the Node REPL runtime did not provide a global `WebSocket` implementation.
- Root Cause: The local Node REPL environment lacks the browser-compatible WebSocket API and no WebSocket package is installed in the workspace.
- Solution: Avoided adding dependencies, constrained the mobile hero content column directly in CSS, and verified the result with headless Chrome screenshots.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Workaround
- Verification: The final mobile screenshot showed the headline, paragraph, and buttons fitting inside the visible column.

## [2026-06-05 17:56] Curl Asset Check Printed Binary Output
- Problem: A final HTTP verification command printed PNG binary data into the command output while checking the hero asset.
- Root Cause: Multiple URLs were passed to one `curl` invocation with a single output option, causing the second response body to be written to stdout.
- Solution: Re-ran the HTTP checks as separate quiet requests so only status codes and content types are reported.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up curl checks returned only concise HTTP status and content-type output.

## [2026-06-05 18:04] In-App Browser Automation Unavailable
- Problem: Attempting to verify the local preview through the in-app browser automation API failed because the `iab` browser backend was unavailable, and the browser backend list was empty.
- Root Cause: Unknown
- Solution: Continued verification with direct local HTTP checks and bounded Chrome screenshot commands instead of relying on the unavailable in-app browser API.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The browser runtime returned an empty backend list, confirming automation was unavailable for this session.

## [2026-06-05 18:07] Mobile Navigation Overflow
- Problem: After adding longer navigation labels, the mobile header exceeded the viewport width and pushed the `Contact` link offscreen.
- Root Cause: The mobile navigation used `justify-content: space-between` with five links and no wrapping behavior.
- Solution: Updated the mobile navigation to wrap with explicit row and column gaps, keeping all links visible within the viewport.
- Files Changed: styles.css, SOLUTIONS.md
- Status: Resolved
- Verification: Re-ran mobile screenshot verification after the CSS update to confirm the navigation fit within the viewport.

## [2026-06-05 18:21] LinkedIn Curl Verification Blocked
- Problem: Direct curl verification of the LinkedIn profile URL returned HTTP `999` instead of a normal success status.
- Root Cause: LinkedIn blocks some automated requests with anti-bot responses.
- Solution: Kept the public LinkedIn URL in the site and treated curl status as an automation limitation rather than a broken local link.
- Files Changed: index.html, SOLUTIONS.md
- Status: Workaround
- Verification: The local page continued to serve successfully, and the LinkedIn link is present in the resume section.

## [2026-06-05 18:21] Anchor Screenshot Captures Blank Page
- Problem: Headless Chrome screenshots of `#portfolio`, `#consulting`, and `#resume` anchor URLs produced blank images.
- Root Cause: Unknown; Chrome captured before the anchored page view painted.
- Solution: Retried section verification with a render budget and alternate screenshot flow.
- Files Changed: SOLUTIONS.md
- Status: Workaround
- Verification: The first anchor screenshots were inspected and confirmed blank before retrying with a different capture approach.

## [2026-06-05 18:23] Repeated Curl Multi-URL Output Issue
- Problem: A combined curl verification command again printed PNG and other response bodies into command output while checking multiple local URLs.
- Root Cause: Multiple URLs were passed through one curl command with output formatting that did not isolate each response body.
- Solution: Stopped using multi-URL curl for verification and re-ran each URL check as a separate quiet request.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up checks used one URL per curl command and returned only status codes and content types.

## [2026-06-08 20:50] Pre-Deploy Placeholder Metadata
- Problem: The site still used `https://example.com/` for canonical, Open Graph, robots, and sitemap URLs, and the public contact button used `hello@example.com`.
- Root Cause: Initial scaffold placeholders had not yet been replaced with production deployment values.
- Solution: Updated production metadata to the GitHub Pages project URL, changed the contact CTA to LinkedIn, refreshed the sitemap `lastmod`, and added `.nojekyll` for static GitHub Pages serving.
- Files Changed: index.html, robots.txt, sitemap.xml, README.md, .nojekyll, SOLUTIONS.md
- Status: Resolved
- Verification: Local HTTP checks returned `200` for the homepage, hero asset, `robots.txt`, and `sitemap.xml`; live GitHub Pages checks returned `200` for the homepage, hero asset, `robots.txt`, and `sitemap.xml`.

## [2026-06-08 20:51] Local Preview Server Stopped
- Problem: Pre-deploy local HTTP checks returned status `000` because `http://localhost:4173/` was no longer accepting connections.
- Root Cause: The earlier local preview server session had stopped.
- Solution: Restarted the local static server with `python3 -m http.server 4173`.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: Follow-up local HTTP checks returned `200` for the homepage, hero asset, `robots.txt`, and `sitemap.xml`.

## [2026-06-08 20:52] Git Commit Identity Missing
- Problem: The repo had no configured Git author name or email, which would prevent creating the deployment commit.
- Root Cause: Local Git identity was not configured for this new repository.
- Solution: Set a local-only Git identity using the authenticated GitHub account name and no-reply email.
- Files Changed: .git/config, SOLUTIONS.md
- Status: Resolved
- Verification: `git config user.name` and `git config user.email` returned configured values after the update.

## [2026-06-08 20:53] GitHub Pages API Payload Rejected
- Problem: The first attempt to enable GitHub Pages returned HTTP `422` with “Invalid input: data matches no possible input.”
- Root Cause: The API call used an invalid payload shape for the Pages `source` object.
- Solution: Retried the Pages enablement call with nested `source[branch]` and `source[path]` fields.
- Files Changed: SOLUTIONS.md
- Status: Resolved
- Verification: GitHub Pages enablement returned `html_url` as `https://anntropea-oss.github.io/personal-branding/` with HTTPS enforced.
