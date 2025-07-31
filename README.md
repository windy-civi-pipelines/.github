# Windy Civi Pipelines Organization

This GitHub organization maintains the backend infrastructure, data pipelines, and automation tools that power the [Windy Civi](https://github.com/windy-civi) project.

Our focus is on building reliable, reproducible, and transparent workflows that:

- 🔁 Run nightly ETL jobs for legislative data from [Open States](https://github.com/openstates/openstates-scrapers)
- 🧼 Sanitize and standardize output for long-term civic use
- 🧠 Structure data in a blockchain-style format for traceability and permanence
- 📁 Commit processed data to state-level repos for public access

We support each U.S. state with a templated GitHub Actions-powered pipeline, allowing maintainers and contributors to set up automated scrapes, formatting, and archival with minimal config.

This organization is collaborative by design. All pipeline repos are meant to integrate with the core [Windy Civi](https://github.com/windy-civi) project, which provides frontend access, API layers, and civic engagement tools.

## 🚀 Repositories in This Org

Each state has its own `STATE-data-pipeline` repo, created from a shared template. These repos:

- Include nightly GitHub Actions workflows
- Format data using shared Python formatter logic
- Commit versioned output to GitHub for transparency and public access

## 👋 Get Involved

We welcome contributions from developers, civic technologists, and anyone interested in making government data more accessible.

Visit the main project at [Windy Civi](https://github.com/windy-civi) or reach out if you'd like to help us build pipelines, improve formatting logic, or onboard new states.

---

Part of the [Windy Civi](https://windycivi.com) ecosystem.
