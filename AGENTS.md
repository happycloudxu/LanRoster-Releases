# Agent notes

This repository is one of the projects under `D:\CopilotCode-Space`.

## Commit identity

Every commit here is authored **and** committed by `happycloudxu` alone:

    git config user.name happycloudxu
    git config user.email 202728096+happycloudxu@users.noreply.github.com

Do not append `Co-authored-by:` lines to commit messages, tag messages, or release notes.
GitHub renders a co-author trailer as "happycloudxu and Copilot" on the commit and on every page
that lists it, which is not wanted in these repositories. If a history is rewritten for any
reason, strip such trailers, then force-push the branch together with any tags whose target moved.

## Browser panels

Do not open browser canvases inside the Copilot desktop app. The app restores panels a session
opened, so one that was useful for a minute reappears on every reload and gets in the way. Use
`gh api`, `gh repo view`, `git`, or a fetch tool to read a web page; if a page genuinely has to
be seen, ask the user to open it. Do not navigate, reload, or otherwise drive an existing panel
either.

## Repository

| Item | Value |
|---|---|
| Default branch | `main` |
| `origin` | `https://github.com/happycloudxu/LanRoster-Releases.git` |
| Owner | `happycloudxu` |