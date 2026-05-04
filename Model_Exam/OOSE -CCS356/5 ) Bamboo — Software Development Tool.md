# 🎋 Bamboo — Software Development Tool for Integration Mechanisms

**Subject:** Software Engineering / DevOps | **Marks:** 15 | **Type:** Theory + Diagram

---

> 🧠 **Core Idea (1 Line):** Atlassian Bamboo is a **Continuous Integration and Continuous Delivery (CI/CD)** server that automates the build, test, and deployment pipeline by integrating with source control, build tools, test frameworks, and project tracking tools — enabling rapid, reliable software delivery.

---

## 🔷 PART 1: Introduction to Bamboo

### 📌 What is Bamboo?

> **Bamboo** is a **commercial CI/CD automation server** developed by **Atlassian** (the company behind Jira, Confluence, and Bitbucket). It automates the building, testing, and releasing of software, providing a centralized platform that connects all stages of the software development lifecycle.

### Key Classification:

|Property|Details|
|---|---|
|**Developer**|Atlassian (launched 2007)|
|**Type**|CI/CD server, DevOps tool|
|**Current version**|Bamboo Data Center (Server edition EOL: Feb 15, 2024)|
|**Platform**|Windows, macOS, Linux, Docker, AWS|
|**Language**|Java-based|
|**License**|Commercial / proprietary (per-agent pricing)|

### Why Bamboo for Integration?

The core problem in software development is that multiple developers commit code simultaneously to a shared repository. Without an integration mechanism:

- Code conflicts accumulate silently → **integration hell**
- Bugs discovered late → expensive to fix
- Manual deployments → error-prone, inconsistent
- No visibility into which commit broke the build

Bamboo solves this by continuously **integrating, building, and testing** every commit automatically, providing immediate feedback to the team.

---

## 🔷 PART 2: Bamboo Architecture — Hierarchical Structure

Bamboo organizes all CI/CD work into a strict **five-level hierarchy**:

```
+─────────────────────────────────────────────────+
│  PROJECT (namespace for related plans)          │
│  ┌───────────────────────────────────────────┐  │
│  │  PLAN (defines the full build workflow)   │  │
│  │  ┌────────────────────────────────────┐   │  │
│  │  │  STAGE 1 — Build (sequential)      │   │  │
│  │  │  ┌──────────┐   ┌──────────┐       │   │  │
│  │  │  │ JOB A    │   │ JOB B    │       │   │  │
│  │  │  │(parallel)│   │(parallel)│       │   │  │
│  │  │  │TASK:     │   │TASK:     │       │   │  │
│  │  │  │Compile   │   │Lint      │       │   │  │
│  │  │  └──────────┘   └──────────┘       │   │  │
│  │  ├────────────────────────────────────┤   │  │
│  │  │  STAGE 2 — Test (runs if 1 passes) │   │  │
│  │  │  ┌──────┐  ┌──────┐  ┌──────┐     │   │  │
│  │  │  │JUnit │  │Maven │  │Sonar │     │   │  │
│  │  │  │tests │  │integ.│  │Qube  │     │   │  │
│  │  │  └──────┘  └──────┘  └──────┘     │   │  │
│  │  ├────────────────────────────────────┤   │  │
│  │  │  STAGE 3 — Deploy                  │   │  │
│  │  │  Publish artifact → environments   │   │  │
│  │  └────────────────────────────────────┘   │  │
│  └───────────────────────────────────────────┘  │
+─────────────────────────────────────────────────+
```

### Level-by-Level Explanation:

|Level|Description|Key Point|
|---|---|---|
|**Project**|Top-level container, namespace for Plans|Organizes related build workflows|
|**Plan**|Defines the complete CI/CD workflow|Triggered by code commits|
|**Stage**|A logical phase (Build, Test, Deploy)|Stages run **sequentially**|
|**Job**|A unit of work assigned to an agent|Jobs within a stage run **in parallel**|
|**Task**|The smallest discrete work unit|Examples: Maven goal, script execution, checkout|

