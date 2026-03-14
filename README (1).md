# SDLC-Assignment-[YourName]
### Course: SDLC & DevOps Fundamentals
### Project: Smart-Attend — Student Attendance Mobile App

---

## 📁 Repository Contents

| File | Description |
|------|-------------|
| `SDLC_Analysis.docx` | Full written analysis — Task 1 (Waterfall Failure) & Task 2 (Agile/Scrum Pivot) |
| `Sprint_Backlog_SmartAttend.xlsx` | Sprint 1 & Sprint 2 Backlogs with User Stories, Priorities & Estimates |
| `README.md` | This file — project overview and CI/CD explanation |

---

## 🚀 Why CI/CD Would Be Used for Smart-Attend

### What is CI/CD?

**CI/CD** stands for **Continuous Integration / Continuous Deployment**. It is a DevOps practice that automates the process of building, testing, and releasing software every time a developer pushes new code.

- **Continuous Integration (CI):** Every time a developer commits code, an automated pipeline runs unit tests, integration tests, and code quality checks. If any test fails, the team is immediately notified — the broken code is never merged into the main branch.

- **Continuous Deployment (CD):** Once all tests pass, the new version of the app is automatically deployed to a staging environment (and optionally to production) without any manual steps.

---

### Why CI/CD is Essential for Smart-Attend's Sprint Updates

In our Scrum workflow, the team delivers a new working increment every **2 weeks**. Without CI/CD, deploying each Sprint update to students and teachers would require:

1. Manually building the app for Android and iOS.
2. Uploading to the Google Play Store and Apple App Store separately.
3. Waiting for app store review processes (which can take 1–3 days).
4. Students manually updating the app on their devices.

This would make our 2-week Sprint cycle completely impractical.

**With CI/CD, here is what happens instead:**

```
Developer pushes code to GitHub
        ↓
CI Pipeline triggers automatically
        ↓
Automated Tests run (Unit → Integration → UI)
        ↓
If ALL tests pass → Build is compiled for Android + iOS
        ↓
Build deployed to Test Environment (for QA review)
        ↓
On Sprint approval → deployed to Production (Play Store / App Store)
        ↓
Students receive an automatic over-the-air (OTA) update notification
```

---

### Specific Benefits for Smart-Attend

| CI/CD Benefit | How It Helps Smart-Attend |
|---|---|
| **Automated Testing** | Every code push triggers Geo-fencing and Face ID tests — bugs are caught before students are affected |
| **Fast Sprint Delivery** | At the end of Sprint 1, the MVP is deployed to all students automatically — no manual release process |
| **Safe Biometric Updates** | Sprint 2 Face ID code is tested in isolation before reaching any student device |
| **Rollback Capability** | If a bad update crashes the app during class, CI/CD allows instant rollback to the previous working version |
| **Environment Separation** | Code goes: `dev branch → staging (testing) → production (students)` — students only ever see stable, tested code |

---

### Tools That Would Be Used

- **GitHub Actions** — CI/CD pipeline automation (free with GitHub)
- **Jest / Detox** — Automated unit and end-to-end testing for React Native
- **Fastlane** — Automates iOS and Android app builds and store deployments
- **Firebase App Distribution** — For distributing test builds to QA testers before production release
- **Google Play Store / Apple TestFlight** — Final production deployment channels

---

### Example CI/CD Pipeline (GitHub Actions)

```yaml
name: Smart-Attend CI/CD Pipeline

on:
  push:
    branches: [main, sprint-*]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: npm install
      - name: Run unit tests
        run: npm test
      - name: Run integration tests
        run: npm run test:integration

  deploy-staging:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Build and deploy to staging
        run: npm run build:staging && npm run deploy:staging

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy to App Stores
        run: fastlane deploy
```

This pipeline ensures that **every Sprint update reaches students automatically, reliably, and only after passing all tests** — making it the backbone of our Agile delivery process.

---

## 📋 Project Summary

**App Name:** Smart-Attend  
**Purpose:** Automate student attendance using Geo-fencing + Biometric Face ID verification  
**Methodology:** Scrum (Agile) — 2-week Sprints  
**Sprint 1 Goal:** MVP — Login, Geo-fencing, Real-time Dashboard  
**Sprint 2 Goal:** Face ID Integration, Dashboard Refinement, CSV Export  

---

*Submitted for SDLC & DevOps Fundamentals — March 2026*
