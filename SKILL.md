---
name: repository-guide-writer
description: Write high-signal AGENTS.md-style repository guides for unfamiliar projects. Use when asked to create or improve AGENTS.md, CLAUDE.md, repository guidelines, AI coding-agent docs, onboarding docs, or bilingual repo development guides.
disable-model-invocation: true
---

# Repository Guide Writer

## Purpose

Create an `AGENTS.md`-style repository guide that helps an AI coding agent and human maintainer work safely in an unfamiliar project. The guide should be concrete, verified from the repo, and shaped around how work actually happens in that codebase.

## Core Principle

Write a practical operating manual, not a generic README. Prefer named files, commands, modules, invariants, pitfalls, and workflows that an agent can follow without guessing.

## Workflow

### 1. Inspect Before Writing

Read the repository before drafting. Start with:

- Existing agent docs: `AGENTS.md`, `AGENTS-zh.md`, `CLAUDE.md`, `.cursor/rules/`, `.github/copilot-instructions.md`
- Project docs: `README*`, `docs/`, architecture notes, contributing guides
- Package and tool config: `pyproject.toml`, `package.json`, `bun.lock`, `uv.lock`, `Makefile`, `tox.ini`, `pytest.ini`, `vite.config.*`, `tsconfig.json`, `ruff.toml`, `.pre-commit-config.yaml`
- Runtime entry points: CLI files, server files, app roots, framework routers, worker scripts
- Test layout: `tests/`, `test/`, `spec/`, CI workflows, custom test scripts
- Deployment/config: `env.example`, compose files, Kubernetes manifests, setup scripts

If the repo is large, use broad search and file discovery first, then read representative files. Do not infer architecture from filenames alone.

### 2. Build The Project Map

Identify:

- What the project does in one short paragraph
- Main languages, frameworks, package managers, and runtime targets
- Top-level directories and what each owns
- Core modules/classes/services and their responsibilities
- Data flow or request flow through the system
- Storage, persistence, external services, and configuration boundaries
- Generated files or directories that should not be hand-edited

Capture only information that helps future coding work. Avoid marketing copy.

### 3. Find The Working Contracts

Look for rules that prevent bugs:

- Required initialization or teardown steps
- Concurrency, locking, migration, caching, or transaction invariants
- Public APIs that must remain stable
- File layout conventions for adding new modules, tests, routes, providers, or backends
- Environment variables and config precedence
- Known footguns and their symptoms
- Mocking rules for external services
- Integration-test gates and required credentials

These contracts are the most valuable part of the guide. Explain the "must do" and "must not do" clearly.

### 4. Verify Commands

Extract commands from repo-owned sources where possible. Prefer commands from scripts, package files, Makefiles, CI, or existing docs.

For each command section, include only commands the repo appears to support:

- Setup/install
- Local development server
- Build/package
- Test suite and focused tests
- Lint/typecheck/format
- Database migrations or generated-code steps, if applicable

If a command is plausible but unverified, either verify it or mark it as needing confirmation. Do not invent package manager commands.

### 5. Draft The Guide

Use this structure unless the repo suggests a better one:

```markdown
# Repository Guidelines

## Project Overview
[What the project is and the core workflow it implements.]

## Project Structure
[Top-level directories.]

### Module Layout (`primary_package_or_app/`)
[Important modules and what they own.]

## Core Architecture
[Key composition, request/data flow, storage, background jobs, or integration boundaries.]

### [Important Contract Name]
[Concrete invariants, rules, lifecycle notes, or tables.]

## Development Commands
[Setup, run, build, test, lint.]

## Testing
[Test runner, layout, markers, mocks, integration gates, where to add tests.]

## Key Implementation Patterns
[Common API usage, initialization requirements, extension patterns, pitfalls.]

## Configuration
[Env files, generated config, secrets, deployment-specific rules.]

## Code Style
[Language/framework conventions actually used in the repo.]

## Commit and Pull Request Guidance
[Repo-specific PR targets, description requirements, checks.]
```

Add or remove sections based on evidence. Keep the document navigable with short headings.

### 6. Make It Agent-Usable

The final guide should:

- Prefer concrete names over abstractions: `src/api/routes/` is better than "the routes folder"
- Say where new code and tests should go
- Include exact commands in fenced code blocks
- Include critical pitfalls near the feature they affect
- Describe generated outputs and files that should not be edited manually
- State when to use mocks and when integration tests are allowed
- Avoid stale details such as temporary dates, personal paths, or machine-specific state
- Avoid broad advice that applies to every repo

### 7. Bilingual Version

If asked for both English and Chinese versions:

- Treat English as the source of truth unless the user says otherwise
- Keep headings and structure synchronized
- Translate technical meaning, not word-for-word phrasing
- Preserve commands, paths, identifiers, environment variables, and code symbols exactly
- Add a final note in the translated file that it is maintained in sync with the source version

## Quality Checklist

Before finishing, verify:

- The guide reflects inspected files, not assumptions
- Commands came from repo evidence or were explicitly verified
- Every major top-level directory is either described or intentionally omitted
- Architecture sections explain real invariants, not just component names
- Testing instructions tell future agents where to add tests
- Configuration instructions distinguish source files from generated output
- No secrets, local credentials, or personal machine paths are included
- The document is specific enough that a new agent can make a small change safely

## Style

Use concise, direct prose. The tone should be senior-engineer practical: enough context to avoid mistakes, no tutorial filler. Tables are useful for contracts and operation matrices; bullets are useful for directories and commands.
