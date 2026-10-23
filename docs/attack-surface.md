# Attack Surface Analysis — Muse (Track A)

Channels through which content reaches the agent. One row per channel.

| # | Channel | Agent sees it? | What is needed to inject instructions |
|---|---|---|---|
| 1 | Visible page text | Yes | Place text in the rendered page body. |
| 2 | Hidden DOM element (display:none) | Only if the agent parses full HTML | Hide instructions in a non-rendered element. |
| 3 | HTML comment | Only if the agent keeps comments | Put instructions in an HTML comment. |
| 4 | Meta description tag | Only if the agent reads metadata | Put instructions in `<meta name="description">`. |
| 5 | Title tag | Yes | Put instructions in `<title>`. |
| 6 | Image alt attribute | Only if the agent reads alt text | Put instructions in an `img` alt attribute. |
| 7 | HTTP response header | No by default | Add a custom header; needs header-aware fetching. |
| 8 | Tool result | Yes | Return instructions as a tool result. |
| 9 | Stored memory (earlier turn) | Yes | Store instructions in agent memory from a prior turn. |
| 10 | Email subject line | Yes | Put instructions in an email subject the agent reads. |
| 11 | Document metadata | No by default | Put instructions in metadata fields; needs a metadata-aware parser. |
| 12 | Image EXIF metadata | No by default | Put instructions in EXIF data; needs an EXIF-aware parser. |

## Entry points that parse untrusted input (Track B view)

The Muse runtime cell parses untrusted content at these points: web page HTML, tool arguments, connector responses, email and document parsers, and memory recall. Each is reachable from the agent loop once the content is fetched or a tool runs. Guards that may apply: Sentinel approval for connector actions, the web-access gate for unfamiliar sites, and the per-action approval card for writes.
