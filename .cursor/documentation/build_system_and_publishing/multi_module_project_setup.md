# Multi-Module Project Setup

<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [.idea/gradle.xml](.idea/gradle.xml)
- [build.gradle](build.gradle)

</details>



This document covers the root-level Gradle build configuration and multi-module project structure for the CircleIndicator repository. It explains how the three main modules (`circleindicator`, `sample`, and `LoopingViewPager`) are organized and configured at the project level.

For detailed library build configuration, see [Library Build Configuration](#3.1). For module-specific dependencies, see [Module Dependencies](#5.1).

## Multi-Module Architecture Overview

The CircleIndicator project is structured as a multi-module Gradle project with a root configuration that applies common settings across all modules.

### Project Module Structure

```mermaid
graph TD
    subgraph "CircleIndicator Project Root"
        RBG["build.gradle<br/>(Root Configuration)"]
        SW["settings.gradle<br/>(Module Declaration)"]
        GP["gradle.properties<br/>(Global Properties)"]
    end
    
    subgraph "Library Modules"
        CI["circleindicator/<br/>Main Library Module"]
        LVP["LoopingViewPager/<br/>Supporting Library Module"]
    end
    
    subgraph "Application Module"
        SM["sample/<br/>Demo Application Module"]
    end
    
    subgraph "Build Plugins Applied"
        AF["com.jfrog.artifactory<br/>Publishing Plugin"]
        MP["maven-publish<br/>Maven Publication"]
    end
    
    RBG --> CI
    RBG --> LVP
    RBG --> SM
    
    RBG --> AF
    RBG --> MP
    
    SM -.-> CI
    SM -.-> LVP
    
    style RBG fill:#f9f9f9,stroke:#333,stroke-width:2px
    style CI fill:#e1f5fe,stroke:#0277bd,stroke-width:2px
    style SM fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
```

Sources: [build.gradle:1-24](), [.idea/gradle.xml:11-18]()

## Root Build Configuration

The root `build.gradle` file establishes the foundation for all modules in the project by configuring shared dependencies, repositories, and plugins.

### Build Script Configuration

The buildscript block defines the tools needed for building the project:

| Component | Purpose | Version |
|-----------|---------|---------|
| Android Gradle Plugin | Android module compilation | 3.4.2 |
| JFrog Build Info Extractor | Artifactory publishing | 4.28.3 |

The configuration includes repositories and classpath dependencies that are applied during the build process.

Sources: [build.gradle:3-14]()

### All Projects Configuration

The `allprojects` block applies common settings to every module in the project:

```mermaid
graph LR
    subgraph "Root Project Scope"
        AP["allprojects block"]
    end
    
    subgraph "Applied Plugins"
        AFP["com.jfrog.artifactory"]
        MPP["maven-publish"]
    end
    
    subgraph "Repository Configuration"
        GR["google()"]
        JR["jcenter()"]
    end
    
    subgraph "Target Modules"
        ROOT["root"]
        CI["circleindicator"]
        LVP["LoopingViewPager"] 
        SM["sample"]
    end
    
    AP --> AFP
    AP --> MPP
    AP --> GR
    AP --> JR
    
    AP --> ROOT
    AP --> CI
    AP --> LVP
    AP --> SM
```

This configuration ensures that:
- All modules can resolve dependencies from Google and JCenter repositories
- All modules have access to Artifactory publishing capabilities
- All modules can generate Maven publications

Sources: [build.gradle:16-23]()

## Module Structure and Relationships

### IDE Module Configuration

The IntelliJ IDEA configuration defines how the modules are recognized and managed within the development environment:

```mermaid
graph TB
    subgraph "IntelliJ IDEA Project Structure"
        PD["PROJECT_DIR<br/>(Root Directory)"]
    end
    
    subgraph "Gradle Module Set"
        ROOT["$PROJECT_DIR$<br/>(Root Module)"]
        CI_MOD["$PROJECT_DIR$/circleindicator<br/>(Library Module)"]
        LVP_MOD["$PROJECT_DIR$/LoopingViewPager<br/>(Library Module)"]
        SM_MOD["$PROJECT_DIR$/sample<br/>(Application Module)"]
    end
    
    subgraph "Gradle Configuration"
        TR["testRunner: GRADLE"]
        DT["distributionType: DEFAULT_WRAPPED"]
        GJ["gradleJvm: Embedded JDK"]
    end
    
    PD --> ROOT
    PD --> CI_MOD
    PD --> LVP_MOD
    PD --> SM_MOD
    
    ROOT --> TR
    ROOT --> DT
    ROOT --> GJ
```

Sources: [.idea/gradle.xml:6-20]()

### Module Hierarchy

The project follows a standard multi-module Android project structure:

| Module | Type | Purpose | Build Output |
|--------|------|---------|--------------|
| `root` | Project Root | Configuration container | N/A |
| `circleindicator` | Android Library | Main CircleIndicator component | `.aar` file |
| `LoopingViewPager` | Android Library | Infinite scrolling ViewPager | `.aar` file |
| `sample` | Android Application | Demo and testing application | `.apk` file |

## Build System Integration

### Publishing Pipeline Configuration

The root configuration establishes a unified publishing pipeline for all library modules:

```mermaid
flowchart TD
    subgraph "Root Build Configuration"
        RBC["build.gradle<br/>Root Configuration"]
        APC["allprojects<br/>Common Settings"]
    end
    
    subgraph "Plugin Application"
        AFP["com.jfrog.artifactory<br/>Applied to All Modules"]
        MPP["maven-publish<br/>Applied to All Modules"]
    end
    
    subgraph "Repository Configuration"
        GRP["google()<br/>Dependency Resolution"]
        JRP["jcenter()<br/>Dependency Resolution"]
    end
    
    subgraph "Target Modules"
        CIM["circleindicator module<br/>Library Publishing"]
        LVM["LoopingViewPager module<br/>Library Publishing"]
        SAM["sample module<br/>Demo Application"]
    end
    
    RBC --> APC
    APC --> AFP
    APC --> MPP
    APC --> GRP
    APC --> JRP
    
    AFP --> CIM
    AFP --> LVM
    MPP --> CIM
    MPP --> LVM
    
    GRP --> CIM
    GRP --> LVM
    GRP --> SAM
    JRP --> CIM
    JRP --> LVM
    JRP --> SAM
```

This configuration enables:
- Consistent artifact publishing across library modules
- Unified dependency resolution strategy
- Centralized plugin management
- Standardized build environment

Sources: [build.gradle:16-23]()

### Development Environment Setup

The project uses Gradle Wrapper with default distribution settings, ensuring consistent build environments across different development machines. The IDE configuration specifies the use of an embedded JDK and Gradle as the test runner.

Sources: [.idea/gradle.xml:7-10]()
