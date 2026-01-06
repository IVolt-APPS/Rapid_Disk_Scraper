# IVolt RapidScan User Manual

**Version:** 1.0  
**Release Time:** 1700 CST — 04-19-2025  
**Platform:** Windows x64  
**Interface:** Command Line (CLI)

---

## Overview

**IVolt RapidScan** is a high-performance disk and directory scanning utility designed to enumerate files and folders at extremely high speed. It recursively scans one or more paths and exports detailed metadata to CSV files.

Typical use cases include:
- Fast inventory of disks or directories
- Auditing file structures
- Large-scale file analysis and indexing
- Performance benchmarking of storage devices

---

## Key Features

- Recursive file and directory scanning
- Multiple input paths per run
- UTF-8 encoded CSV output
- Extremely high throughput (tested near 82,000 entries/sec)
- Optional high-resolution execution timing
- Efficient Windows filesystem API usage

---

## System Requirements

- Windows 7 or later
- NTFS or compatible filesystem
- Command Prompt, PowerShell, or terminal environment
- Write access to the output directory

---

## Command Line Syntax

```text
IVolt_RapidScan.exe [-timeit] -paths <path1,path2,...> -out <output_directory>

![image](https://github.com/user-attachments/assets/15cf03fa-e171-4c3d-aace-e529186d5c78)


## Purpose “Advanced AI File Mapping”

IVolt_RapidScan is a Windows x64 CLI utility that recursively enumerates files and folders at very high speed and exports the results as UTF-8 CSV metadata. This is the ideal “ground-truth snapshot” layer for any AI system that needs to understand what’s on a drive before it can classify, reorganize, govern, or optimize it.

Key characteristics that matter for AI mapping:

* **High-throughput enumeration** (tested near **82,000 entries/sec**) so you can scan large drives frequently enough to stay current.
* **Multi-root scanning** in one run (multiple paths per execution).
* **Deterministic CSV output** suitable for indexing, diffing, and model input pipelines.
* **Optional timing mode** (`-timeit`) so you can baseline storage performance and detect regressions or bottlenecks. 
* It’s positioned explicitly for **inventory, auditing, analysis/indexing, and benchmarking** use cases. 
* The repo indicates an **“Upgraded Perf. 1.2.0.0 Full Release” dated Dec 27, 2025**, which suggests ongoing performance iteration.

## How to use it for Advanced AI File Mapping and Organization

The practical pattern is:

### 1) Scan → produce a “drive facts table”

Run the tool against one or more roots and write to an output directory:

* `IVolt_RapidScan.exe [-timeit] -paths <path1,path2,...> -out <output_directory>` 

This yields CSV(s) that become your canonical dataset for that snapshot in time.

### 2) Convert CSV → a structure the AI can reason over

Typical post-processing steps (deterministic, not “AI” yet):

* Normalize paths, split into directory segments, compute depth.
* Derive features per file/folder: extension, “likely type” buckets, size bands, age bands (if timestamps are included), parent-child relationships.
* Build a **graph representation**:

  * Nodes: folders, files
  * Edges: `CONTAINS`, plus inferred edges like `LIKELY_RELATED_TO`, `DUPLICATE_OF`, `DERIVED_FROM`

### 3) Apply AI to label, cluster, and recommend actions

Once you have the facts table/graph, AI can do higher-level tasks reliably:

**A. Semantic labeling (organization layer)**

* Predict “domain categories” per subtree (Finance, Legal, Source, Media, CAD, HR, etc.) using:

  * folder names
  * file extensions
  * co-location patterns
  * (optional) content sampling later (only for high-value candidates)

**B. Clustering and “project boundary detection”**

* Identify natural project folders by:

  * dense file activity regions
  * consistent naming patterns
  * repeated structures (e.g., `src/`, `docs/`, `assets/`)

**C. Rule generation**

* Create enforceable rules like:

  * “All invoices should live under `Finance/Invoices/<Year>/`”
  * “All installers/binaries should go to `Software/Distributions/`”
  * “All large media files > X GB must be moved to archival storage”

**D. Reorganization planning (safe mode)**

* Output a **proposed move plan** (CSV/JSON):

  * old path → new path
  * confidence score
  * reason codes
* Run a “no-op” simulation first (counts, conflicts, collisions), then implement.

### 4) Continuous snapshots → diffs → policy enforcement

Re-scan on a schedule:

* Compute diffs (new/moved/deleted) from snapshot to snapshot.
* Detect drift from desired structure.
* Trigger workflows: notify, quarantine, archive, backup changes.

## Benefits of analyzing a drive’s structure and contents (what AI can unlock)

Below are the major benefit categories an AI system can deliver once it has complete, fast, repeatable enumeration (which is what RapidScan is built for). 

### 1) Findability and productivity

* Unified “inventory index” of everything on disk
* Faster search (even without content indexing) using metadata + structure
* Automatic “where should this go?” suggestions at save time
* Better onboarding (“here are the canonical project locations and where your files belong”)

### 2) Storage optimization and cost reduction

* Duplicate and near-duplicate detection (by name/size/path patterns; optionally hash in a follow-on step)
* Identify cold data suitable for cheaper storage tiers
* Detect runaway folders (log dumps, caches, accidental copies)
* Capacity forecasting by growth rate per subtree

### 3) Security hardening

* Detect suspicious patterns:

  * unusual extension clusters (e.g., mass renames)
  * sudden explosions in file count
  * anomalous directory depth
* Identify risky locations:

  * executables in user document trees
  * credentials-like filenames in unsafe places
* Create “high value target” maps (where critical business data concentrates) for prioritizing protections

### 4) Governance, compliance, and defensibility

* Data classification and retention mapping by location/pattern
* Audit-friendly reporting:

  * what exists, where it is, how it’s organized
* Enforce storage policies:

  * “PII must be in encrypted volumes”
  * “Legal holds apply to these subtrees”
* Support eDiscovery prep by identifying likely repositories of relevant material

### 5) Backup and disaster recovery improvements

* Smarter backup selection:

  * prioritize business-critical subtrees
  * exclude noise (caches/temp/build artifacts)
* Detect when critical folders are not being backed up (structure-based coverage checks)
* Improve restore workflows (restore order by dependency importance)

### 6) Data hygiene and lifecycle management

* Identify stale subtrees for archival or deletion review
* Normalize naming conventions (title casing, date formats, project codes)
* Detect “orphan” files (no clear project association) and recommend homes

### 7) Performance benchmarking and anomaly detection

Because RapidScan supports optional timing and is intended for benchmarking storage devices :

* Track scan time trends per volume and per subtree
* Detect filesystem performance degradation (fragmentation-like symptoms, failing drives, network share slowdowns)
* Compare storage solutions empirically (HDD vs SSD vs NAS paths)

### 8) Building higher-level “drive intelligence” products

With repeated scans you can build:

* A “Drive Map” UI (treemap, sunburst, graph)
* A recommendation engine (“clean this”, “archive that”, “reorganize here”)
* Automated workflows (tickets, alerts, scheduled reorganizations)
* A foundation for content-level AI (selective deep indexing only where it matters)

## A concrete “AI File Mapping” architecture pattern using RapidScan

* **RapidScan** produces CSV snapshots (fast, frequent). 
* **Ingest service** loads snapshots into SQL (or parquet) and builds a folder/file graph.
* **Feature builder** derives structure features + heuristics (extension buckets, depth, entropy, growth rate).
* **AI layer**:

* classifier for folder purpose
* clustering for project grouping
* planner for move/rename proposals (with confidence + rollback plan)
* **Policy engine** enforces rules and flags drift.
* **UI** shows: structure map, top offenders, duplicate candidates, archive candidates, compliance coverage.


