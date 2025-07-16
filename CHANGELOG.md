# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-07-16

### Added
- Initial release of HOTKU MailHub Mass Checker
- Microsoft email authentication support (Outlook, Hotmail, Live)
- Mass email verification functionality
- Modular architecture with separate libraries
- CLI interface for standalone usage
- NPM package for library integration
- Real-time progress monitoring
- Multiple result categories (Valid, Invalid, Custom, NFA, Retry)
- Automatic result saving to files
- Beautiful console interface with color coding
- Retry mechanism for failed requests
- Rate limiting to prevent blocking
- Configurable settings via app-config.js
- File handler utilities for combo parsing
- Display utilities for formatted output
- Professional error handling

### Technical Features
- Node.js 14+ support
- Axios for HTTP requests
- readline-sync for interactive CLI
- Modular folder structure:
  - `lib/` - Core authentication libraries
  - `src/` - Main application logic
  - `utils/` - Utility functions
  - `config/` - Configuration files
- Global CLI installation support
- Library integration capabilities

### Documentation
- Comprehensive README with usage examples
- API documentation for all modules
- Deployment guide for NPM and GitHub
- MIT License
- Security and ethics guidelines

### Authors
- **masanto** - Main author and maintainer
- **Not-ISellStuff** - Original developer and contributor

[1.0.0]: https://github.com/masanto/hotku-mailhub/releases/tag/v1.0.0
