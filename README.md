# AGENTS.md

A universal set of engineering guidelines for AI coding agents working on web projects.

## Overview

This repository provides a shared instruction file that helps coding agents produce consistent, secure, accessible, and maintainable changes. It prioritizes correctness and security, accessibility, repository conventions, clean architecture, and a polished user experience.

## Usage

Copy [AGENTS.md](./AGENTS.md) to the root of a project:

```bash
curl -L https://raw.githubusercontent.com/iamapinan/AGENTS.md/main/AGENTS.md -o AGENTS.md
```

Coding agents can then read `AGENTS.md` automatically or when explicitly pointed to repository instructions.

## Guideline areas

- Prioritizing requirements when instructions conflict
- Planning and coding workflows
- Architecture and strict typing conventions
- Frontend accessibility and responsive UI standards
- API, backend, and security requirements
- UI polish and micro-interactions
- Pre-finish verification checks

Explicit user instructions take precedence over this file.

## Contributing

Contributions should keep the guidelines:

- concise and actionable;
- framework-neutral unless a web convention is explicitly required;
- consistent with correctness, security, and accessibility priorities; and
- free of project-specific assumptions that belong in a separate repository.

Use Conventional Commit messages for changes, for example:

```text
docs: clarify accessibility requirements
fix: correct security guidance
```

## License

No license has been added to this repository. All rights remain with the repository owner unless a license is explicitly provided.