---

## 🔷 PART 3: Bamboo Integration Mechanisms

### 📌 What is a CI Integration Mechanism?

> An **integration mechanism** is the automated process by which Bamboo connects the developer's code commit with the build, test, deployment, and notification systems — without any manual intervention.

---

### 🔹 Integration Mechanism 1: Source Control Integration

Bamboo supports the following version control systems (VCS):

|VCS|Integration Method|
|---|---|
|**Git**|Native; SSH key-pair authentication|
|**Bitbucket**|First-class; webhook triggers, branch detection|
|**GitHub / GitLab**|REST API-based integration|
|**SVN**|Subversion support|
|**Mercurial**|Native support|
|**Perforce**|Supported via connector|
|**CVS**|Legacy support|
|**Fisheye**|Code review and commit traceability|

**How it works:**

```
Developer pushes commit
        ↓
Bitbucket/Git receives commit → sends webhook to Bamboo
        ↓
Bamboo detects change → determines which Plans are affected
        ↓
Bamboo queues a build → assigns to available Build Agent
```

**Branch detection:** Bamboo automatically detects new branches and brings them under the same CI scheme as `main`. Any two branches can also be automatically merged before each test run.

---

### 🔹 Integration Mechanism 2: Build Agents

Build agents are the **workers** that actually execute Jobs and Tasks. Bamboo supports multiple agent types:

|Agent Type|Location|Use Case|
|---|---|---|
|**Local Agent**|Runs on the Bamboo server itself|Small teams, low concurrency|
|**Remote Agent**|Runs on any other connected machine|Distributed builds, OS-specific builds|
|**AWS Cloud Agent**|Elastic agents on Amazon Web Services|Scale builds on demand|
|**Docker Agent**|Containerised, isolated build environment|Reproducible, consistent builds|

**Agent Capabilities:** Each agent exposes a set of capabilities (e.g., JDK version, Maven version, OS). Bamboo matches job requirements to agent capabilities before dispatch.

**Parallel execution:** Multiple agents allow multiple jobs to run simultaneously, drastically reducing total build time.

---

### 🔹 Integration Mechanism 3: Build Tool Integration

Bamboo integrates with all major build and compilation tools out of the box (no plugins needed):

|Tool|Purpose|
|---|---|
|**Maven**|Java project build, dependency management|
|**Ant**|XML-based Java build scripting|
|**Gradle**|Modern JVM build automation|
|**MSBuild**|.NET / Windows builds|
|**Xcode**|iOS / macOS builds|
|**Grails**|Groovy/Grails framework builds|
|**npm / Yarn**|Node.js front-end builds|

**Bamboo Specs (Configuration as Code):** Plans and stages can be defined using **Bamboo Specs** — a Domain-Specific Language (DSL) in YAML or Java. These specs are stored in version control alongside the source code, enabling:

- Full traceability of pipeline configuration changes
- Easy replication of build environments across projects
- Peer-reviewed CI/CD configuration

---

### 🔹 Integration Mechanism 4: Test Framework Integration

Bamboo is deeply integrated with test runners and reporting frameworks:

|Framework|Language|Integration|
|---|---|---|
|**JUnit**|Java|Native test result parsing|
|**TestNG**|Java|XML result parsing|
|**Selenium**|Web UI|Browser-based test automation|
|**pytest**|Python|Test result import|
|**NUnit**|.NET|Test result parsing|
|**Mocha/Jest**|JavaScript|Node.js test reporting|
|**SonarQube**|All|Code quality, coverage analysis|
|**Clover**|Java|Code coverage reports|

After tests run, Bamboo:

1. Parses test result files (JUnit XML format)
2. Displays a **test results dashboard** per build
3. Highlights newly failing tests vs. pre-existing failures
4. Fails the stage if any test fails → downstream stages do not proceed

