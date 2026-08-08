# Custom Upgrade Workflow

## Repository layout

- Custom fork: hongshw/llm-for-zotero
- origin: https://github.com/hongshw/llm-for-zotero.git
- upstream: https://github.com/yilewang/llm-for-zotero.git

## Verified V1 baseline

Official llm-for-zotero:
- Version: v3.8.31
- Commit: 09577544dd57e54c0f92352ed250805cc8005e90

Selection Translate V1:
- Branch: feature/selection-translate
- Commit: 9154712f92c8f9c275110452c552c464314f242d
- Tag: custom-v3.8.31-selection-translate-v1

Validation:
- npm run typecheck: PASS
- npm run test:unit: 2690 passing, 0 failing, 1 pending
- npm run build: PASS
- Zotero real-world validation: PASS

## Update policy

Do not base the custom plugin on unreleased upstream/main.

Wait for an official release tag such as v3.8.32 or v3.8.33, then create a new branch from that official tag and reapply the custom feature.

## Check official updates

Run:

    git fetch upstream --tags
    git log -1 --oneline upstream/main
    git tag --sort=-version:refname | head -5
    git status

## Upgrade example

For a future v3.8.32 release:

    git fetch upstream --tags
    git switch -c feature/selection-translate-v3.8.32 v3.8.32
    git cherry-pick 9154712f92c8f9c275110452c552c464314f242d

If cherry-pick reports conflicts, inspect upstream changes carefully. Preserve new upstream behavior and then reapply Selection Translate.

The two V1-modified files are:

- src/modules/contextPanel/index.ts
- src/modules/contextPanel/shortcuts.ts

## Validation after upgrade

Run:

    git --no-pager diff --check
    npm run typecheck
    npm run test:unit
    npm run build

Then install:

    .scaffold/build/llm-for-zotero.xpi

Real-world regression checks:

1. Original Add Text still works.
2. Translate appears in the PDF selection popup.
3. Translate adds the selected text to the correct LLM conversation.
4. Translate triggers the editable Quick Action whose label is Translate.
5. AI replies in the same conversation.
6. One click produces only one request.

## Architecture rule

Selection Translate reuses existing llm-for-zotero machinery.

- runShortcutByLabel() finds and clicks an existing Quick Action.
- addTextToPanel() adds selected text and returns the actual target panel.
- runSelectedTextShortcut() waits for Add Text to succeed before triggering the Quick Action.
- The translation prompt is not hard-coded in source code.
- The Quick Action label currently used by the popup is: Translate.

Future popup actions should reuse the same architecture rather than duplicating provider, API, or chat-send logic.
