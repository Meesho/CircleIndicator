# Version Control Setup

<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [.gitignore](.gitignore)
- [.idea/encodings.xml](.idea/encodings.xml)
- [.idea/vcs.xml](.idea/vcs.xml)
- [circleindicator/.gitignore](circleindicator/.gitignore)

</details>



This document covers the Git version control configuration for the CircleIndicator project, including ignore patterns, IDE integration, and multi-module project considerations. It focuses on the practical setup required for developers to work effectively with the repository.

For information about IDE project configuration beyond version control, see [IDE Configuration](#6.1). For details about the overall project structure that affects version control organization, see [Project Structure](#5).

## Git Configuration Overview

The CircleIndicator project uses Git for version control with a multi-module Android project structure. The version control setup includes both project-level and module-specific configurations to handle Android build artifacts, IDE files, and development-specific content.

## Version Control Hierarchy

Git Version Control Structure:
```mermaid
graph TB
    subgraph "Root Project Directory"
        RGI[".gitignore<br/>Project-Level Exclusions"]
        IDEA[".idea/<br/>IDE Configuration"]
        VCS[".idea/vcs.xml<br/>VCS Mapping"]
        ENC[".idea/encodings.xml<br/>File Encoding"]
    end
    
    subgraph "circleindicator Module"
        CGI["circleindicator/.gitignore<br/>Module-Specific Exclusions"]
        CIML["circleindicator.iml<br/>IntelliJ Module File"]
        CBUILD["circleindicator/build/<br/>Build Output"]
    end
    
    subgraph "Other Modules"
        SAMPLE["sample/<br/>Sample Application"]
        LVP["LoopingViewPager/<br/>Supporting Library"]
    end
    
    RGI --> CGI
    RGI --> IDEA
    IDEA --> VCS
    IDEA --> ENC
    CGI --> CIML
    CGI --> CBUILD
    
    RGI --> SAMPLE
    RGI --> LVP
```

Sources: [.gitignore:1-8](), [circleindicator/.gitignore:1-3](), [.idea/vcs.xml:1-6]()

## Project-Level Git Ignore Patterns

The root `.gitignore` file excludes common Android development artifacts and IDE-generated content:

| Pattern | Purpose | Impact |
|---------|---------|--------|
| `*.iml` | IntelliJ module files | Prevents IDE-specific module configuration from being tracked |
| `.gradle` | Gradle cache directory | Excludes Gradle build cache and temporary files |
| `/local.properties` | Local Android SDK paths | Prevents machine-specific SDK configuration from being committed |
| `/.idea/workspace.xml` | IDE workspace state | Excludes user-specific IDE window/editor state |
| `/.idea/libraries` | IDE library definitions | Prevents auto-generated library configurations from being tracked |
| `/.idea/dictionaries` | IDE custom dictionaries | Excludes user-specific spell-check dictionaries |
| `.DS_Store` | macOS system files | Prevents macOS Finder metadata from being committed |
| `/build` | Root build directory | Excludes compiled artifacts and build outputs |

Sources: [.gitignore:1-8]()

## Module-Level Git Ignore Patterns

The `circleindicator` module has additional specific exclusions:

```mermaid
graph LR
    subgraph "circleindicator Module Exclusions"
        IML["/circleindicator.iml<br/>Module Definition File"]
        BUILD["/build<br/>Module Build Output"]
    end
    
    subgraph "Inherited from Root"
        GLOBAL["Root .gitignore<br/>Global Patterns"]
    end
    
    GLOBAL --> IML
    GLOBAL --> BUILD
    
    IML --> BUILD
```

The module-specific `.gitignore` targets:
- `/circleindicator.iml`: IntelliJ module file specific to the circleindicator library
- `/build`: Module-specific build directory containing compiled library artifacts

Sources: [circleindicator/.gitignore:1-3]()

## IntelliJ IDEA VCS Integration

### VCS Directory Mapping

The IntelliJ IDEA VCS configuration maps the project root to Git version control:

```xml
<component name="VcsDirectoryMappings">
  <mapping directory="$PROJECT_DIR$" vcs="Git" />
</component>
```

This configuration in `.idea/vcs.xml` establishes that:
- The entire project directory (`$PROJECT_DIR$`) uses Git as the version control system
- All modules inherit this Git configuration
- IDE VCS operations apply to the unified project structure

Sources: [.idea/vcs.xml:3-5]()

### File Encoding Configuration

The project enforces UTF-8 encoding across all files through IntelliJ IDEA configuration:

```xml
<component name="Encoding">
  <file url="PROJECT" charset="UTF-8" />
</component>
```

This ensures consistent character encoding for:
- Source code files (Java, XML)
- Configuration files
- Documentation and resources
- Version control commit messages

Sources: [.idea/encodings.xml:3-5]()

## Multi-Module Version Control Strategy

Version Control Module Organization:
```mermaid
graph TB
    subgraph "Git Repository Root"
        ROOT["CircleIndicator Repository"]
    end
    
    subgraph "Version Controlled Modules"
        CI["circleindicator/<br/>Library Source Code"]
        SAMPLE["sample/<br/>Demo Application"] 
        LVP["LoopingViewPager/<br/>Supporting Library"]
    end
    
    subgraph "Excluded Build Artifacts"
        CIBUILD["circleindicator/build/<br/>AAR Output"]
        SAMPLEBUILD["sample/build/<br/>APK Output"]
        LVPBUILD["LoopingViewPager/build/<br/>Library Output"]
    end
    
    subgraph "IDE Files"
        WORKSPACE[".idea/workspace.xml<br/>User-Specific State"]
        MODULES["*.iml<br/>Module Definitions"]
    end
    
    ROOT --> CI
    ROOT --> SAMPLE
    ROOT --> LVP
    
    CI -.-> CIBUILD
    SAMPLE -.-> SAMPLEBUILD  
    LVP -.-> LVPBUILD
    
    ROOT -.-> WORKSPACE
    ROOT -.-> MODULES
    
    style CIBUILD fill:#ffebee,stroke:#c62828,stroke-dasharray: 5 5
    style SAMPLEBUILD fill:#ffebee,stroke:#c62828,stroke-dasharray: 5 5
    style LVPBUILD fill:#ffebee,stroke:#c62828,stroke-dasharray: 5 5
    style WORKSPACE fill:#fff3e0,stroke:#ef6c00,stroke-dasharray: 5 5
    style MODULES fill:#fff3e0,stroke:#ef6c00,stroke-dasharray: 5 5
```

The multi-module strategy ensures:
- All source code modules are version controlled together
- Build artifacts are consistently excluded across modules
- IDE configuration is partially tracked (shared settings) while excluding user-specific state
- Module dependencies can reference each other through relative paths

Sources: [.gitignore:1-8](), [circleindicator/.gitignore:1-3](), [.idea/vcs.xml:1-6]()

## Development Workflow Considerations

The version control setup supports:

1. **Clean Repository State**: Build artifacts and IDE-specific files are excluded to maintain repository cleanliness
2. **Cross-Platform Development**: Platform-specific files (`.DS_Store`) and local configurations (`local.properties`) are ignored
3. **Team Collaboration**: Shared IDE settings are tracked while user-specific workspace state is excluded
4. **Automated Builds**: Build directories are ignored, allowing CI/CD systems to generate fresh artifacts
5. **Module Independence**: Each module can have additional ignore patterns while inheriting global exclusions

Sources: [.gitignore:1-8](), [circleindicator/.gitignore:1-3](), [.idea/vcs.xml:1-6](), [.idea/encodings.xml:1-5]()
