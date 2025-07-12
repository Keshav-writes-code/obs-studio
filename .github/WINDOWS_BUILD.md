# Windows Build Guide for OBS Studio

This repository provides comprehensive Windows build support through GitHub Actions.

## Build Options

### 1. Main Build Workflow (build-project.yaml)
The main build workflow builds OBS Studio for all platforms including Windows. This runs automatically on:
- Push to master or release branches
- Pull requests
- Tagged releases
- Scheduled runs

### 2. Dedicated Windows Build Workflow (build-windows.yaml)
A standalone Windows-only build workflow for focused testing and development. This provides:

**Automatic Triggers:**
- Pull requests that modify Windows-related files
- Pushes to master or release branches that affect Windows components

**Manual Triggers:**
- Workflow dispatch with customizable options:
  - **Build Configuration**: Debug, RelWithDebInfo, Release
  - **Architecture**: x64, arm64, or both
  - **Create Package**: Enable/disable package creation

## Using the Windows Build Workflow

### Manual Build Execution

1. Go to the "Actions" tab in the GitHub repository
2. Select "Build Windows" from the workflow list
3. Click "Run workflow"
4. Configure options:
   - **Branch**: Select the branch to build from
   - **Build configuration**: Choose Debug, RelWithDebInfo, or Release
   - **Target architecture**: Choose x64, arm64, or both
   - **Create package**: Check to create installable packages

### Build Artifacts

Successful builds produce:
- **obs-studio-windows-{arch}-{commit}.zip**: Complete build package
- **obs-studio-windows-{arch}-{commit}-pdbs.zip**: Debug symbols (Release builds only)

Artifacts are available for 30 days after the build completes.

## Build Requirements

The Windows build uses:
- **OS**: Windows Server 2022
- **Generator**: Visual Studio 17 2022
- **Architectures**: x64 and ARM64
- **Dependencies**: Automatically downloaded from obs-deps releases

## Build Scripts

The Windows build process uses:
- `.github/scripts/Build-Windows.ps1`: Main build script
- `.github/scripts/Package-Windows.ps1`: Packaging script
- `.github/actions/build-obs/`: Reusable build action
- `.github/actions/package-obs/`: Reusable packaging action

## Environment Variables

The build can use these optional environment variables:
- `TWITCH_CLIENTID` / `TWITCH_HASH`: Twitch integration
- `RESTREAM_CLIENTID` / `RESTREAM_HASH`: Restream integration  
- `YOUTUBE_CLIENTID` / `YOUTUBE_SECRET`: YouTube integration
- `GPU_PRIORITY_VAL`: GPU priority configuration

## CMake Configuration

Windows builds use predefined CMake presets:
- `windows-ci-x64`: x64 CI configuration
- `windows-ci-arm64`: ARM64 CI configuration

See `CMakePresets.json` for complete configuration details.

## Troubleshooting

### Build Failures
1. Check the build logs in the GitHub Actions run
2. Verify dependencies are properly downloaded
3. Check for Windows-specific compilation errors

### Missing Artifacts
- Artifacts are only created if the build completes successfully
- Debug symbols are only included for Release builds
- Verify the "Create package" option is enabled for manual runs

### Performance Issues
- ARM64 builds may take longer due to cross-compilation
- Consider building only the required architecture for faster iteration

## Local Development

For local Windows development:
1. Install Visual Studio 2022 with C++ development tools
2. Install PowerShell 7.2+
3. Use the build scripts: `.github/scripts/Build-Windows.ps1`

Example local build:
```powershell
.github/scripts/Build-Windows.ps1 -Target x64 -Configuration RelWithDebInfo
```