---

### 🔹 Integration Mechanism 5: Jira Software Integration

This is Bamboo's most powerful and distinctive integration:

**How Bamboo ↔ Jira works:**

```
Developer creates Jira issue (e.g., PROJ-42)
        ↓
Developer commits code referencing "PROJ-42: fix login bug"
        ↓
Bamboo detects commit → runs build → reports result to Jira
        ↓
Jira issue PROJ-42 now shows:
  - Which builds are linked to this issue
  - Build status (pass/fail)
  - Which deployment environment the fix is in
  - Release version the fix will ship in
```

**Bidirectional integration:**

- Jira issues can **trigger** Bamboo builds
- Bamboo builds automatically **update** Jira issues with status
- Provides **end-to-end traceability**: from requirement → code → build → test → deployment

---

### 🔹 Integration Mechanism 6: Deployment and Environment Management

Bamboo separates build plans from deployment projects:

```
Build Plan:   Code → Compile → Test → Artifact (JAR/WAR/Docker Image)
                                              ↓
Deployment Project:
  Artifact ──► DEV environment (auto-deploy on every build)
           ──► STAGING environment (auto-deploy if DEV passes)
           ──► PRODUCTION environment (manual approval gate)
```

**Deployment features:**

- **Per-environment permissions:** Different teams control different environments
- **Deployment history:** Full audit trail of what was deployed where and when
- **Rollback:** Redeploy any previous artifact with one click
- **Smoke tests:** Run automated health checks after each deployment

---

### 🔹 Integration Mechanism 7: Notification and Alerting

Bamboo integrates with communication platforms to immediately alert teams:

|Channel|Trigger|
|---|---|
|**Email**|Build pass/fail, new test failures|
|**Slack**|Build notifications via webhook|
|**HipChat**|Atlassian's native chat (legacy)|
|**Webhooks**|Any custom HTTP endpoint|
|**RSS feed**|Subscribe to build status updates|

**Custom notification rules:** Notify only the committer who broke the build, or alert the entire team when a critical plan fails.

---

### 🔹 Integration Mechanism 8: REST API Integration

Bamboo exposes a comprehensive **REST API** for programmatic integration:

```
GET  /rest/api/latest/result/{planKey}   → Get build results
POST /rest/api/latest/queue/{planKey}    → Trigger a build manually
GET  /rest/api/latest/deploy/project     → List deployment projects
POST /rest/api/latest/deploy/release     → Trigger a deployment
```

This allows integration with:

- Custom dashboards and monitoring tools
- External ticketing and ITSM systems
- ChatOps bots (trigger builds from Slack commands)
- Datadog, Grafana, and observability platforms

---

## 🔷 PART 4: Bamboo CI/CD Pipeline — End-to-End Flow

```
TIER 1: SOURCE CONTROL
  Developer → commits code → Bitbucket / Git / SVN
                                      ↓ webhook
TIER 2: BAMBOO CI SERVER
  Detects commit → determines affected Plan → queues build
                                      ↓ dispatches
TIER 3: BUILD AGENTS (parallel)
  Local Agent | Remote Agent | AWS Agent | Docker Agent
  Each runs: checkout → compile → unit test → package
                                      ↓ results
TIER 4: BUILD TOOLS & QUALITY
  Maven build | JUnit tests | SonarQube analysis | Artifact creation
                                      ↓ pass/fail
TIER 5: INTEGRATIONS & DELIVERY
  Jira updated | Deployment project triggered | Notifications sent
  ↓                    ↓                            ↓
  Issue tracking    DEV → STAGING → PROD         Email / Slack
```

---

## 🔷 PART 5: Bamboo vs Jenkins — Comparison

