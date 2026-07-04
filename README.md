# NXRADAR CycloneDX Property Taxonomy

This document defines the official property namespace taxonomy for **`nxradar`**, maintained by
[CyberNX Technologies Pvt. Ltd.](https://www.cybernx.com), the makers of **NXRadar** — an SBOM
management and compliance platform.

The `nxradar` namespace is registered in the
[CycloneDX Property Taxonomy](https://github.com/CycloneDX/cyclonedx-property-taxonomy).

---

## Purpose

The CycloneDX specification natively supports a core set of component-level fields. However,
regulatory frameworks — specifically the
[CERT-In Technical Guidelines on SBOM (Version 2.0, dated 9 July 2025)](https://www.cert-in.org.in)
issued by the Indian Computer Emergency Response Team — prescribe 21 minimum data fields per
component, several of which have no native representation in the CycloneDX schema.

This namespace defines standardised, machine-parseable property names and value formats for
those CERT-In-prescribed fields that are **not natively supported** by CycloneDX, so that:

- Regulated Entities (REs) under RBI, SEBI, and other Indian regulators can produce
  CERT-In-compliant SBOMs in CycloneDX format without ambiguity.
- Vendors supplying SBOMs to REs use a consistent, interoperable naming convention.
- SBOM-management and validation tooling can reliably parse and cross-validate these fields
  across SBOMs from different sources.

---

## Namespace Rules

- All properties in this namespace use the prefix `nxradar:`.
- Property names are lowercase, using underscores as word separators.
- All properties are **optional** at the schema level but may be **required** by applicable
  regulatory frameworks (see CERT-In guidelines Section 7.1.4).
- Unless stated otherwise, each property **MAY appear once** per component.
- Where data is genuinely unavailable from any source, the value `NOT-AVAILABLE` **SHOULD** be
  used to distinguish a documented data gap from an accidental omission.

---

## Property Definitions

### 1. `nxradar:criticality`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 12 — Criticality |
| **CycloneDX native equivalent** | None |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
The criticality or importance of the software component to the overall functionality or security
of the application. This is an organisational assessment made by the consuming/operating entity
based on how the component is deployed in their environment.

**Allowed values:**

| Value | Meaning |
|-------|---------|
| `critical` | Failure or compromise of this component directly impacts core business operations or security |
| `high` | Significant impact on functionality or security if compromised |
| `medium` | Moderate impact; other controls exist to reduce exposure |
| `low` | Minimal impact; easily replaceable or isolated |
| `NOT-AVAILABLE` | Criticality assessment has not yet been performed or is not determinable |

**Example:**
```json
{ "name": "nxradar:criticality", "value": "high" }
```

---

### 2. `nxradar:usage_restrictions`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 13 — Usage Restrictions |
| **CycloneDX native equivalent** | None (licenses cover open-source terms; this covers export control, IP, and contractual restrictions beyond license terms) |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
Any usage restrictions or limitations associated with the software component beyond its open-source
or commercial license terms. Includes export control classifications, intellectual property
restrictions, contractual usage limitations, or deployment environment constraints.

**Value format:** Free text string. Use `NOT-AVAILABLE` if no restriction information is available
or determinable.

**Examples:**
```json
{ "name": "nxradar:usage_restrictions", "value": "Export controlled - EAR99" }
{ "name": "nxradar:usage_restrictions", "value": "Licensed for internal use only; redistribution prohibited" }
{ "name": "nxradar:usage_restrictions", "value": "None" }
{ "name": "nxradar:usage_restrictions", "value": "NOT-AVAILABLE" }
```

---

### 3. `nxradar:eol_date`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 11 — End-of-Life (EOL) Date |
| **CycloneDX native equivalent** | None |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
The date when vendor support or maintenance for the software component is scheduled to end,
indicating the end of its active lifecycle. After this date, security patches and updates will
no longer be provided by the supplier.

**Value format:** ISO 8601 date — `YYYY-MM-DD`. Use `NOT-AVAILABLE` if the EOL date is
not published or determinable from any source.

**Examples:**
```json
{ "name": "nxradar:eol_date", "value": "2026-12-31" }
{ "name": "nxradar:eol_date", "value": "NOT-AVAILABLE" }
```

---

### 4. `nxradar:release_date`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 10 — Release Date |
| **CycloneDX native equivalent** | None at component level (metadata timestamp exists at document level only) |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
The date when this specific version of the software component was publicly released or made
available by its supplier. Distinct from the SBOM document timestamp, which records when the
SBOM itself was generated.

**Value format:** ISO 8601 date — `YYYY-MM-DD`. Use `NOT-AVAILABLE` if the release date is
not published or determinable.

**Examples:**
```json
{ "name": "nxradar:release_date", "value": "2022-01-10" }
{ "name": "nxradar:release_date", "value": "NOT-AVAILABLE" }
```

---

### 5. `nxradar:patch_status`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 9 — Patch Status |
| **CycloneDX native equivalent** | None as a standalone field (vulnerability data exists in VEX/vulnerabilities section but patch status as a single component-level summary is not native) |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
The current patch or update status of the software component, indicating whether all known
vulnerabilities and issues have been addressed through available patches. This is a point-in-time
status that should be updated whenever the component's patch posture changes.

**Allowed values:**

| Value | Meaning |
|-------|---------|
| `up-to-date` | All known patches have been applied; no outstanding vulnerabilities requiring patching |
| `patch-available` | One or more patches exist but have not yet been applied |
| `no-patch-available` | Known vulnerability exists but no vendor patch has been released |
| `end-of-life` | Component is past EOL; patches are no longer being issued by the vendor |
| `under-review` | Patch applicability is currently being assessed |
| `NOT-AVAILABLE` | Patch status cannot be determined |

**Example:**
```json
{ "name": "nxradar:patch_status", "value": "up-to-date" }
```

---

### 6. `nxradar:comments`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 15 — Comments or Notes |
| **CycloneDX native equivalent** | None as a distinct annotation field (`description` serves a different purpose — it describes the component's functionality, not operational notes about its inclusion) |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
Additional comments, notes, or annotations relevant to this software component's inclusion in the
SBOM, its operational context, or any observations the SBOM author wishes to record. Distinct
from `description`, which describes what the component does.

**Value format:** Free text string. Each note should be a standalone observation. Use
`NOT-AVAILABLE` if no notes are applicable.

**Examples:**
```json
{ "name": "nxradar:comments", "value": "Component used only in non-production environments" }
{ "name": "nxradar:comments", "value": "Pending replacement with internal implementation in Q3 2026" }
{ "name": "nxradar:comments", "value": "NOT-AVAILABLE" }
```

---

### 7. `nxradar:executable_property`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 18 — Executable Property |
| **CycloneDX native equivalent** | None as a simple boolean; CycloneDX `type: file` with `fileTypes` exists but is not equivalent to a component-level executable flag across all component types |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
Indicates whether the component contains or constitutes an executable binary — i.e., whether it
can be directly executed by an operating system or runtime environment. This includes `.exe`,
`.sh`, `.bat`, `.elf`, and similar executable artefacts.

**Allowed values:**

| Value | Meaning |
|-------|---------|
| `true` | Component contains or constitutes an executable binary |
| `false` | Component does not contain an executable binary (e.g., a pure library, data file, or configuration) |
| `NOT-AVAILABLE` | Cannot be determined |

**Example:**
```json
{ "name": "nxradar:executable_property", "value": "true" }
```

---

### 8. `nxradar:archive_property`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 19 — Archive Property |
| **CycloneDX native equivalent** | None as a simple boolean |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
Indicates whether the component is distributed as or stored within an archive or compressed file
format, such as `.zip`, `.tar`, `.gz`, `.jar`, `.war`, `.ear`, or similar.

**Allowed values:**

| Value | Meaning |
|-------|---------|
| `true` | Component is distributed as or within an archive/compressed file |
| `false` | Component is not an archive |
| `NOT-AVAILABLE` | Cannot be determined |

**Example:**
```json
{ "name": "nxradar:archive_property", "value": "true" }
```

---

### 9. `nxradar:structured_property`

| Attribute     | Value |
|---------------|-------|
| **Maps to**   | CERT-In Section 4.2, Field 20 — Structured Property |
| **CycloneDX native equivalent** | None |
| **Attached to** | `component` |
| **Cardinality** | MAY appear once |

**Description:**  
Indicates whether the component contains or produces structured data output, such as defined
schemas, configuration files with structured formats (XML, JSON, YAML), or API contracts.
This helps consuming organisations understand whether the component's outputs require schema
validation or structured parsing.

**Allowed values:**

| Value | Meaning |
|-------|---------|
| `true` | Component contains or produces structured data with a defined format or schema |
| `false` | Component does not produce structured data output |
| `NOT-AVAILABLE` | Cannot be determined |

**Example:**
```json
{ "name": "nxradar:structured_property", "value": "true" }
```

---

## Complete CycloneDX JSON Example

The following illustrates all nine `nxradar` properties applied to a single component in a
CycloneDX SBOM:

```json
{
  "type": "library",
  "name": "Apache Tomcat",
  "version": "9.0.41",
  "supplier": { "name": "Apache Software Foundation" },
  "purl": "pkg:maven/org.apache.tomcat/tomcat@9.0.41",
  "hashes": [
    { "alg": "SHA-256", "content": "7f87a8b8ed5c23546789b8d7586219a1..." }
  ],
  "properties": [
    { "name": "nxradar:criticality",         "value": "high" },
    { "name": "nxradar:usage_restrictions",  "value": "None" },
    { "name": "nxradar:eol_date",            "value": "2025-03-22" },
    { "name": "nxradar:release_date",        "value": "2021-01-07" },
    { "name": "nxradar:patch_status",        "value": "up-to-date" },
    { "name": "nxradar:comments",            "value": "Web server for all production deployments" },
    { "name": "nxradar:executable_property", "value": "true" },
    { "name": "nxradar:archive_property",    "value": "false" },
    { "name": "nxradar:structured_property", "value": "true" }
  ]
}
```

---

## CERT-In Field Coverage Summary

| CERT-In Field (Section 4.2) | CycloneDX Native Field | nxradar Property |
|-----------------------------|------------------------|------------------|
| 1. Component Name | `name` | — |
| 2. Component Version | `version` | — |
| 3. Component Description | `description` | — |
| 4. Component Supplier | `supplier` | — |
| 5. Component License | `licenses` | — |
| 6. Component Origin | `purl`, `externalReferences` | — |
| 7. Component Dependencies | `dependencies` | — |
| 8. Vulnerabilities | `vulnerabilities` (VEX) | — |
| 9. Patch Status | — | `nxradar:patch_status` |
| 10. Release Date | — | `nxradar:release_date` |
| 11. End-of-Life (EOL) Date | — | `nxradar:eol_date` |
| 12. Criticality | — | `nxradar:criticality` |
| 13. Usage Restrictions | — | `nxradar:usage_restrictions` |
| 14. Checksums or Hashes | `hashes` | — |
| 15. Comments or Notes | — | `nxradar:comments` |
| 16. Author of SBOM Data | `metadata.authors` | — |
| 17. Timestamp | `metadata.timestamp` | — |
| 18. Executable Property | — | `nxradar:executable_property` |
| 19. Archive Property | — | `nxradar:archive_property` |
| 20. Structured Property | — | `nxradar:structured_property` |
| 21. Unique Identifier | `purl`, `cpe`, `bom-ref` | — |

---

## Versioning and Maintenance

This taxonomy is maintained by CyberNX Technologies Pvt. Ltd.  
For questions, corrections, or additions, open an issue at:  
**https://github.com/orgs/cybernxtech/cyclonedx-property-taxonomy/issues**

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-07-09 | Initial release — 9 properties covering CERT-In v2.0 non-native fields |