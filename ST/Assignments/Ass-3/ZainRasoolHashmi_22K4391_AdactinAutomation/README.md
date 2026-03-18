# Adactin Hotel App Test Automation (Playwright)

This repository contains UI automation for the Adactin Hotel Booking application using **Playwright Test** and the **Page Object Model (POM)** pattern.

## Project Overview

The suite validates critical user journeys for hotel booking, including:

- End-to-end booking flow
- Positive smoke/regression checks
- Negative validation/error-handling scenarios

Target application:

- <https://adactinhotelapp.com/>

Framework stack:

- Playwright Test (`@playwright/test`)
- JavaScript (ES Modules)
- Data-driven testing using JSON files

## Repository Structure

```text
.
|- Data/
|  |- e2e.json
|  |- negative.json
|  \- positive.json
|- Pages/
|  |- BookingPage.js
|  |- ConfirmationPage.js
|  |- ItineraryPage.js
|  |- LoginPage.js
|  |- SearchHotelPage.js
|  \- SearchPage.js
|- tests/
|  |- e2e.spec.js
|  |- negative/
|  |  |- invalidBooking.spec.js
|  |  |- invalidCC.spec.js
|  |  |- invalidHotelSelection.spec.js
|  |  |- invalidLogin.spec.js
|  |  \- invalidSearch.spec.js
|  \- positive/
|     |- validBooking.spec.js
|     |- validLogin.spec.js
|     |- validlogout.spec.js
|     |- validSearch.spec.js
|     \- validSelection.spec.js
|- playwright.config.js
|- package.json
\- .gitignore
```

## Test Design

### 1. Page Object Model (POM)

Each screen has a dedicated page class under `Pages/` that encapsulates locators and UI actions.

Examples:

- `LoginPage.js`: login navigation and authentication action
- `SearchPage.js`: search form fields and submission
- `BookingPage.js`: guest and card details form
- `ConfirmationPage.js`: order number extraction and itinerary navigation
- `ItineraryPage.js`: order search and logout

This improves maintainability by centralizing selectors and reducing duplication in tests.

### 2. Data-Driven Approach

Test data is separated from test logic:

- `Data/positive.json`: valid login/search/booking data
- `Data/negative.json`: invalid cases and expected messages
- `Data/e2e.json`: consolidated full-flow data

### 3. Test Layers

- `tests/e2e.spec.js`: complete booking flow from login to logout
- `tests/positive/`: valid functional behavior
- `tests/negative/`: error-path validation and form/input constraints

## Covered Scenarios

### End-to-End

- **TC01**: Full booking workflow including itinerary search and logout

### Positive Suite

- **TC02**: Successful login with valid credentials
- **TC03**: Hotel search with valid filters
- **TC04**: Hotel selection and booking interaction
- **TC05**: Booking confirmation shows generated order number
- **TC06**: Logout after completing a booking

### Negative Suite

- **TC07**: Login with invalid credentials
- **TC08**: Search without mandatory location field
- **TC09**: Continue without selecting a hotel result
- **TC10**: Booking with missing personal details
- **TC11**: Booking with invalid credit-card number

## Prerequisites

Install the following on your machine:

- Node.js 18+ (recommended LTS)
- npm (bundled with Node.js)

## Installation

1.Clone the repository.
2.Install dependencies:

```bash
npm install
```

3.Install Playwright browsers:

```bash
npx playwright install
```

## Running Tests

Since `package.json` currently has no npm scripts, use Playwright CLI commands directly.

Run all tests:

```bash
npx playwright test
```

Run only end-to-end test:

```bash
npx playwright test tests/e2e.spec.js
```

Run only positive tests:

```bash
npx playwright test tests/positive
```

Run only negative tests:

```bash
npx playwright test tests/negative
```

Run in headed mode:

```bash
npx playwright test --headed
```

Run in a specific browser project:

```bash
npx playwright test --project=chromium
```

## Reporting

This project uses Playwright HTML reporting (`reporter: 'html'`).

After execution, open report:

```bash
npx playwright show-report
```

Generated artifacts include:

- `playwright-report/`
- `test-results/`
- trace data for failed retries (`trace: 'on-first-retry'`)

## Configuration Notes

Current `playwright.config.js` highlights:

- `testDir: './tests'`
- parallel execution enabled (`fullyParallel: true`)
- retries on CI only
- projects:
  - chromium
  - firefox
  - webkit

## Test Data and Security

- Do not commit real production credentials.
- Keep demo/test-only accounts in data files.
- Prefer environment variables (`.env`) for sensitive values in future enhancements.

## Known Considerations

- Hard-coded test credentials and dates can become stale over time.
- Fixed date inputs in test data may fail depending on application date validation windows.
- To improve reliability, consider generating dynamic check-in/check-out dates in helper utilities.

## Suggested Improvements

- Add npm scripts in `package.json` for common commands (smoke, positive, negative, e2e).
- Add CI workflow (GitHub Actions) to run tests on pull requests.
- Add tags (`@smoke`, `@regression`, `@negative`) for selective execution.
- Add reusable utility layer for date handling and random test data generation.
- Add screenshots/video retention strategy for failed test triage.

## Quick Start (Minimal)

```bash
npm install
npx playwright install
npx playwright test
npx playwright show-report
```

## License

This project currently uses the `ISC` license as declared in `package.json`.
