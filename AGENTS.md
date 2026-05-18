Claude Code Subagent Prompt: Gmail Summarizer → Google Sheet
Here's a prompt structured around the four-part framework from the chart (Goal, Context, Constraints, Done When):

Goal.
Build a Claude Code subagent that reads unread emails from my Gmail inbox, generates a short summary of each, and writes the results to a Google Sheet — one row per email.
Context.

Use the Gmail MCP server for inbox access (search_threads, get_thread).
Use the Google Drive / Sheets MCP server to create and populate the target spreadsheet.
Default scope: unread messages in the primary inbox from the last 24 hours, unless I specify otherwise at invocation time.
Sheet layout:

Column A → Subject line (verbatim from the email header)
Column B → Summary (one short paragraph, ~3–4 sentences, ~60–80 words)
Column C → Sender's email address (extracted from the From header, address only — strip the display name)


Row 1 is a header row: Subject | Summary | Sender Email.
New sheet name format: Inbox Summary — YYYY-MM-DD.

Constraints.

Do not mark emails as read, archive, label, or modify them in any way. Read-only on Gmail.
Summaries must be neutral and factual — what the email is about and what (if anything) it asks of me. No opinions, no suggested replies.
If an email is a newsletter, promo, or automated notification, still summarize it but lead the paragraph with the category (e.g., "Newsletter from…", "Automated alert from…").
Skip emails flagged as spam or in Promotions/Updates/Social tabs unless I explicitly include them.
Strip signatures, legal disclaimers, and prior quoted thread history before summarizing — summarize the newest message only.
Process emails in batches; do not load full thread bodies into context all at once.
No new dependencies or scripts — use the existing MCP connectors only.

Done when.

A new Google Sheet exists in my Drive with the date-stamped name.
Every qualifying unread email from the run window has exactly one row.
Columns A, B, and C are populated correctly for every row; no blanks, no truncated subjects.
The subagent reports back in chat with: sheet link, total emails processed, and any that were skipped (with reason).
No emails in Gmail were modified.


For complex runs (large inbox, weird thread structures), invoke with:

"Ask me clarifying questions before you write any code or call any tools."

Save this to AGENTS.md in the project so you don't re-paste it every session. If you end up running this daily, that's the signal to wrap it as a Claude Code skill ($skill-creator) and eventually automate it on a schedule — steps 2 → 3 → 4 in the chart.
