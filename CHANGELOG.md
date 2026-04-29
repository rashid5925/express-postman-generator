# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.2] - 2026-04-29

### Fixed
- **Critical: Incorrect base path generation from filenames** - Routes were being named after their source filename (e.g. `/api/users`) instead of their actual mount path (e.g. `/api/users-routes`). The parser now correctly reads `app.use()` mount points from entry files and uses those as the authoritative base path.
- Router mount paths defined in entry files are now resolved before route files are parsed, ensuring all routes reflect their true mounted paths.
- Filename-based path heuristic is now a last resort fallback only, used when no `app.use()` mount point is found for a file.

### Added
- `scanMountPoints(filePath)` method on `RouteParser` — performs a dedicated first-pass scan of an entry file to build a router-to-mount-path map (`routerMountMap`) before any route parsing begins.
- `routerMountMap` internal cache on `RouteParser` that maps resolved router file paths to their mount paths, persisting across all `parseFile()` calls.
- Generator now automatically detects and scans for mount points prior to the main parse loop.
- Support for resolving mount paths through middleware chains — `app.use('/path', mw1, mw2, router)` correctly identifies the router even when preceded by middleware arguments.

### Changed
- `parseFile()` now resolves base path in priority order: (1) `routerMountMap` entry for the file, (2) caller-supplied `inferredBasePath`, (3) filename heuristic. Previously only (2) and (3) were considered.

## [1.0.1] - 2025-11-06

### Changed
- Updated installation commands in README.md and QUICKSTART.md to use scoped package name `@rashid5925/express-postman-generator`

## [1.0.0] - 2025-11-06

### Added
- Initial release of Express Postman Generator
- AST-based route parsing using Babel parser
- Support for all HTTP methods (GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS)
- **Controller function resolution** - Follows `require()` imports to parse controller files
- **Separate controller file support** - Extracts parameters from handlers in different files
- Automatic detection of:
  - Path parameters (`:id`, `:userId`, etc.)
  - Query parameters from `req.query` usage (including destructuring)
  - Body parameters from `req.body` usage (including destructuring)
  - Form-data detection from `req.file` and `req.files`
  - Controller methods accessed as `controller.methodName`
  - Both inline handlers and external controller functions
- Destructuring support for parameter extraction
- Route grouping by base path
- CLI tool with multiple options
- Configuration file support (`.postmanrc.json`)
- Postman Collection v2.1 format output
- Base path inference from file structure
- Examples directory with sample Express app including:
  - Inline route handlers
  - Separate controller files
  - Service layer integration
  - Multiple parameter types

### Features
- Parse Express route files automatically
- Generate ready-to-import Postman collections
- Support for JSON and multipart/form-data body types
- Configurable output path and collection metadata
- File pattern inclusion/exclusion support
- Environment variable support with `{{baseUrl}}`
- DELETE requests with body support (when body params detected)
