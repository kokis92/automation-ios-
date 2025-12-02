# automation-ios-
End-to-end Swift XCTest automation framework with CI/CD using GitHub Actions
# Swift XCTest Automation Framework (GitHub Actions CI)

A professional, end-to-end QA automation portfolio project. This repo houses a modular **XCTest/XCUITest** framework for iOS that integrates with **GitHub Actions** for CI, reporting, and dashboards. It’s designed to mirror real-world SDET workflows and prepare you for QA Automation interviews within a year.

---

## 🎯 Goals

* Build hands-on expertise by automating key **UI, API, and integration** scenarios.
* Run suites automatically via **GitHub Actions** .
* Produce readable **reports & dashboards** with CI visibility.
* Ship a polished **GitHub portfolio** project.

## 🧭 Vision

* **Automation coverage:** core user flows (smoke, regression), targeted API checks, and end-to-end paths.
* **CI-first:** green-to-merge checks, nightly regressions, and artifacts (logs, screenshots, videos, reports).
* **Scalable design:** Page Object Model, test data utilities, environment config, and parallel readiness.

## 🧰 Stack & Tools

* **Language:** Swift (XCTest/XCUITest) — optional Python for API automation
* **Frameworks:** XCTest, XCUITest, URLSession/Alamofire (if needed), Swift Concurrency where helpful
* **CI/CD:** GitHub Actions
* **Version Control:** Git + GitHub
* **Reporting:** Allure (optional), Xcode logs & attachments

## 🗺️ Roadmap (12 Months)

* **Months 1–2:** Environment setup, basic tests, connect to GitHub
* **Months 3–4:** Framework architecture (Page Object Model), utilities
* **Months 5–6:** Integrate ATP + GitHub Actions for automated runs
* **Months 7–9:** Add API + data-driven testing, stabilize & flake-reduce
* **Months 10–12:** Polish docs, reporting, and portfolio presentation

---

## 🏗️ Repository Structure (suggested)

```
.
├── README.md
├── .github/
│   └── workflows/
│       └── ios-tests.yml
├── fastlane/                      # optional: build/test lanes
├── Scripts/
│   ├── bootstrap.sh                # install tools, pods, gems, etc.
│   ├── test.sh                     # local runner (CLI options)
│   └── export-artifacts.sh         # collect logs/screenshots/reports
├── Sources/
│   └── TestSupport/                # shared code (HTTP, fixtures, utils)
├── Tests/
│   ├── UITests/
│   │   ├── Pages/                  # Page Objects
│   │   ├── Flows/                  # business flows across pages
│   │   ├── Fixtures/               # test data builders
│   │   ├── Helpers/                # waits, assertions, screenshots
│   │   └── Smoke/Regression/       # group test targets
│   └── APITests/                   # optional: API suite (Swift or Python)
├── Config/
│   ├── env.plist                   # env config (base URLs, flags)
│   └── testplan.xctestplan         # organize configs/suites
└── Reports/                        # CI artifacts, Allure results
```

---

## 🧩 Architecture Highlights

* **Page Object Model (POM):** Pages expose stable actions/assertions; no raw selectors in tests.
* **Flows:** Reusable business journeys (e.g., login → purchase) composed of page methods.
* **Test Data:** Builders/fixtures for clean, deterministic input; optional seeded accounts.
* **Config:** Centralized per-env settings (dev/stage/prod-like), toggled via scheme or test plan.
* **Resilience:** Smart waits, retry-on-known-flakes (judicious), screenshots on failure.
* **Parallel Ready:** Keep tests stateless; avoid cross-test coupling; isolate data.

### Sample Page Object (Swift)

```swift
final class LoginPage {
    private let app: XCUIApplication
    init(app: XCUIApplication) { self.app = app }

    var emailField: XCUIElement { app.textFields["email"] }
    var passwordField: XCUIElement { app.secureTextFields["password"] }
    var signInButton: XCUIElement { app.buttons["sign_in"] }

    @discardableResult
    func login(email: String, password: String) -> HomePage {
        emailField.tap(); emailField.typeText(email)
        passwordField.tap(); passwordField.typeText(password)
        signInButton.tap()
        return HomePage(app: app)
    }
}
```

---

## 🚀 Local Setup

