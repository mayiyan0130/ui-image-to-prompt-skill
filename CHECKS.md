# Package checks

- Skill source: `ui-image-to-prompt/SKILL.md`
- Packaged archive: `dist/ui-image-to-prompt.skill`
- SHA256: `c2f3800e89a865f53f2337fde26e4df069643acaed6e4254fd9a70331fb1621f`
- Archive layout: top-level `ui-image-to-prompt/` folder containing `SKILL.md`

## Validation performed

- Frontmatter contains required `name` and `description` fields.
- Skill name is hyphen-case and portable.
- Description stays under the OpenClaw validator limit.
- Skill body is under 500 lines.
- Packaged `.skill` archive can be expanded and contains the expected `SKILL.md` path.
- Repository includes both source folder and packaged archive for portable installation.
