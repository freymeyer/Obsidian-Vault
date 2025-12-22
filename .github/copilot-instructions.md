# Copilot Instructions for Obsidian Vault

This workspace is an Obsidian Vault focused on Cybersecurity, Infrastructure, and AI.

## Knowledge Graph Structure
- **Maps of Content (MOC)**: Use MOC files (e.g., `000 Home MOC.md`, `010 Cybersecurity MOC.md`) as entry points and structural guides.
- **Linking**: ALWAYS use `[[Wikilinks]]` for internal links, not standard markdown links (e.g., `[[Docker]]`, not `[Docker](Docker.md)`).
- **Hierarchy**:
  - `000 Home MOC.md` is the root index.
  - Numbered MOCs (e.g., `010`, `020`) represent top-level domains.
  - Individual notes (e.g., `Docker.md`) should be linked from their respective MOCs.

## File Conventions
- **Frontmatter**: All notes must start with YAML frontmatter containing hierarchical tags with `#` prefix.
  ```yaml
  ---
  tags:
    - "#cybersecurity/blue-team/soc"
    - "#interview/concepts"
  aliases:
    - Short Name
  ---
  ```
- **Tag Hierarchy**: Use `/` for nesting (e.g., `#cybersecurity/web-security/xss`, `#infrastructure/containers`).
- **Naming**: Keep filenames concise and descriptive. MOCs follow the pattern `### 🏷️ Name MOC`.
- **Content**: Focus on technical accuracy, clear definitions, and practical examples (commands, code snippets).

## Editing Guidelines
- When adding a new note, ensure it is linked from the most relevant MOC to maintain the graph structure.
- Use standard Markdown for formatting (headers, lists, code blocks).
- Avoid creating folders unless necessary; prefer a flat structure organized by MOCs and links.
- **Always check for existing notes** before creating new ones—link to them via `[[Wikilinks]]` instead of duplicating content.

## Workflow: Concept Extraction from THM Rooms & Writeups

### When to Trigger
- Files tagged with `#ctf/thm/writeup`, `#ctf/htb/writeup`, or `#learning/lab`
- User explicitly requests concept extraction from a writeup or room notes

### Process
1. **Scan for Unfamiliar Concepts**: Look for terms, techniques, or tools mentioned in the writeup that don't have existing notes.
2. **Check Existing Notes**: Search the vault for existing `[[Wikilinks]]`—if a concept already exists, link to it rather than creating a duplicate.
3. **Extract & Create**: For each new concept:
   - Create a new note using `Templates/Concept Template.md` as the structure.
   - Use `Authorization.md` as the **gold standard** for tone, depth, and formatting.
   - Populate sections based on context from the writeup + external knowledge.

### Concept Note Requirements
- **One-liner**: A single sentence explaining the concept.
- **What Is It?**: Clear definition with context.
- **How It Works**: Technical explanation, diagrams, or code if applicable.
- **Detection & Prevention**: Blue team perspective—how to detect or mitigate.
- **Interview Angles**: Common questions and a STAR story if applicable.
- **Related Concepts**: `[[Wikilinks]]` to connected notes in the vault.

### Quality Checklist
- [ ] Frontmatter has proper `#`-prefixed hierarchical tags
- [ ] Note is **atomic**—one concept per note, not a copy of writeup text
- [ ] All mentioned concepts are linked via `[[Wikilinks]]`
- [ ] Note is linked from the appropriate MOC (e.g., `011 🛡️ Blue Team & SOC Operations MOC.md`)
- [ ] Code snippets and commands are in fenced code blocks with language hints
