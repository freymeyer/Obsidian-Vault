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
- **Frontmatter**: All notes must start with YAML frontmatter containing hierarchical tags.
  ```yaml
  ---
  tags:
    - infrastructure/containers
    - cybersecurity/blue-team
  ---
  ```
- **Naming**: Keep filenames concise and descriptive. MOCs follow the pattern `### 🏷️ Name MOC`.
- **Content**: Focus on technical accuracy, clear definitions, and practical examples (commands, code snippets).

## Editing Guidelines
- When adding a new note, ensure it is linked from the most relevant MOC to maintain the graph structure.
- Use standard Markdown for formatting (headers, lists, code blocks).
- Avoid creating folders unless necessary; prefer a flat structure organized by MOCs and links.

## Workflow: Writeup Parsing & Concept Extraction
- **Trigger**: When a file is tagged with `#ctf/thm/writeup` or `#learning/lab` (e.g., THM rooms).
- **Task**: Parse the writeup to extract core concepts (often found in "Knowledge Inbox").
- **Output Format**: Create new concept notes based on `Templates/Concept Template.md`.
  - **Reference**: Use `Authorization.md` as the gold standard example of a populated concept note.
- **Content Strategy**:
  - Use the writeup's content to populate the concept note's sections (Definition, How It Works, Detection).
  - Ensure the new note is atomic and reusable, not just a copy of the writeup text.
