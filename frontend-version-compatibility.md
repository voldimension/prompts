Create a comprehensive frontend dependencies management system to ensure all React TypeScript packages are compatible and prevent version conflicts during development and deployment.

MAIN OBJECTIVE:
Establish a bulletproof system for managing frontend dependencies that prevents version conflicts, ensures team consistency, and maintains compatibility across React 18, TypeScript 5, Ant Design 5, and all testing libraries.

REQUIREMENTS:

1. LOCKED DEPENDENCY CONFIGURATION:
Create package.json with exact, tested, compatible versions for:
- React 18.2.0 ecosystem (React, React DOM, TypeScript types)
- Ant Design 5.12.x with matching icons
- TypeScript 5.3.x with proper React types
- Testing stack: Jest 29.x + React Testing Library 14.x + MSW 2.x
- Build tools: Vite/CRA with React 18 compatibility
- Additional libraries: React Query, Recharts, Tailwind CSS, Zod
- Development tools: ESLint, Prettier, Husky for React 18

2. VERSION COMPATIBILITY MATRIX:
Create documentation showing:
- Which versions work together
- Known incompatible combinations
- Peer dependency requirements
- Node.js version requirements
- Package manager version requirements

3. AUTOMATED VERSION VALIDATION:
Create validation script that checks:
- React and React DOM versions match exactly
- TypeScript version compatibility with React types
- Ant Design and icons version alignment
- Testing library compatibility with React version
- No conflicting peer dependencies
- Bundle builds successfully with all packages

4. LOCK FILE MANAGEMENT:
Set up configuration for:
- package-lock.json or yarn.lock strict usage
- Exact version installation by default
- Peer dependency resolution
- Security audit integration
- Automated lock file updates

5. CI/CD VERSION CHECKING:
Create GitHub Actions workflow that:
- Validates package.json changes
- Checks for version conflicts
- Runs compatibility tests
- Prevents incompatible version merges
- Audits for security vulnerabilities
- Verifies bundle builds with new versions

6. DEVELOPMENT ENVIRONMENT STANDARDS:
Ensure consistency with:
- .nvmrc file for Node.js version
- .npmrc configuration for exact versions
- Engine requirements in package.json
- Docker environment with locked versions
- IDE configuration for version warnings

7. TEAM DEVELOPMENT GUIDELINES:
Create standards for:
- How to safely upgrade dependencies
- Required checks before adding new packages
- Peer dependency warning resolution
- Version conflict troubleshooting
- Emergency rollback procedures

8. AUTOMATED DEPENDENCY MANAGEMENT:
Set up tools for:
- Daily dependency security scanning
- Weekly compatibility checking
- Monthly safe upgrade recommendations
- Automated patch version updates
- Breaking change detection

9. ERROR PREVENTION SYSTEM:
Implement safeguards for:
- Pre-commit hooks that check versions
- Development server startup validation
- Build-time compatibility verification
- Runtime error detection for version issues
- Clear error messages for version conflicts

10. TESTING INTEGRATION:
Ensure version compatibility with:
- Unit test framework compatibility
- Integration test mock library versions
- E2E testing tool compatibility
- Performance testing tool versions
- Visual regression testing tools

SPECIFIC IMPLEMENTATION REQUIREMENTS:

1. Package.json Configuration:
- Exact versions for all critical dependencies
- Proper peer dependency declarations
- Engine requirements specification
- Scripts for version checking
- Resolutions for known conflicts

2. Validation Scripts:
- validate-versions.js for compatibility checking
- check-peer-deps.js for peer dependency validation
- security-audit.js for vulnerability scanning
- bundle-analysis.js for build verification

3. CI/CD Integration:
- version-check.yml workflow for GitHub Actions
- Pre-commit hooks with version validation
- Automated testing with locked versions
- Deployment verification with version checks

4. Docker Configuration:
- Multi-stage build with exact Node.js version
- Package installation from lock files only
- Version verification during build process
- Production environment version consistency

5. Development Tools:
- ESLint rules for version compatibility
- TypeScript configuration for strict checking
- IDE extensions for dependency warnings
- Development server with version validation

6. Documentation:
- Version compatibility matrix
- Upgrade procedures and guidelines
- Troubleshooting common version conflicts
- Emergency rollback procedures

7. Monitoring and Alerts:
- Automated dependency vulnerability alerts
- Version drift detection
- Compatibility regression monitoring
- Team notification for version issues

SUCCESS CRITERIA:
✅ Zero version conflicts during development
✅ Consistent dependency versions across all environments
✅ Automated detection of incompatible packages
✅ Fast resolution of version-related issues
✅ Team-wide adherence to version standards
✅ Secure and up-to-date dependency management
✅ Reliable builds across all environments
✅ Clear upgrade and rollback procedures

EXPECTED OUTPUTS:
- Complete package.json with locked compatible versions
- Validation scripts for version checking
- CI/CD workflows for automated verification
- Development environment configuration
- Team guidelines and documentation
- Error prevention and recovery procedures
- Monitoring and alerting system

The goal is to create a robust, automated system that prevents version conflicts before they occur and ensures consistent, compatible frontend dependencies across all development stages and team members.