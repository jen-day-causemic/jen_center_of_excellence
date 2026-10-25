# Business process flow (BPF) diagrams in Lucid

What worked, and what didn't, building current-state BPF swimlane diagrams with Claude and the Lucid connector (first used October 2026).

## Keep one source of truth
- Write each BPF once, in a markdown "companion" file: lanes, then one line per node with an ID, type, lane, label and its "Next" connections, plus scope, cross-references, open questions (with owners) and assumptions.
- Generate everything else from it with scripts: the written companion Google Doc, any Lucid prompts, and the Lucid import JSON. To change a flow, edit the source and regenerate. Never hand-edit the outputs.
- Give every node an ID (S01, E01, X01) shown at the start of its label, so a reviewer can check each arrow against the written list.

## Build diagrams with Lucid's native import
- Best: the connector's create-from-specification tool (Standard Import JSON). It makes real, editable swimlanes and shapes, places nodes exactly, and needs no pasting.
- Avoid Lucid's "Create with AI" for anything detailed. It drops content from long prompts, ignores layout instructions, and an edit prompt on an existing diagram rewires connectors unpredictably. If you must use it, paste short passes (header and lanes, nodes, connectors, styling), check each, and never send a fix-up prompt.
- Avoid the Mermaid import tool: it creates an unrendered Mermaid code block, not shapes.
- Shape and line IDs must be unique across every page of a multi-page import. Prefix them per page.
- Lay nodes out on a column grid: a step in another lane goes in the column after the step it follows. Without this, cross-lane steps drift left and arrows loop backward.
- The import's preflight flags intentional overlaps (text on a header bar, shapes inside a key box). Those are fine; check the real render with a PNG export.
- New documents land at the top level of the user's Lucid documents. Move them into the client folder afterward.

## Keep the diagram clean
- No sticky notes on the canvas. Sources and step notes go in the companion Doc; open questions and assumptions go in a side panel.
- Mark vacant roles with their own lane ("<name> (role vacant)"), so handoff gaps are visible.

## Google Docs written companion
- Write content through the Docs API from the same source, then verify by reading the Doc back and comparing every paragraph's text, heading level and bullet state with the source.
- Nested bullets: insert the text with a leading tab per level, apply heading and bold styles first, then run the bullet requests from the bottom of the tab upward. Removing the tabs shifts later positions.
- To replace a tab's content, it can be faster to add a new tab and delete the old one; tab titles must be unique, so rename the old tab first. This changes the tab's link.