|Feature|Bamboo|Jenkins|
|---|---|---|
|**Cost**|Commercial (paid per agent)|Open-source (free)|
|**Setup**|Easy, minimal config|Requires significant setup|
|**Atlassian integration**|Native, first-class|Via plugins only|
|**Plugin ecosystem**|150+ Marketplace apps|1800+ plugins|
|**Config as code**|Bamboo Specs (YAML/Java)|Jenkinsfile (Groovy)|
|**UI**|Intuitive, polished|Functional, complex|
|**Branch detection**|Automatic|Manual or via plugin|
|**Deployment tracking**|Built-in deployment projects|Requires plugins|
|**Support**|Atlassian support team|Community only|

---

## 🔷 PART 6: Advantages and Disadvantages

### ✅ Advantages of Bamboo

- **Built-in functionality** — Minimal plugin dependency vs Jenkins
- **Native Atlassian ecosystem** — Tight Jira, Bitbucket, Confluence integration
- **Parallel builds** — Jobs run concurrently; faster feedback loops
- **Bamboo Specs** — Pipeline as code; stored in version control
- **Deployment projects** — Built-in CD support with environment control
- **Auto branch detection** — Automatically CIs new feature branches
- **Enterprise support** — Dedicated support team from Atlassian

### ❌ Disadvantages of Bamboo

- **Cost** — Commercial license; per-agent pricing can be expensive at scale
- **Vendor lock-in** — Deep Atlassian tool dependency
- **Smaller plugin ecosystem** — Fewer third-party integrations than Jenkins
- **Server edition EOL** — Bamboo Server reached End of Life on Feb 15, 2024; must migrate to Data Center
- **Less flexibility** — Opinionated structure (Project → Plan → Stage → Job → Task) limits non-standard workflows

---

## ⚡ QUICK REVISION — 1-4-7 Rule

### 1️⃣ One Line:

> Bamboo is Atlassian's CI/CD server that automates build-test-deploy pipelines through a five-level hierarchy (Project → Plan → Stage → Job → Task) and integrates natively with Git, Jira, Bitbucket, and all major build and test tools.

---

### 4️⃣ Four Key Points:

1. **Five-level hierarchy:** Project > Plan > Stage (sequential) > Job (parallel) > Task (sequential within Job)
2. **Four agent types:** Local, Remote, AWS Cloud, Docker — enable parallel and distributed builds
3. **Native Atlassian integration:** Bamboo + Bitbucket + Jira = full traceability from requirement to production
4. **Bamboo Specs:** Pipeline defined as code in YAML/Java DSL, stored in version control alongside source code

---

### 7️⃣ Seven Deep Points:

1. **CI mechanism:** Every commit triggers a Plan via webhook; Bamboo detects the affected branch automatically
2. **Stages are sequential:** Stage 2 (Test) only runs if Stage 1 (Build) passes — fail-fast philosophy
3. **Jobs within a stage are parallel:** Multiple agents run different jobs simultaneously for speed
4. **Jira bidirectional integration:** Jira issues trigger builds AND builds update Jira issue status automatically
5. **Bamboo Specs (config as code):** Allows pipeline configs to be peer-reviewed, versioned, and replicated
6. **Deployment projects** separate from build plans: DEV → STAGING → PROD with per-environment permission control
7. **REST API** enables full programmatic control: trigger builds, fetch results, trigger deployments from external tools

---

## 📝 3 Possible Exam Questions

1. **"Apply the Bamboo software development tool for integration mechanisms with a neat diagram."** _(15 Marks)_ ← This note answers this!
2. **"Compare Bamboo and Jenkins CI/CD tools. Which would you choose and why?"** _(7 Marks)_
3. **"Explain the Bamboo hierarchy (Project, Plan, Stage, Job, Task) with a suitable example."** _(10 Marks)_

---

_📁 Save in Obsidian: `Software Engineering > DevOps > Bamboo CI CD`_ _🏷️ Tags: #Bamboo #CICD #DevOps #Integration #Atlassian #15Marks #SoftwareEngineering #ContinuousIntegration_