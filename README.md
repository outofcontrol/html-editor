<!--
SPDX-FileCopyrightText: 2026 Out of Control, Inc.
SPDX-License-Identifier: MIT
-->

# WYSIWYG HTML Sanitizer

A single static HTML page demonstrating a WYSIWYG rich-text editor with a live two-pane layout: the editor on the left, the generated HTML source on the right.

## Features

- **Live two-pane view.** Edits in the editor update the HTML source, and edits to the source update the editor.
- **Paste sanitization.** On every paste, the editor strips `dir`, `role`, and `aria-level` attributes (common in Google Docs content), removes empty elements, and unwraps `<p>` tags nested inside `<li>`.
- **Inline code button.** A toolbar button (and `Ctrl+E`) wraps the selection in a `<code>` tag.
- **Constrained tables.** Table and row classes are limited to a small allowed set.

## Getting started

No build step or dependencies. TinyMCE is loaded from a CDN.

Open `public/index.html` directly in a browser, or serve the `public/` directory with any static server.

## Demo site

You can test out the project on a live demo site here: 

https://outofcontrol.ca/editor/

