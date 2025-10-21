# 🏛️ Windy Civi Pipelines

**Production-scale data infrastructure for U.S. legislative transparency**

This GitHub organization maintains the backend data pipelines that power [Windy Civi](https://github.com/windy-civi) — an open-source initiative to make legislative data permanent, verifiable, and accessible.

We build **reliable, reproducible pipelines** that process legislative data from all 50 states and Congress, creating a blockchain-style archive that survives administration changes, website redesigns, and data loss.

[![Main Repository](https://img.shields.io/badge/Main_Repo-opencivicdata--blockchain--transformer-blue)](https://github.com/windy-civi/opencivicdata-blockchain-transformer)
[![Active Pipelines](https://img.shields.io/badge/Active_Pipelines-14-green)](https://github.com/windy-civi-pipelines)

---

## 🎯 What We Do

Each state repository in this organization runs automated workflows that:

- 🔁 **Scrape** legislative data nightly using [OpenStates](https://github.com/openstates/openstates-scrapers) scrapers
- 🧼 **Sanitize** output by removing ephemeral fields for deterministic versioning
- 🧠 **Structure** data in blockchain-style directories with full provenance tracking
- 🔗 **Link** bills, votes, and events across sessions and chambers
- 📄 **Extract** full text from PDFs, XMLs, and HTML (multi-format fallback)
- 📊 **Monitor** data quality with automated orphan detection
- 📂 **Commit** processed data to GitHub for permanent, auditable storage

**Result:** A complete, versioned legislative record for each state — resilient to source changes and always accessible.

---

## 🏗️ Architecture Highlights

### Production Features

✅ **Incremental Processing** - Only processes new/changed bills (no wasted compute)  
✅ **Auto-Save Failsafe** - Commits every 30 minutes to survive 6-hour GitHub Actions timeout  
✅ **Data Quality Monitoring** - Tracks orphaned bills (votes without bill data) to catch scraper issues  
✅ **Concurrent Updates** - Multiple jobs can safely update repos with git rebase conflict resolution  
✅ **Multi-Format Text Extraction** - XML → HTML → PDF fallback with strikethrough detection  
✅ **Fault Tolerance** - Individual bill failures don't block the entire pipeline  

### Technical Stack

- **Orchestration:** GitHub Actions (Docker + Python)
- **Language:** Python 3.12+ (pipenv, black, type hints)
- **Text Extraction:** pdfplumber, PyPDF2, BeautifulSoup4, lxml
- **Data Format:** JSON with ISO timestamps and semantic versioning
- **Storage:** Git (GitHub) for versioning + permanent URLs

---

## 📊 Current Status

| Jurisdiction          | Status        | Bills Tracked | Last Updated |
| --------------------- | ------------- | ------------- | ------------ |
| 🇺🇸 **Federal (USA)**  | ✅ Production | ~15,000+      | Daily        |
| 🏛️ **Illinois (IL)**  | ✅ Production | ~8,000+       | Daily        |
| 🏛️ **Tennessee (TN)** | ✅ Production | ~2,000+       | Daily        |
| 🏛️ **Texas (TX)**     | ✅ Production | ~7,000+       | Daily        |
| 🏛️ **Wisconsin (WI)** | ✅ Production | ~1,500+       | Daily        |
| 🏛️ **Wyoming (WY)**   | ✅ Production | ~500+         | Daily        |
| ...                   | 🚧 Onboarding | -             | -            |

**Template Repository:** [windy-civi-template-pipeline](https://github.com/windy-civi-pipelines/windy-civi-template-pipeline)

---

## 🗂️ Data Format

Each state repository produces structured, version-controlled output:

```
data_output/
├── data_processed/
│   └── country:us/state:il/
│       └── sessions/103/
│           ├── bills/
│           │   └── HB1234/
│           │       ├── metadata.json           # Bill data + processing timestamps
│           │       ├── files/                  # Extracted text
│           │       │   ├── HB1234_text.xml
│           │       │   ├── HB1234_text.pdf
│           │       │   └── HB1234_text_extracted.txt
│           │       └── logs/                   # Actions, votes, events
│           └── events/                         # Committee hearings
├── orphaned_placeholders_tracking.json  # Data quality monitoring
└── data_not_processed/                  # Error logs by category
```

**Key Features:**

- `_processing` timestamps track when logs and text were last updated
- Incremental updates only touch changed bills
- Orphan tracking identifies bills with votes/events but no metadata
- Errors categorized by type (download failures, parsing errors, etc.)

---

## 🔧 Pipeline Architecture

Pipelines run as **modular GitHub Actions** in two stages:

### Stage 1: Scrape & Format (Sequential Jobs)

1. **Scrape** - Docker-based OpenStates scraper
2. **Format** - Process data, link events, monitor quality

### Stage 2: Text Extraction (Independent)

- Runs separately to handle 4-6 hour extraction times
- Auto-saves progress every 30 minutes
- Resumes on restart if timeout occurs

**Why this design?**

- Decouples fast metadata updates from slow text extraction
- Prevents scraping from blocking on extraction timeouts
- Enables independent debugging of each stage
- Allows selective re-runs (e.g., re-extract without re-scraping)

---

## 🛠️ Technologies & Patterns

**Patterns Implemented:**

- Incremental processing with timestamp-based change detection
- Two-level timestamps (bill-level + action-level)
- Fault-tolerant design (individual failures don't block pipeline)
- Data quality monitoring with orphan detection
- Auto-save for long-running processes
- Git-based conflict resolution for concurrent updates

**Code Quality:**

- Type hints throughout
- Black code formatting
- Modular handler/utility architecture
- Comprehensive error logging
- Unit + integration tests

---

## 🧪 Testing & Validation

Each pipeline includes:

- ✅ Test suite with synthetic and real data
- ✅ Incremental processing validation (100% skip rate on re-runs)
- ✅ Orphan detection with multi-run simulation
- ✅ Text extraction with multi-format fallback testing

**Example:** USA pipeline test with 205 bills - all placeholders cleaned, 0 orphans detected.

---

## 📚 Documentation

Comprehensive docs in the main repository:

- **[Main Repository](https://github.com/windy-civi/opencivicdata-blockchain-transformer)** - Full technical documentation
- **[Setup Guide](https://github.com/windy-civi/opencivicdata-blockchain-transformer/blob/main/docs/for-caller-repos/README_TEMPLATE.md)** - State pipeline setup
- **[Incremental Processing](https://github.com/windy-civi/opencivicdata-blockchain-transformer/blob/main/docs/incremental_processing/)** - How updates work
- **[Orphan Tracking](https://github.com/windy-civi/opencivicdata-blockchain-transformer/blob/main/docs/orphan_tracking.md)** - Data quality monitoring

---

## 🚀 Current Development: Civic Engagement Bots

We're building **automated social media bots** that turn legislative data into actionable content for organizations, advocacy groups, and elected officials.

**Why bots?** We built a civic engagement app first - and nobody used it. The lesson: don't make people come to _your_ platform. **Meet people where they already are** (Twitter, BlueSky, etc.). Instead of building another civic tech app that sits unused, we're integrating legislative transparency into the feeds people already follow.

### How It Works

Organizations sign up and configure:

- **Topics of interest** (keywords, policy areas, bill sponsors)
- **Social platforms** (BlueSky, Twitter/X, etc.)
- **Posting preferences** (frequency, format, tone)

The system automatically:

1. Monitors legislative activity across all states
2. Matches new bills/actions to org-specific topics
3. Generates shareable posts (summaries, links, impact notes)
4. Publishes to configured social accounts

### Example Use Cases

**Advocacy Organizations:**

- **Black Lives Matter** → Auto-post bills about criminal justice reform, police accountability, voting rights
- **LGBTQIA+ Groups** → Track legislation affecting LGBTQ+ rights, healthcare access, discrimination laws
- **Environmental Orgs** → Monitor climate policy, renewable energy bills, conservation funding

**Elected Officials:**

- **State Senators** → Auto-post when they sponsor or co-sponsor legislation
- **U.S. Congress** → Share bills they vote on with constituent-friendly summaries
- **Local Representatives** → Keep constituents informed about state-level impacts

**Researchers & Journalists:**

- **Policy Think Tanks** → Track legislation in specific domains (education, healthcare, housing)
- **News Outlets** → Get alerts on high-impact bills before they become news

### Technical Architecture

- **Data Pipeline** → Provides clean, structured, timestamped bill data
- **AI Layer** → Summarizes bills, extracts themes, identifies relevance
- **Bot Engine** → Generates platform-specific posts, manages posting schedules
- **API** → Allows orgs to configure topics, approve/reject posts, view analytics

**Why this matters:** Most people don't read legislative data, but they _do_ follow organizations they trust on social media. This bridges that gap — making legislative transparency automatic, accessible, and actionable.

---

## 🗺️ Roadmap

**Phase 1: Infrastructure** ✅

- ✅ Incremental processing (complete)
- ✅ Auto-save failsafe (complete)
- ✅ Orphan tracking (complete)
- 🚧 Expanding to all 50 states
- 🚧 Historical data backfill

**Phase 2: Intelligence Layer** 🚧

- 🚧 AI-powered bill summarization
- 🚧 Topic classification and keyword matching
- 🚧 Impact analysis (who's affected, how)
- 🚧 Automated post generation

**Phase 3: Engagement Platform** 📋

- 📋 Organization signup and configuration
- 📋 Social media API integrations
- 📋 Post approval workflows
- 📋 Analytics dashboard

**Phase 4: Decentralization** 🔮

- Integration with decentralized storage (IPFS)
- Blockchain-based provenance tracking
- Federated bot hosting

---

## 🤖 Development Notes

This project was built with AI assistance (ChatGPT, Cursor) as technical collaborators. AI helped with:

- Architecture exploration and design tradeoffs
- Workflow refactoring and optimization
- Documentation and error handling
- Debugging complex edge cases

The result: more maintainable code, better documentation, and faster iteration — while deepening my understanding of distributed systems, data pipelines, and production-grade automation.

---

## 🤝 Contributing

Interested in civic tech? We welcome:

- 🏛️ **State pipeline onboarding** - Help add your state
- 🐛 **Bug reports** - Found an issue? Open a ticket
- 💡 **Feature ideas** - Suggest improvements
- 📖 **Documentation** - Help clarify setup or usage

**Getting Started:**

1. Check out the [main repository](https://github.com/windy-civi/opencivicdata-blockchain-transformer)
2. Review the [setup guide](https://github.com/windy-civi/opencivicdata-blockchain-transformer/blob/main/docs/for-caller-repos/README_TEMPLATE.md)
3. Open an issue or PR

---

## 📫 Contact

- **Main Project:** [Windy Civi](https://github.com/windy-civi)
- **Website:** [windycivi.com](https://windycivi.com)
- **Organization:** Chicago-based civic tech initiative

---

**Building open, durable civic infrastructure — one state at a time.** 🏛️

_Part of the [Windy Civi](https://github.com/windy-civi) ecosystem._
