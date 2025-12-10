# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Created `.env.example` file to provide a clear template for environment configuration.
- Populated the `docs/` user manual with initial guides for Installation, Configuration, Administration, and Troubleshooting.
- Formally documented the project's monorepo and GitHub Pages hosting strategy in `ARCHITECTURE.md`.

### Changed
- **`project_plan.md`**:
    - Updated the Docker Compose blueprint to use the `${MUSIC_PATH}` environment variable.
    - Added a detailed `Phase 0` to the roadmap, marking the documentation scaffolding as complete.
    - Refined the `Phase 1` roadmap with a specific SQL schema, clearer MVP requirements for the frontend, and a new API testing task.
- **`README.md`**:
    - Added an MIT License badge.
    - Included a new "Project Structure" section for quick developer orientation.
    - Converted all documentation links to clickable Markdown links.
- **`ARCHITECTURE.md`**:
    - Corrected the numbering of all sections for better readability.

### Removed
- Nothing in this cycle.

## [0.1.0] - 2025-12-09

### Added
- Initial project structure and `docker-compose.yml` blueprint.
- MIT License for open-source distribution.
- Comprehensive project documentation suite, including:
  - `README.md`, `project_plan.md`, `ARCHITECTURE.md`, `API_DOCUMENTATION.md`, `CONTRIBUTING.md`, and this `CHANGELOG.md`.
  - A `docs/` directory for the User Manual, to be developed in parallel with the code.
- Commercial "Open Core" strategy and future feature planning (Video, Admin Dashboard, Theming) to `ARCHITECTURE.md`.

### Changed
- Refined the project's service-oriented architecture and synchronized all planning documents for consistency.
- Updated `CONTRIBUTING.md` to require testing and include guidance on updating the user manual.

### Removed
- Deleted redundant `project_plan2.md` to consolidate all planning into `project_plan.md`.