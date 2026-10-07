# Recon

Surface-area notes for Muse. One file per area once it grows; for now a single
scratchpad.

## Purpose

Map *what* the product is before looking for *how* it could break. Recon here
is passive/read-only: reading public docs, observing your own session. Nothing
here touches other users or production.

## Leading questions

- [ ] What does a Muse session look like end to end? How is a sandbox spawned?
- [ ] Where are credentials kept (first-party tokens, connector OAuth, refresh
      tokens, connector secrets)? What can sandbox code reach?
- [ ] What network paths exist out of the sandbox? Which are by design?
- [ ] What does the control plane expose that a user controls?
- [ ] Where does user content enter that an injected prompt would read
      (files, emails, docs, transcripts)?
- [ ] What identifies a VM/sandbox instance — and is any of it guessable?
- [ ] What tool/connector tokens exist, and what scopes do they carry?

## Notes

<!-- Append dated notes as you go. Each entry: date, what you looked at,
finding or dead end, and the finding number if it produced one. -->