---
date: 2026-09-28
tags:
  - Versioning
  - SemVer
  - Git
  - Release
description: How to number versions (Semantic Versioning, CalVer, Conventional Commits), how it maps to Git tags, Git Flow and GitHub releases, and the personal rules used for the portfolio.
---
## 📋 **Table of Contents**

- [[#1. Why version?]]
- [[#2. Semantic Versioning (SemVer)]]
- [[#3. Conventional Commits → version bump]]
- [[#4. Other schemes (CalVer, ...)]]
- [[#5. Versions, tags and releases]]
- [[#6. Checklist before a release]]

---

## 1. Why version?

A version number is a **label on a frozen state of the code**. It tells users and other developers *what kind of change* to expect between two releases, and lets you go back to a known state (see [[GIT]]).

---

## 2. Semantic Versioning (SemVer)

The de facto industry standard (npm, Angular, Spring Boot, Cargo, ...). Spec: [semver.org](https://semver.org).

```text
MAJOR . MINOR . PATCH
  4   .   0   .   1
```

| Part      | Increment when...                                 | Example             |
| --------- | ------------------------------------------------- | ------------------- |
| **MAJOR** | **Incompatible** change (breaking the public API) | `3.4.2` → `4.0.0`   |
| **MINOR** | New feature, **backward-compatible**              | `3.4.2` → `3.5.0`   |
| **PATCH** | Bug fix, **backward-compatible**                  | `3.4.2` → `3.4.3`   |

### Core rules

1. Numbers are **non-negative integers**, no leading zeros (`1.02.0` ❌).
2. Increasing a part **resets the ones on its right** to `0`: `1.4.7` → `1.5.0` → `2.0.0`.
3. **A released version is immutable.** Any change, even a typo, means a **new** version.
4. `0.y.z` = **initial development**: anything may change at any time, nothing is considered stable.
5. `1.0.0` = the public API is **defined and stable**. From then on, the rules above apply strictly.
6. Marking a feature as **deprecated** requires at least a **MINOR** bump (removal later = MAJOR).
7. Comparison is **numeric, part by part**: `1.9.0 < 1.10.0 < 1.11.0` (not alphabetical).

### Pre-releases and build metadata

```text
1.0.0-alpha.1 < 1.0.0-alpha.2 < 1.0.0-beta.1 < 1.0.0-rc.1 < 1.0.0
1.0.0+20260928      ← build metadata: ignored when comparing versions
```

- A pre-release is **lower** than the final version.
- Usual order: `alpha` → `beta` → `rc` (release candidate).

### npm ranges (`package.json`)

| Range     | Meaning                                | Accepts                      |
| --------- | -------------------------------------- | ---------------------------- |
| `^1.2.3`  | Compatible: same MAJOR                 | `>=1.2.3 <2.0.0`             |
| `~1.2.3`  | Patch updates only                     | `>=1.2.3 <1.3.0`             |
| `1.2.3`   | Exact version                          | `1.2.3` only                 |
| `^0.2.3`  | Special case in `0.x`: same MINOR      | `>=0.2.3 <0.3.0`             |

> 💡 SemVer was designed for **libraries with a public API**. For an **application or website** (like a portfolio) there is no API to break, so MAJOR/MINOR/PATCH becomes a **convention** you define yourself (see [section 6](#6-personal-rules-portfolio)). The important part is to stay **consistent**.

---

## 3. Conventional Commits → version bump

[Conventional Commits](https://www.conventionalcommits.org) is a commit message convention that maps directly to SemVer.

```text
<type>(<scope>): <description>

feat(technord): create the component for technord
fix(styles): enhance text alignment in project card
feat(i18n)!: switch to native Angular i18n      ← "!" = breaking change
```

| Commit type                                  | Version bump |
| -------------------------------------------- | ------------ |
| `fix`                                        | **PATCH**    |
| `feat`                                       | **MINOR**    |
| `!` after the type, or `BREAKING CHANGE:` footer | **MAJOR** |
| `chore`, `docs`, `style`, `refactor`, `test`, `build`, `ci` | no bump (usually) |

Tools like `semantic-release` or `release-please` read these commits to compute the next version and generate the changelog automatically.

---

## 4. Other schemes (CalVer, ...)

| Scheme                 | Format              | Used by                               | Good for                       |
| ---------------------- | ------------------- | ------------------------------------- | ------------------------------ |
| **SemVer**             | `4.0.1`             | npm packages, Angular, Spring Boot    | Libraries, frameworks, APIs    |
| **CalVer**             | `2026.09` / `24.04` | Ubuntu (`24.04`), pip (`24.0`), Black | Time-driven releases, apps, OS |
| **Sequential**         | `v1`, `v2`, `v3`    | Internal tools, some APIs (`/v2/`)    | Simple products                |
| **Git hash / build #** | `a1e9484`, `#412`   | CI artifacts, Docker images           | Internal builds, nightly       |

Reference: [calver.org](https://calver.org).

> Angular publishes a **new MAJOR roughly every 6 months** (18 → 19 → ... → 22). A MAJOR there does not mean "everything breaks", it follows their fixed release schedule.

---

## 5. Versions, tags and releases

The version lives in **several places**. Keep them consistent.

| Where                     | Example            | Notes                                              |
| ------------------------- | ------------------ | -------------------------------------------------- |
| **Git tag**               | `v4.0.0`           | The reference. Prefix `v` by convention            |
| **GitHub Release**        | `Release v4.0.0`   | A tag + release notes (+ optional assets)          |
| `package.json` `version`  | `"version": "4.0.0"` | Optional for an app, mandatory for a published npm package |
| Changelog / release notes | `## v4.0.0`        | Human-readable summary                             |

### Git tags

```bash
# Create an annotated tag (recommended: stores author, date, message)
git tag -a v4.0.0 -m "Release v4.0.0"

# Lightweight tag (just a pointer, no metadata)
git tag v4.0.0

git tag                       # list tags
git show v4.0.0               # details of a tag
git push origin v4.0.0        # push one tag
git push origin --tags        # push all tags

# Delete a tag (local, then remote)
git tag -d v4.0.0
git push origin --delete v4.0.0
```

> ⚠️ A tag is **not** pushed by a normal `git push`. Push it explicitly.

## 6. Checklist before a release

- [ ] All features finished and merged into `develop`
- [ ] Build and tests pass (`ng build`, `ng test`)
- [ ] Version chosen with the table above
- [ ] README and `package.json` version up to date
- [ ] `Start Release` → last adjustments → `Finish Release` (tag created)
- [ ] `git push origin main develop --tags`
- [ ] GitHub Release created from the tag, with release notes

---

## 📚 Ressources

- [Semantic Versioning 2.0.0](https://semver.org)
- [Conventional Commits](https://www.conventionalcommits.org)
- [Calendar Versioning](https://calver.org)
- [Keep a Changelog](https://keepachangelog.com)
- [npm semver calculator](https://semver.npmjs.com)
- [GitHub Docs: Managing releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