1. **Clone & bootstrap**

   ```bash
   git clone <your-repo-url>
   cd repo && bash Scripts/bootstrap.sh
   ```
2. **Open Xcode** and select the `UITests` scheme.
3. **Run tests** locally:

   ```bash
   bash Scripts/test.sh --suite smoke --device "iPhone 15"
   ```

> Tip: Put environment values in `Config/env.plist` and read with a tiny helper (e.g., `Env.current.baseURL`).

---

## 🔁 GitHub Actions (iOS Simulator) – Example

```yaml
name: iOS UI Tests
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  ui-tests:
    runs-on: macos-14
    steps:
      - uses: actions/checkout@v4
      - name: Select Xcode
        run: sudo xcode-select -switch /Applications/Xcode_16.app
      - name: Bootstrap
        run: bash Scripts/bootstrap.sh
      - name: Run UI Tests (Smoke)
        run: bash Scripts/test.sh --suite smoke --device "iPhone 15"
      - name: Collect Artifacts
        run: bash Scripts/export-artifacts.sh
      - name: Upload Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: ui-test-artifacts
          path: Reports/
```

---

## 🔁 CI Integration (Current)

Your test suite currently runs via **GitHub Actions**. You can optionally integrate other CI tools in the future (e.g., Jenkins, Bitrise, CircleCI).

## 🧪 Reporting & Artifacts & Artifacts

* **Xcode/XCTest:** XCTAttachments (screens, logs) captured on failure.
* **Allure (optional):** Export results to `Reports/allure-results/` and publish in CI.
* **ATP:** Central report with pass/fail trends and run history.

---

## 🧰 Utilities & Conventions

* **Waits:** Prefer expectation-based waits; avoid hard sleeps.
* **Assertions:** Small, descriptive helpers; include context in messages.
* **Selectors:** Use accessibility identifiers; ban index-based traversal where possible.
* **Flake Handling:** Retry only for known transient issues; track in a `FlakeLog.md`.
* **Coding Style:** SwiftFormat/SwiftLint (optional) for consistency.

---

## 📦 API Testing (Optional Paths)

* **Swift:** Use `URLSession` with lightweight client + response models.
* **Python:** Keep API tests in `Tests/APITestsPython/` with `pytest` + `requests`; wire into CI as a separate job.

---

## 🤝 Contribution Workflow (Solo-friendly)

1. Create a branch per feature/suite.
2. Add/adjust tests + docs.
3. Run locally, then open a PR to `main`.
4. Ensure CI checks are green (GitHub & ATP) before merge.

---

## 🧭 Documentation to Include

* `ARCHITECTURE.md` – POM, utilities, data strategy, env handling
* `TESTPLAN.md` – suites (smoke/regression/e2e), devices, schedules
* `SETUP.md` – detailed bootstrap for new machine/CI
* `ATP.md` – ATP pipelines, secrets, dashboards

---

## ✅ Next Steps Checklist (do these first)

* [ ] **Create repo** on GitHub and paste this README.
* [ ] **Add .github/workflows/ios-tests.yml** using the sample above.
* [ ] **Write `bootstrap.sh`** to install tools (brew, bundler, cocoapods/swiftpm, etc.).
* [ ] **Write `test.sh`** to run Xcode tests with CLI flags for suite/device and export to `Reports/`.
* [ ] **Scaffold POM**: `LoginPage`, `HomePage`, one end-to-end **smoke** test.
* [ ] **Enable artifacts** (screenshots on failure, logs) and verify in Actions run.
* [ ] **Draft `ATP.md`** with your target pipeline shape; add TODOs for secrets + triggers.
* [ ] **Decide reporting** (stick with XCT first; add Allure later if needed).
* [ ] **Add badges** (CI status, coverage if applicable) to the top of this README.
* [ ] **Open Issues** for Months 1–2 tasks and tag with `milestone:m1-m2`.

---

## 🔮 Stretch Ideas

* Device matrix (iOS versions, iPhone/iPad), shard by suite.
* Flake dashboard (simple CSV → chart) and weekly stability report.
* Contract tests for APIs; mock server for deterministic UI flows.
* Parallelization with multiple simulators; selective test runs on changed areas.

---

## 📄 License

Choose a license (e.g., MIT) to make reuse clear.
