# Custom Build and Install Guide

## Project

Repository:
- hongshw/llm-for-zotero

Custom branch:
- feature/selection-translate

Official baseline:
- llm-for-zotero v3.8.31
- commit 09577544dd57e54c0f92352ed250805cc8005e90

Verified Selection Translate V1:
- commit 9154712f92c8f9c275110452c552c464314f242d
- tag custom-v3.8.31-selection-translate-v1

## Required environment

Verified development environment:

- macOS on Apple Silicon
- Node.js v24.19.0
- npm 11.17.0
- Git 2.50.1
- Homebrew 6.0.15
- GitHub CLI 2.97.0

Node.js was installed from the official Node.js installer.

Homebrew Node is not required.

## Clone custom repository

    git clone https://github.com/hongshw/llm-for-zotero.git
    cd llm-for-zotero
    git switch feature/selection-translate

## Install dependencies

For the current verified checkout:

    npm install

Do not automatically run:

    npm audit fix
    npm audit fix --force
    npm update

Dependency upgrades should be handled separately from Selection Translate development.

## Validation

Run:

    npm run typecheck
    npm run test:unit
    npm run build

Verified V1 results:

- typecheck: PASS
- unit tests: 2690 passing, 0 failing, 1 pending
- production build: PASS

## XPI output

The production build generates:

    .scaffold/build/llm-for-zotero.xpi

To reveal the build directory in Finder:

    open .scaffold/build

## Install into Zotero

1. Open Zotero.
2. Open the Plugins/Add-ons manager.
3. Choose Install Plugin From File or Install Add-on From File.
4. Select .scaffold/build/llm-for-zotero.xpi.
5. Replace the existing llm-for-zotero installation if prompted.
6. Restart Zotero if requested.

Plugin preferences and Quick Actions are normally stored in the Zotero profile,
so replacing the XPI should not require recreating the Translate prompt.

## Required Quick Action

Selection Translate V1 expects an editable llm-for-zotero Quick Action with:

    Label: Translate

The translation prompt is intentionally managed through the Quick Action UI
and is not hard-coded into the plugin source.

## Real-world acceptance test

After installing the XPI:

1. Open a PDF.
2. Open the LLM Assistant panel.
3. Select a short English passage.
4. Confirm Add Text appears.
5. Confirm Translate appears.
6. Test Add Text and confirm the selected text is added normally.
7. Select another passage and click Translate.
8. Confirm the selected text is automatically added.
9. Confirm the Translate Quick Action is automatically triggered.
10. Confirm the AI reply appears in the same conversation.
11. Confirm a single click sends only one request.

## Recovery point

The verified V1 feature can always be recovered from:

    custom-v3.8.31-selection-translate-v1

or commit:

    9154712f92c8f9c275110452c552c464314f242d
