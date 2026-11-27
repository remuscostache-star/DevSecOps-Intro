# Lab 4 — SBOM + SCA

## Task 1

### SBOM Inventory Comparison (4.1)
- **CycloneDX (`juice-shop.cdx.json`) component count:** `3069`
- **SPDX (`juice-shop.spdx.json`) package count:** `909`
- **CycloneDX `specVersion`:** `1.7`
- **Why CycloneDX and SPDX disagree:** CycloneDX is security-oriented and records every detected software artifact inside `.components` (including nested/transitive libraries, OS layers, runtime components, and individual files). SPDX separates installed software `.packages` (`909`) from individual `.files` in distinct top-level arrays, resulting in a much smaller package count for the exact same container image.

### Vulnerability Severity Breakdown (`Grype` on SBOM, 4.2)

| Severity | Count |
| :--- | :---: |
| Critical | 19 |
| High | 125 |
| Medium | 115 |
| Low | 27 |
| Negligible | 11 |
| Unknown | 2 |
| **Total** | **299** |

### Top 10 Ranked Findings (4.3)

```text
Critical	CVE-2026-111497	node@24.15.0	fix: 11.0.0,22.23.4,24.22.1,26.12.1
Critical	GHSA-c7hr-j4mj-j2w6	jsonwebtoken@0.1.0	fix: 4.2.2
Critical	GHSA-c7hr-j4mj-j2w6	jsonwebtoken@0.4.0	fix: 4.2.2
Critical	GHSA-jf85-cpcp-j695	lodash@2.4.2	fix: 4.17.12
Critical	CVE-2026-63073	libssl3t64@3.5.5-1~deb13u2	fix: 3.5.7-1~deb13u2
Critical	GHSA-hrh2-vp3x-79xf	decompress@4.2.1	fix: 
Critical	GHSA-8r5x-fm3f-whwj	handlebars@4.7.9	fix: 4.7.10
Critical	CVE-2026-5450	libc6@2.41-12+deb13u2	fix: 2.41-12+deb13u4
Critical	GHSA-mp2f-45pm-3cg9	decompress@4.2.1	fix: 
Critical	GHSA-xwcq-pm8m-c4vf	crypto-js@3.3.0	fix: 4.2.0
```

### Triage & Remediation Priority
- **Findings with an available fix:** **8 out of 10** have a patched version listed (`node`, `jsonwebtoken@0.1.0`, `jsonwebtoken@0.4.0`, `lodash@2.4.2`, `libssl3t64`, `handlebars`, `libc6`, and `crypto-js`), while **2 out of 10** (`decompress@4.2.1`) have no upstream fix available.
- **First action based on severity and fix availability:** Immediately upgrade the **8 `Critical` dependencies that already have fixes** (starting with internet-facing runtime and authentication packages like `node`, `jsonwebtoken`, and `libssl3t64`) to achieve rapid risk reduction, while applying compensating controls or replacing the unmaintained `decompress@4.2.1` package where no patch exists.
