# Testing Strategy

> This document details ShowMate's testing approach, frameworks, and philosophy.

## Philosophy

ShowMate follows the **Testing Pyramid** approach:
- Heavy investment in **unit tests** (fast, isolated, covering business logic)
- Moderate **integration tests** (repository layer, database interactions)
- Targeted **screenshot tests** (visual regression for key UI components)
- Minimal **benchmark tests** (critical performance paths)

## Test Distribution

```
Total: 586 Tests — 100% Pass Rate
═══════════════════════════════════════════

  ViewModels & Presentation    ████████████████████████████  ~350 tests
  Use Cases & Domain Logic     ██████████████████           ~150 tests
  Repository (KMP :shared)     ████████                      85 tests
  Screenshot (Roborazzi)       █                             ~10 tests
  Benchmark                    ▏                               1 test
```

## Frameworks & Tools

| Tool | Purpose |
|:--|:--|
| **JUnit4** | Test runner for all unit tests |
| **MockK** | Mocking framework for Kotlin (replaces Mockito) |
| **Turbine** | Testing Kotlin Flows and StateFlow emissions |
| **Robolectric** | Running Android tests on JVM without emulator |
| **Roborazzi** | Screenshot testing with pixel-perfect comparison |
| **Macrobenchmark** | Cold startup and frame timing benchmarks |
| **Kover** | Kotlin-native code coverage reporting |

## What's Tested

### ViewModel Tests (~350)
- State initialization and default values
- User action handling (toggle watched, add favorite, rate show)
- Error state propagation
- Loading → Success → Error state transitions
- Navigation events
- Edge cases (empty lists, network errors, null data)

### Domain / Use Case Tests (~150)
- Recommendation scoring algorithm correctness
- Group consensus with variance penalty
- Churn prediction thresholds
- Genre affinity decay over time
- Achievement unlocking conditions
- Boundary conditions and floating-point precision

### Repository Tests (85 — KMP `:shared`)
- Data mapping between DTOs ↔ Domain Models
- Offline sync queue operations
- Cache invalidation logic
- Concurrent access patterns

### Screenshot Tests (~10)
- `AuthPrimaryButton` — Enabled, Disabled, Loading states
- `ShowCard` — With data, loading placeholder
- `UiStateHandler` — Loading, Error, Empty, Content states
- Dark theme consistency

## Running Tests

```bash
# Full test suite
./gradlew testDebugUnitTest

# Only KMP shared module tests
./gradlew :shared:testDebugUnitTest

# Screenshot tests — generate baselines
./gradlew recordRoborazziDebug

# Screenshot tests — verify against baselines
./gradlew verifyRoborazziDebug

# Coverage report
./gradlew koverHtmlReportDebug
# Output: app/build/reports/kover/htmlDebug/index.html

# Benchmark
./gradlew :benchmark:connectedAndroidTest
```

## CI Integration

Tests run automatically on every push via GitHub Actions:
1. Checkout code
2. Set up JDK 17
3. Run `./gradlew testDebugUnitTest`
4. Upload test reports as artifacts
5. Fail the build on any test failure
