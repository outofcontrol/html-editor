# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single static HTML page (`public/index.html`) that demonstrates a TinyMCE 6.7.2 rich-text editor with a live two-pane layout: the editor on the left, the generated HTML source on the right. There is no build system, no package manager, and no test suite. TinyMCE is loaded from a CDN, so the page has no local dependencies.

## Running

Open `public/index.html` directly in a browser, or serve the `public/` directory with any static server (e.g. `python3 -m http.server` from `public/`). No build or install step.

## Architecture

Everything lives in `public/index.html`. Two `<textarea>` elements are bound together:

- `#previewCode` is upgraded into a TinyMCE editor via `tinymce.init`.
- `#sourceCode` is a plain textarea showing the editor's HTML output.

Data flow is two-way and manual:
- Editor `change`/`keyup` events call `updateSource()`, which writes `ed.getContent()` into `#sourceCode`.
- A `keyup` listener on `#sourceCode` calls `updatePreview()`, which pushes the raw HTML back into the editor via `tinymce.get('previewCode').setContent(...)`.

## Key behavior: paste sanitization

The `paste_postprocess` callback in `tinymce.init` is the substantive logic in this file. It runs on every paste and:
1. Strips `dir`, `role`, and `aria-level` attributes from all pasted elements (these come from Google Docs / other rich sources).
2. Removes empty elements (no children, or no text and no child elements).
3. Unwraps `<p>` tags nested inside `<li>` (moves children up, deletes the `<p>`).

When changing paste handling, edit this callback. The TinyMCE config also constrains tables (`table_class_list` allows only `''` or `table`; `table_row_class_list` allows `''` or `intermission`; advanced tab disabled). Editor config (`forced_root_block: 'p'`, `force_br_newlines`, `element_format: 'html'`) controls the markup TinyMCE emits.

## Notes

- Bumping the TinyMCE version means changing the CDN `<script>` URL on line 6. Verify that config keys still exist for the target version (some classic-era keys like `paste_auto_cleanup_on_paste` are no-ops in TinyMCE 6).
- Indentation in this file is inconsistent (mixed tabs and spaces); match the surrounding lines when editing.
