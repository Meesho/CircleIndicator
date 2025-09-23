# Sample Application

<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [circleindicator/src/main/java/me/relex/circleindicator/SnackbarBehavior.java](circleindicator/src/main/java/me/relex/circleindicator/SnackbarBehavior.java)
- [sample/build.gradle](sample/build.gradle)
- [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java](sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java)

</details>



## Purpose and Scope

The sample application demonstrates various usage patterns and configurations of the CircleIndicator library through a collection of interactive examples. This application serves as both a testing platform for developers and a practical reference implementation for integrating CircleIndicator into Android projects.

This document covers the sample application's architecture, navigation system, and example implementations. For detailed information about specific CircleIndicator configurations shown in the examples, see [Configuration and Customization](#2.2). For information about the core library functionality, see [CircleIndicator Library](#2).

## Application Architecture

The sample application follows a single-activity architecture with fragment-based navigation, providing a clean demonstration platform for various CircleIndicator implementations.

### Core Components Structure

```mermaid
graph TB
    subgraph "Sample Application Module"
        SA["SampleActivity<br/>(AppCompatActivity)"]
        SLF["SampleListFragment<br/>(Fragment)"]
        SLA["SampleListAdapter<br/>(RecyclerView.Adapter)"]
        IVH["ItemViewHolder<br/>(RecyclerView.ViewHolder)"]
        SI["SampleInfo<br/>(Data Class)"]
    end
    
    subgraph "Example Fragments"
        DF["DefaultFragment"]
        CAF["CustomAnimationFragment"] 
        CCF["ChangeColorFragment"]
        DAF["DynamicAdapterFragment"]
        RAF["ResetAdapterFragment"]
        LVF["LoopViewPagerFragment"]
        SBF["SnackbarBehaviorFragment"]
    end
    
    subgraph "Dependencies"
        CI["circleindicator module"]
        LVP["LoopingViewPager module"]
        MD["Material Design Components"]
    end
    
    SA --> SLF
    SLF --> SLA
    SLA --> IVH
    SLA --> SI
    SA --> DF
    SA --> CAF
    SA --> CCF
    SA --> DAF
    SA --> RAF
    SA --> LVF
    SA --> SBF
    
    DF --> CI
    CAF --> CI
    CCF --> CI
    DAF --> CI
    RAF --> CI
    LVF --> LVP
    SBF --> CI
    SBF --> MD
```

Sources: [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java:1-158](), [sample/build.gradle:26-32]()

### Fragment Management System

The application uses a fragment-based navigation system with back stack management for seamless navigation between examples.

```mermaid
sequenceDiagram
    participant User
    participant SampleActivity
    participant FragmentManager
    participant SampleListFragment
    participant ExampleFragment
    
    User->>SampleActivity: onCreate()
    SampleActivity->>FragmentManager: beginTransaction()
    SampleActivity->>SampleListFragment: instantiate()
    FragmentManager->>SampleListFragment: replace(R.id.fragment_container)
    SampleActivity->>FragmentManager: addOnBackStackChangedListener()
    
    User->>SampleListFragment: click example item
    SampleListFragment->>ExampleFragment: Fragment.instantiate()
    SampleListFragment->>FragmentManager: setCustomAnimations()
    SampleListFragment->>FragmentManager: replace() + addToBackStack()
    
    User->>SampleActivity: onBackPressed()
    SampleActivity->>FragmentManager: popBackStack()
    FragmentManager->>SampleListFragment: return to list
```

Sources: [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java:29-52](), [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java:123-131]()

## Navigation Implementation

### Main Activity Setup

The `SampleActivity` class manages the overall application lifecycle and fragment navigation system. It initializes with a toolbar and sets up the primary list fragment.

Key implementation details:
- Fragment container management via `R.id.fragment_container`
- Back stack listener for navigation state updates
- Toolbar configuration with navigation support

```mermaid
graph LR
    subgraph "SampleActivity Methods"
        OC["onCreate()"]
        IT["initToolbar()"]
        BSL["OnBackStackChangedListener"]
    end
    
    subgraph "UI Components"
        TB["Toolbar"]
        FC["Fragment Container"]
        AB["ActionBar"]
    end
    
    subgraph "Fragment System"
        FM["FragmentManager"]
        FT["FragmentTransaction"]
        BS["BackStack"]
    end
    
    OC --> IT
    OC --> FM
    IT --> TB
    IT --> AB
    FM --> FT
    FT --> FC
    BSL --> BS
    BSL --> AB
```

Sources: [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java:29-62]()

### Sample List Implementation

The `SampleListFragment` contains a `RecyclerView` that displays available examples using a custom adapter pattern.

| Example Type | Fragment Class | Purpose |
|--------------|----------------|---------|
| Default | `DefaultFragment` | Basic CircleIndicator integration |
| Custom Animation | `CustomAnimationFragment` | Animation customization examples |
| Change Color | `ChangeColorFragment` | Color styling demonstrations |
| Dynamic Adapter | `DynamicAdapterFragment` | Runtime content management |
| Reset Adapter | `ResetAdapterFragment` | Adapter reset functionality |
| LoopViewPager | `LoopViewPagerFragment` | Infinite scrolling integration |
| Snackbar Behavior | `SnackbarBehaviorFragment` | Material Design coordination |

Sources: [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java:79-87]()

## RecyclerView Architecture

### Adapter Implementation

