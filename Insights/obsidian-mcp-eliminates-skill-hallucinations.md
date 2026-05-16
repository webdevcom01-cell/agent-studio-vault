---
type: insight
created: "2026-05-15"
tags:
  - mcp
  - agent-studio
  - skill
context: Building custom Obsidian MCP server during Claude Cowork session
---

# Pravi MCP server eliminiše halucinacije u skill-ovima za logovanje

## What I noticed

Knowledge-logging skills (poput `obsidian-knowledge-logger`) mogu da haluciniraju — simuliraju uspešno čuvanje note bez ikakve stvarne akcije. To se dešava kada skill nema pravi alat za pisanje i oslanja se na vlastitу procenu šta se "desilo".

Dodavanje filesystem-based MCP servera (`obsidian-mcp-server`, stdio transport) menja dinamiku: svaki poziv `obsidian_create_note` ili `obsidian_update_note` ide direktno na disk i vraća `bytes_written` kao dokaz. Halucinacija postaje strukturalno nemoguća — greška se vraća kao tool error, ne kao tiha propust.

## Why it matters

- **Pouzdanost > brzina**: bolje je dobiti pravi error nego lažni success
- **Audit trail**: svaka nota postoji na disku sa `created` datumom u frontmatter-u
- **Tag reuse**: `obsidian_list_tags` pre čuvanja sprečava duplikate tagova (`agentorchestration` vs `agent-orchestration`)
- **Wikilink integritet**: `obsidian_search_notes` pre pisanja linka znači da `[[Note Name]]` uvek postoji — nema orphaned linkova

## Related
- [[niche-glossary]]
