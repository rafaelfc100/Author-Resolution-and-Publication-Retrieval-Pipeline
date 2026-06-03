# Author Resolution and Publication Retrieval Pipeline

This project implements a complete Author Resolution pipeline capable of identifying researchers from name variants or ORCID identifiers and retrieving their scientific publications from multiple academic data sources.

The system combines text normalization, string similarity, clustering, and API integration to solve one of the most common problems in scholarly data analysis: author name ambiguity.

## Project Objectives

* Resolve author identities from name variants.
* Detect and merge different representations of the same researcher.
* Retrieve publications from academic APIs.
* Normalize publication metadata.
* Remove duplicate records.
* Generate structured outputs ready for NLP and citation analysis tasks.

## Problem Statement

Researchers often appear under different name formats across databases:

* Rafael Frias Cortez
* Rafael A. Frias Cortez
* R. A. Frias-Cortez
* Frias Cortez, Rafael
* Rafael Frias

Although these records may refer to the same person, traditional searches treat them as different authors.

This project aims to automatically identify and unify these variants.

## Pipeline Overview

### 1. Author Variant Dataset Creation

A dataset of real-world author name variations is created.

Example:

| Author ID | Name Variant           |
| --------- | ---------------------- |
| A001      | Rafael Frias Cortez    |
| A001      | Rafael A. Frias Cortez |
| A001      | R. A. Frias-Cortez     |
| A001      | Frias Cortez, Rafael   |

## 2. Name Normalization

Each name is standardized by applying:

* Lowercase conversion
* Accent removal
* Punctuation removal
* Whitespace normalization

Example:

```text
Rafael Á. Frías-Cortez
↓
rafael a frias cortez
```

## 3. Name Tokenization

Names are decomposed into:

* First names
* Last names
* Initials

The system also handles incomplete author names.

Example:

```text
rafael a frias cortez
→ [rafael, a, frias, cortez]
```

## 4. Canonical Representation

A canonical representation is generated using predefined rules.

Example:

```text
Frias Cortez + RA
→ frias_cortez_ra
```

This representation becomes the internal identifier used during matching.

## 5. Similarity Computation

The system calculates similarity between names using:

* Levenshtein Distance
* String similarity scores
* Threshold-based matching

Example:

```text
rafael frias cortez
rafael a frias cortez

Similarity = 0.94
```

## 6. Author Clustering

All author variants are compared and grouped automatically.

Output:

```text
Cluster 001
├── Rafael Frias Cortez
├── Rafael A. Frias Cortez
├── R. A. Frias-Cortez
└── Frias Cortez, Rafael
```

Each cluster receives a unique author identifier.

## 7. Author Resolution

Input can be:

### Name

```text
Input:
Rafael A Frias

Output:
Author ID: A001
```

### ORCID

```text
Input:
0000-0002-XXXX-XXXX

Output:
Direct author resolution
```

## 8. Publication Retrieval

The resolved author is searched across academic APIs:

* ORCID API
* OpenAlex API
* CrossRef API

Retrieved information includes:

* Publications
* Author lists
* Citation metadata
* Research identifiers

## 9. Publication Parsing

Each publication is converted into a standardized structure.

Example:

```json
{
  "title": "Author Name Resolution using NLP",
  "authors": ["Rafael Frias Cortez"],
  "year": 2025,
  "doi": "10.xxxx/abcd",
  "abstract": "..."
}
```

## 10. Author Normalization inside Publications

All publication authors are normalized using the same canonical representation.

This guarantees consistency across records and databases.

## 11. Duplicate Removal

Duplicate publications are detected through:

### DOI Matching

```text
10.1000/xyz123
==
10.1000/xyz123
```

### Title Similarity

Used when DOI information is missing.

Near-duplicate titles are automatically merged.

## 12. Unified Pipeline

Final function:

```python
resolve_author(query)
```

Input:

```text
"Rafael Frias Cortez"
```

or

```text
"0000-0002-XXXX-XXXX"
```

Output:

```json
{
  "author": {...},
  "variants": [...],
  "publications": [...]
}
```

## Technologies

* Python
* Pandas
* NLTK
* RapidFuzz / Levenshtein
* Requests
* ORCID API
* OpenAlex API
* CrossRef API
* JSON

## Applications

This project can be used for:

* Bibliometric analysis
* Citation extraction
* Research profiling
* Academic search engines
* Knowledge graph construction
* Scientific author disambiguation
* Research analytics systems

## Future Improvements

* Transformer-based author matching.
* Semantic similarity using embeddings.
* Institution-aware disambiguation.
* Co-author network analysis.
* Citation network construction.
* Integration with Semantic Scholar and Scopus.


