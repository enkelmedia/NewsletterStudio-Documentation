# AGENTS.md

Guidance for AI agents and automation working in this repository.

## Project Overview

This repository contains the source Markdown documentation for Newsletter Studio, published at https://newsletterstudio.org.

Documentation is versioned under `content/package/<version>/`. The current version is defined in `content/package/product.yml`;

## Repository Structure

- `content/package/product.yml` defines product metadata, current version, and version groups.
- `content/package/readme.md` is the package-level landing content.
- `content/package/<version>/readme.md` is the overview page for a specific version.
- `content/package/<version>/nav.yml` defines the navigation for that version.
- `content/package/<version>/getting-started/`, `concepts/`, `develop/`, `extensions/`, and `other/` contain version-specific documentation pages.
- `media/` contains shared images, screenshots, diagrams, GIFs, and videos used by the docs.

## Target Audience
- Texts in these docs are targeted towards both editors (end users) and developers. 
- You'll notice the target for each page based on the content, pages with many images and less code is most likely target towards editors while pages with code samples are target towards developers.
- Try keeping this focus when working in the files.

## Editing Guidelines

- Prefer editing the newest documentation version first unless the request names a specific version.
- When changing behavior that applies to multiple supported versions, check whether the same page exists in older version folders and decide whether the update should be copied there too.
- Keep version-specific differences intact. Do not blindly synchronize pages across versions when product behavior or Umbraco support differs. But feel free to propose additions if you think it's needed.
- If adding a new page, update the matching `nav.yml` for each affected version.
- Use existing section names, front matter, heading levels, and wording style as the local pattern.
- Keep Markdown concise and practical. These docs should help users complete tasks, not read marketing copy.
- Use root-relative or existing relative links consistently with nearby pages.
- Place new screenshots or diagrams in `media/` or an established subfolder such as `media/extensions/...`, then reference them from Markdown.
- Do not rename or move media files unless the documentation references are updated at the same time.
- Be aware that the documentation contains older versions that are no longer supported.
- When asked to apply changes to older versions, only apply changes to the currently supported versions. This page https://www.newsletterstudio.org/installation/ will list currently supported versions.
- Links to other files should start with `../` even if the file is located in the same folder.

## Proofreading Guidelines

- Treat proofreading as a narrow pass. Focus on typos, grammar, punctuation, wrong words, broken examples, and obvious mistakes.
- Do not suggest or make major rewrites, structural changes
- For "latest" documentation, confirm the current version in `content/package/product.yml` before choosing a file.
- Preserve the author's voice and existing terminology where possible.
- Keep edits small and easy to review. Prefer line-level wording fixes over paragraph rewrites.
- Fix obvious technical mistakes in examples when found, such as invalid JSON syntax or incorrect file names.
- If only asked for suggestions, provide concise suggestions with file and line references rather than editing the file.
- If asked to apply proofreading fixes, edit only the requested file or version unless the user asks to update older versions too.
- After applying proofreading edits, run `git diff --check` when possible and mention any unrelated working tree changes separately.

## Review Guidelines
- When asked to review a page, look for ways to improve the content, missing information, potential additions and places where a context is assumed but not explained.
- During review you are allowed to propose structural changes
- Look for ideas on how to make the text easier to understand for the reader.

## Markdown Conventions

- Documentation pages should start with YAML front matter:

  ```yaml
  ---
  title: Page title
  description: Short description.
  ---
  ```

- Use fenced code blocks with a language when possible.
- Use the custom hint macro for callouts:

  ```markdown
  {% hint style="info" %}
  Helpful information.
  {% endhint %}
  ```

- Use the custom contrib macro when linking to code from the NewsletterStudioContrib repository:

  ```markdown
  {% contrib file="V14/Extensions-Demos/Demo.Web/Extensions/EmailEditorControl/HelloEmailControlType.cs" %}
  {% endcontrib %}
  ```

## Product And Version Notes

- Newsletter Studio major versions above 9 map to the same Umbraco major versions.
- Current version mappings live in `content/package/product.yml`.
- Do not assume a package, class, API, or UI feature exists in every version. Verify against the relevant version folder before documenting it.
- For current-version work, start in `content/package/17.0.0/`.

## Quality Checks

- Check links and image paths manually when adding or moving content.
- Check front matter formatting on new pages.
- Check `nav.yml` indentation carefully; it is whitespace-sensitive YAML.
- Keep examples accurate for the documented Newsletter Studio and Umbraco version.
- Before finishing, run `git diff --check` when possible to catch whitespace issues.

## Git And Change Safety

- The repository may contain user changes. Do not revert unrelated work.
- Keep changes scoped to the requested documentation update.
- Avoid formatting churn across many version folders unless the task explicitly calls for it.
- Mention any version folders intentionally left unchanged when the same topic exists elsewhere.
