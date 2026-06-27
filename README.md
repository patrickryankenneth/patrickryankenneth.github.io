# Patrick Ryan

Supply chain and procurement professional with nearly 10 years of experience across purchasing, vendor operations, and enterprise procurement environments. I started automating work in 2016 with Excel VBA macros that cut order upload time from about an hour to ten minutes. At the time, I did not think of it as programming. I thought of it as fixing something that was too slow.

That pattern kept repeating.

In early 2025, a work laptop froze during a large Excel PowerQuery join. Not just slow — full GUI lockup, daily crashes, and no viable path forward inside the tool. That was the point where I moved seriously into Python, tested WSL against native Linux, started profiling the difference, and began working down the stack.

Excel/VBA → Python → Pandas → Polars → Parquet → ramdisk I/O → Linux → syscall profiling.

My focus is data engineering, pipeline performance, automation, and understanding what actually happens underneath high-level abstractions.

---

## Engineering Focus

The production systems I build at work are confidential, but the engineering patterns are transferable:

* Multi-source ingestion from SQL, enterprise systems with no API, email attachments, browser automation, and cloud object storage.
* Data transformation pipelines with explicit stage contracts, checkpointing, cache invalidation, and restart-safe scheduling.
* Fuzzy matching, confidence scoring, hidden join-key discovery, and cross-system record linkage.
* Parquet over CSV for columnar access patterns.
* Ramdisk-backed intermediate I/O to avoid unnecessary disk writes during pipeline execution.
* Vectorized operations instead of row-wise loops.
* Stage-level timing, bottleneck isolation, and before/after performance measurement.
* Automated publishing to Excel workbooks, dashboards, and downstream consumers.

A representative production pipeline I built runs daily with no manual intervention, processes hundreds of thousands of records across multiple source systems, and reduced manual task resolution from roughly 30+ minutes to 2–3 minutes per item.

---

## Public Projects

### [civic-data-pipeline](https://github.com/patrickryankenneth/civic-data-pipeline)

Extracts municipal procurement data from a City of Tempe PowerBI Gov dashboard with no public export path.

The project reverse-engineers the DSR binary protocol used by the dashboard, including bitmask row reconstruction, ValueDict index resolution, and millisecond timestamp conversion. It extracts 9,502 contract records across 928 vendors representing approximately $23B in estimated contract value, then cross-references vendors against USASpending.gov federal award data.

No API access was available. The DSR decode was the only practical programmatic path.

This project demonstrates a core engineering belief: missing infrastructure is a constraint, not a blocker.

---

## LeetCode

I use LeetCode problems as a benchmarking sandbox. When a problem has a one-line solution, I want to know what that line actually costs.

Recent example: [From 4 Syscalls to 8,517 — What Doubling a Column Actually Costs Across the Stack](https://leetcode.com/problems/modify-columns/solutions/8360593/from-4-syscalls-to-8517-what-doubling-a-b1euy)

For a simple Pandas column update, I benchmarked multiple Pandas strategies plus NumPy buffer mutation, ASM, C++, Rust, and Polars. I separated warm in-memory execution from cold-start syscall surface using randomized round-robin timing and `strace`.

Key findings:

* Direct assignment was the fastest standard Pandas API.
* `*= 2` was slower than `= col * 2` in this benchmark.
* `.loc[]`, `.apply(lambda)`, `.iloc` loops, and `.iterrows()` all added measurable overhead.
* `.to_numpy(copy=False)[:] *= 2` bypassed most Pandas assignment machinery and was the fastest Pandas-backed path.
* Pandas’ cold-start syscall surface was dramatically larger than native ASM/C++/Rust binaries.

Profile: [leetcode.com/u/tUhGYEF4fl](https://leetcode.com/u/tUhGYEF4fl/)

---

## Stack

**Languages:** Python, SQL, Bash, C++ learning, Rust learning
**Data:** Pandas, Polars, NumPy, PyArrow
**Pipeline:** systemd, ramdisk I/O, CDC watermarking, Parquet, stage profiling
**Automation:** Playwright, COM automation
**Infra:** Linux, conda/miniforge, cloud object storage

---

## Certifications & Education

* AWS Certified AI Practitioner — Dec 2025
* AWS Certified Data Engineer Associate — in progress
* Springboard Data Analytics Career Program — Jun 2026
* MITx MicroMasters, Supply Chain Management — 2018–2019
* Master of Science, International Business — Hult International Business School, 2018
* Bachelor of Applied Science, Supply Chain Management — Broward College, 2017

**LinkedIn:** [patrickryankenneth](https://www.linkedin.com/in/patrickryankenneth/)
