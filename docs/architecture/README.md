# ShowMate Showcase — Architecture Documentation

This directory contains documentation about ShowMate's architecture and engineering decisions.

## Overview

ShowMate follows **Clean Architecture** with three clearly separated layers:

### Layer Responsibilities

| Layer | Module | Responsibility |
|:--|:--|:--|
| **Presentation** | `:app` | Jetpack Compose UI, ViewModels, Navigation, Design System |
| **Domain** | `:shared` (KMP) | Use Cases, Business Rules, Domain Models, Scoring Algorithms |
| **Data** | `:app` | Repository Implementations, Room DAOs, Ktor API Clients, Firebase Services |

### Key Design Decisions

#### 1. Domain as Pure Kotlin (KMP Module)
The domain layer has **zero Android dependencies**. This enables:
- Running all business logic tests on JVM without emulators
- Potential code sharing with iOS or Desktop targets
- Maximum testability with MockK

#### 2. Offline-First with Optimistic Updates
Every user action writes to Room first (with `syncPending = true`), then attempts cloud sync. If the network is unavailable, actions are queued in `SyncActionDao` and retried via WorkManager when connectivity returns.

#### 3. Encrypted Database as Single Source of Truth
Room with SQLCipher serves as the SSOT. Network responses update the local database, which then reactively updates the UI through StateFlow emissions. This prevents stale data and ensures consistency.

#### 4. Koin for Dependency Injection
Chosen over Hilt/Dagger for its simplicity and native KMP support. Module declarations are explicit and grouped by feature.

#### 5. Coil with Aggressive Caching
Image loading configured with 25% memory cache and 100MB disk cache to minimize network requests in an image-heavy app.

## Diagrams

Architecture diagrams are rendered directly in the [README](../../README.md) using Mermaid syntax.