The `SampleListAdapter` manages the list of available examples using the ViewHolder pattern for efficient view recycling.

```mermaid
graph TB
    subgraph "SampleListAdapter"
        SLA["SampleListAdapter<br/>(RecyclerView.Adapter)"]
        ML["mList<br/>(List&lt;SampleInfo&gt;)"]
        OCVH["onCreateViewHolder()"]
        OBVH["onBindViewHolder()"]
        GIC["getItemCount()"]
        ADD["add()"]
    end
    
    subgraph "ItemViewHolder"
        IVH["ItemViewHolder<br/>(RecyclerView.ViewHolder)"]
        BV["bindView()"]
        CREATE["create()"]
        IV["itemView<br/>(TextView)"]
    end
    
    subgraph "Data Model"
        SI["SampleInfo"]
        TITLE["title<br/>(String)"]
        FN["fragmentName<br/>(String)"]
    end
    
    SLA --> ML
    SLA --> OCVH
    SLA --> OBVH
    SLA --> GIC
    SLA --> ADD
    
    OCVH --> IVH
    OBVH --> IVH
    IVH --> BV
    IVH --> CREATE
    IVH --> IV
    
    ML --> SI
    SI --> TITLE
    SI --> FN
    
    OBVH --> TITLE
    BV --> IV
```

Sources: [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java:90-156]()

### Navigation Flow

The navigation system uses fragment instantiation by class name for dynamic fragment creation and management.

```mermaid
flowchart TD
    START["User clicks item"]
    GET_POS["holder.getAdapterPosition()"]
    GET_INFO["mList.get(position)"]
    GET_NAME["sampleInfo.fragmentName"]
    INSTANTIATE["Fragment.instantiate(context, fragmentName)"]
    BEGIN_TX["getFragmentManager().beginTransaction()"]
    SET_ANIM["setCustomAnimations()"]
    REPLACE["replace(R.id.fragment_container, fragment)"]
    ADD_BACK["addToBackStack(fragmentName)"]
    COMMIT["commit()"]
    
    START --> GET_POS
    GET_POS --> GET_INFO
    GET_INFO --> GET_NAME
    GET_NAME --> INSTANTIATE
    INSTANTIATE --> BEGIN_TX
    BEGIN_TX --> SET_ANIM
    SET_ANIM --> REPLACE
    REPLACE --> ADD_BACK
    ADD_BACK --> COMMIT
```

Sources: [sample/src/main/java/me/relex/circleindicator/sample/SampleActivity.java:101-131]()

## Build Configuration

### Module Dependencies

The sample application module depends on both library modules and Material Design components for comprehensive demonstration capabilities.

```mermaid
graph LR
    subgraph "Sample Module Dependencies"
        SA["sample module"]
        CI["circleindicator module"]
        LVP["LoopingViewPager module"]
        MAT["Material Design 1.2.1"]
        AC["AppCompat 1.0.0"]
        RV["RecyclerView 1.0.0"]
    end
    
    SA --> CI
    SA --> LVP
    SA --> MAT
    SA --> AC
    SA --> RV
```

### Build Configuration Details

| Configuration | Value | Purpose |
|---------------|-------|---------|
| `applicationId` | `me.relex.circleindicator.sample` | Unique app identifier |
| `compileSdkVersion` | 28 | Build target SDK |
| `minSdkVersion` | 14 | Minimum supported Android |
| `targetSdkVersion` | 28 | Optimized for Android 9.0 |
| `versionCode` | 1 | Internal version number |
| `versionName` | "1.0" | User-visible version |

Sources: [sample/build.gradle:3-32]()

### Lint Configuration

The build configuration includes lint options to prevent build failures during development while maintaining code quality standards.

- `abortOnError: false` - Allows builds to continue despite lint warnings
- Standard ProGuard configuration for release builds
- Debug builds with minification disabled for development

Sources: [sample/build.gradle:14-24]()

## Material Design Integration

The sample application demonstrates Material Design integration through the `SnackbarBehavior` example, showcasing how CircleIndicator coordinates with Material Design components in a `CoordinatorLayout`.

### SnackbarBehavior Implementation

The `SnackbarBehavior` class extends `CoordinatorLayout.Behavior<CircleIndicator>` to provide automatic positioning adjustments when Snackbar components are displayed.

```mermaid
graph TB
    subgraph "SnackbarBehavior"
        SB["SnackbarBehavior<br/>(CoordinatorLayout.Behavior)"]
        LDO["layoutDependsOn()"]
        ODVC["onDependentViewChanged()"]
        GTYS["getTranslationYForSnackbar()"]
    end
    
    subgraph "CoordinatorLayout System"
        CL["CoordinatorLayout"]
        CI["CircleIndicator"]
        SBL["Snackbar.SnackbarLayout"]
        DEP["getDependencies()"]
    end
    
    subgraph "View Transformation"
        TY["setTranslationY()"]
        VO["ViewCompat.getTranslationY()"]
        OVL["doViewsOverlap()"]
    end
    
    SB --> LDO
    SB --> ODVC
    SB --> GTYS
    
    LDO --> SBL
    ODVC --> TY
    GTYS --> DEP
    GTYS --> VO
    GTYS --> OVL
    
    CL --> CI
    CL --> SBL
    CL --> DEP
```

Sources: [circleindicator/src/main/java/me/relex/circleindicator/SnackbarBehavior.java:1-45]()
