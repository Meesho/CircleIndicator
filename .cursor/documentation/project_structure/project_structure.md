# Project Structure

<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [.idea/gradle.xml](.idea/gradle.xml)
- [circleindicator/gradle.properties](circleindicator/gradle.properties)
- [circleindicator/proguard-rules.pro](circleindicator/proguard-rules.pro)
- [circleindicator/src/main/AndroidManifest.xml](circleindicator/src/main/AndroidManifest.xml)
- [circleindicator/src/main/res/drawable/white_radius.xml](circleindicator/src/main/res/drawable/white_radius.xml)
- [settings.gradle](settings.gradle)

</details>



This document provides an overview of the multi-module project organization, dependencies, and module relationships within the CircleIndicator repository. It covers the physical file structure, Gradle module configuration, and how the different components are organized to support library development, testing, and distribution.

For detailed information about module dependencies and inter-module relationships, see [Module Dependencies](#5.1). For build system configuration details, see [Build System and Publishing](#3).

## Multi-Module Architecture Overview

The CircleIndicator project follows a standard multi-module Android library structure with three primary modules coordinated through a root Gradle project.

### Module Structure Diagram

```mermaid
graph TB
    subgraph "Root Project"
        RP["CircleIndicator Root"]
        SG["settings.gradle"]
        RBG["build.gradle"]
    end
    
    subgraph "Library Modules"
        CI["circleindicator"]
        LVP["LoopingViewPager"]
    end
    
    subgraph "Application Module"
        SM["sample"]
    end
    
    subgraph "IDE Configuration"
        GX[".idea/gradle.xml"]
        MS[".idea/modules.xml"]
    end
    
    SG --> CI
    SG --> LVP  
    SG --> SM
    
    SM -.->|"depends on"| CI
    SM -.->|"depends on"| LVP
    
    GX --> CI
    GX --> LVP
    GX --> SM
    
    RP --> SG
    RP --> RBG
```

**Sources:** [settings.gradle:1](), [.idea/gradle.xml:11-18]()

### Module Definitions

The project structure is defined in the root `settings.gradle` file, which declares three modules:

| Module | Type | Purpose |
|--------|------|---------|
| `circleindicator` | Android Library | Main UI component library |
| `sample` | Android Application | Demo app and usage examples |
| `LoopingViewPager` | Android Library | Supporting ViewPager component |

**Sources:** [settings.gradle:1]()

## Directory Organization

The project follows standard Android multi-module conventions with each module containing its own source tree, resources, and build configuration.

### File System Structure

```mermaid
graph TD
    subgraph "CircleIndicator Root"
        ROOT["/"]
        
        subgraph "circleindicator module"
            CI_ROOT["circleindicator/"]
            CI_SRC["src/main/"]
            CI_JAVA["java/me/relex/circleindicator/"]
            CI_RES["res/"]
            CI_MANIFEST["AndroidManifest.xml"]
            CI_BUILD["build.gradle"]
            CI_PROPS["gradle.properties"]
            CI_PROGUARD["proguard-rules.pro"]
        end
        
        subgraph "sample module"
            S_ROOT["sample/"]
            S_SRC["src/main/"]
            S_JAVA["java/"]
            S_RES["res/"]
            S_BUILD["build.gradle"]
        end
        
        subgraph "LoopingViewPager module"
            LVP_ROOT["LoopingViewPager/"]
            LVP_SRC["src/main/"]
            LVP_BUILD["build.gradle"]
        end
        
        subgraph "Project Configuration"
            SETTINGS["settings.gradle"]
            ROOT_BUILD["build.gradle"]
            GRADLE_PROPS["gradle.properties"]
        end
    end
    
    ROOT --> CI_ROOT
    ROOT --> S_ROOT
    ROOT --> LVP_ROOT
    ROOT --> SETTINGS
    ROOT --> ROOT_BUILD
    
    CI_ROOT --> CI_SRC
    CI_ROOT --> CI_BUILD
    CI_ROOT --> CI_PROPS
    CI_ROOT --> CI_PROGUARD
    
    CI_SRC --> CI_JAVA
    CI_SRC --> CI_RES
    CI_SRC --> CI_MANIFEST
    
    S_ROOT --> S_SRC
    S_ROOT --> S_BUILD
    
    LVP_ROOT --> LVP_SRC
    LVP_ROOT --> LVP_BUILD
```

**Sources:** [circleindicator/src/main/AndroidManifest.xml:1](), [circleindicator/gradle.properties:1](), [circleindicator/proguard-rules.pro:1]()

## Module Configuration Details

### CircleIndicator Library Module

The main library module is configured with Maven publishing properties and follows Android library conventions:

```mermaid
graph LR
    subgraph "circleindicator Module"
        MANIFEST["AndroidManifest.xml<br/>package: me.relex.circleindicator"]
        PROPS["gradle.properties<br/>POM_NAME: CircleIndicator<br/>POM_ARTIFACT_ID: circleindicator<br/>POM_PACKAGING: aar"]
        PROGUARD["proguard-rules.pro<br/>Library obfuscation rules"]
        RESOURCES["res/<br/>drawable/white_radius.xml<br/>values/attrs.xml"]
    end
    
    MANIFEST --> PROPS
    PROPS --> PROGUARD
    PROGUARD --> RESOURCES
```

**Package Structure:** The library uses the package name `me.relex.circleindicator` as defined in its Android manifest.

**Publishing Configuration:** Maven artifact properties are defined for publishing to repositories:
- `POM_NAME`: CircleIndicator
- `POM_ARTIFACT_ID`: circleindicator  
- `POM_PACKAGING`: aar

**Sources:** [circleindicator/src/main/AndroidManifest.xml:2](), [circleindicator/gradle.properties:20-22](), [circleindicator/src/main/res/drawable/white_radius.xml:1]()

### IDE Integration

The project is configured for IntelliJ IDEA with Gradle integration:

```mermaid
graph TB
    subgraph "IntelliJ IDEA Configuration"
        GS["GradleSettings"]
        GPS["GradleProjectSettings"]
        MODULES["modules set"]
    end
    
    subgraph "Module Paths"
        ROOT_PATH["$PROJECT_DIR$"]
        CI_PATH["$PROJECT_DIR$/circleindicator"]  
        LVP_PATH["$PROJECT_DIR$/LoopingViewPager"]
        SAMPLE_PATH["$PROJECT_DIR$/sample"]
    end
    
    GS --> GPS
    GPS --> MODULES
    MODULES --> ROOT_PATH
    MODULES --> CI_PATH
    MODULES --> LVP_PATH
    MODULES --> MODULES_PATH["SAMPLE_PATH"]
```

The IDE configuration specifies:
- **Test Runner**: GRADLE
- **Distribution Type**: DEFAULT_WRAPPED
- **Gradle JVM**: Embedded JDK

**Sources:** [.idea/gradle.xml:6-19]()

## Build System Coordination

The multi-module structure enables:

1. **Independent Module Building**: Each module can be built separately with its own `build.gradle`
2. **Dependency Management**: The sample module can depend on both library modules
3. **Publishing Coordination**: The root project coordinates artifact publishing to repositories
4. **IDE Integration**: IntelliJ IDEA recognizes all modules for development workflow

The `settings.gradle` file serves as the central coordination point, defining which modules are included in the project build.

**Sources:** [settings.gradle:1](), [.idea/gradle.xml:1-22]()
