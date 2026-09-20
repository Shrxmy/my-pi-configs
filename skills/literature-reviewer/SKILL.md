---
name: "literature-reviewer"
description: "Conduct comprehensive Review of Related Literature (RLL) searches. Use when researching academic papers, finding peer-reviewed sources, building bibliographies, or writing literature reviews. Covers Google Scholar, PubMed, arXiv, citation search, and proper academic referencing formats."
compatibility: pi
---

# Literature Reviewer

Conduct comprehensive, scientifically rigorous literature reviews with properly cited sources.

## Literature Review Workflow

```
1. Define Research Question → 2. Search Databases → 3. Screen & Select → 4. Extract & Synthesize → 5. Write & Cite
```

---

## 1. Define Your Research Question

### PICO Framework (Clinical/Health Sciences)

| Element | Description | Example |
|---------|-------------|---------|
| **P**opulation | Who/what is studied? | Adults with type 2 diabetes |
| **I**ntervention | What is the treatment? | Metformin therapy |
| **C**omparison | What is the control? | Placebo or standard care |
| **O**utcome | What is measured? | HbA1c reduction |

### SPIDER Framework (Qualitative/Mixed Methods)

| Element | Description |
|---------|-------------|
| **S**ample | Who participates? |
| **P**henomenon of Interest | What is studied? |
| **I**ntervention | What is done? |
| **D**esign | How is it studied? |
| **E**valuation | What is measured? |
| **R**esearch Type | Qualitative, quantitative, mixed? |

### Research Question Templates

```markdown
## Quantitative
- "What is the effect of [X] on [Y] in [population]?"
- "How does [X] compare to [Y] for [outcome] in [population]?"

## Qualitative
- "How do [population] experience/perceive [phenomenon]?"
- "What are the experiences of [population] with [issue]?"

## Systematic Review
- "What is the effectiveness of [intervention] for [condition]?"
```

---

## 2. Database Search Strategy

### Primary Academic Databases

| Database | Best For | Access |
|----------|----------|--------|
| **Google Scholar** | Multi-disciplinary, citations | Free |
| **PubMed** | Biomedical, health sciences | Free |
| **arXiv** | Physics, math, CS, biology | Free |
| **IEEE Xplore** | Engineering, computer science | Subscription |
| **ScienceDirect** | All sciences | Subscription |
| **JSTOR** | Humanities, social sciences | Subscription |
| **Scopus** | Multi-disciplinary | Subscription |
| **Web of Science** | Multi-disciplinary, citations | Subscription |
| **Cochrane Library** | Systematic reviews, health | Free/Trial |
| **PsycINFO** | Psychology, behavioral sciences | Subscription |

### Search Query Construction

```python
# Boolean search operators
operators = """
AND     - Both terms must be present
OR      - Either term can be present  
NOT     - Exclude term

Examples:
"machine learning" AND "healthcare"     # Both terms
diabetes AND (treatment OR therapy)    # Grouping
"deep learning" NOT image              # Exclusion

Truncation: diabet* → diabetes, diabetic, diabetics
Phrases: "artificial intelligence"      # Exact phrase
"""
```

### Advanced Search Examples

```python
# PubMed-style search
pubmed_query = """
(
    "machine learning"[Title/Abstract] OR 
    "deep learning"[Title/Abstract] OR 
    "neural network"[Title/Abstract]
)
AND 
(
    "diagnos*"[Title/Abstract] OR 
    "predict*"[Title/Abstract] OR 
    "detection"[Title/Abstract]
)
AND 
(
    "cancer"[Title/Abstract] OR 
    "tumor"[Title/Abstract] OR 
    "carcinoma"[Title/Abstract]
)
AND 
("2019:2024"[dp])  # Date range filter
"""

# Google Scholar search
scholar_query = """
"systematic review" machine learning healthcare 
site:scholar.google.com OR site:pubmed.ncbi.nlm.nih.gov
"""
```

---

## 3. Source Evaluation Criteria

### CRAAP Test

| Criterion | Questions to Ask |
|-----------|------------------|
| **C**urrency | When was it published? Is it current enough? |
| **R**elevance | How does it relate to your question? |
| **A**uthority | Who wrote it? Credentials? Affiliation? |
| **A**ccuracy | Peer-reviewed? Sources cited? |
| **P**urpose | Why was it published? Objectivity? |

### Source Hierarchy

```
┌─────────────────────────────────────────┐
│           SYSTEMATIC REVIEWS            │  ← Most reliable
│        (Cochrane, meta-analyses)        │
├─────────────────────────────────────────┤
│           RANDOMIZED CONTROL            │
│              TRIALS (RCTs)              │
├─────────────────────────────────────────┤
│          COHORT STUDIES /               │
│          CASE CONTROL STUDIES           │
├─────────────────────────────────────────┤
│          CASE REPORTS /                 │
│            SERIES                        │
├─────────────────────────────────────────┤
│           EXPERT OPINION                │
│        (when well-reasoned)            │
├─────────────────────────────────────────┤
│           EDITORIALS /                   │
│           OPINIONS                       │  ← Least reliable
└─────────────────────────────────────────┘
```

### Evaluating Web Sources

```python
def evaluate_source(url, metadata):
    """
    Quick evaluation checklist for sources.
    """
    checklist = {
        'is_peer_reviewed': False,
        'has_doi': False,
        'has_proper_citation': False,
        'recent_publication': False,
        'credible_journal': False,
    }
    
    # Check for DOI (most scientific papers have DOI)
    if metadata.get('doi'):
        checklist['has_doi'] = True
        checklist['is_peer_reviewed'] = True
    
    # Check publication date
    if metadata.get('year'):
        checklist['recent_publication'] = int(metadata['year']) >= 2019
    
    # Check journal credibility
    credible_domains = [
        'nature.com', 'science.org', 'springer.com',
        'wiley.com', 'ieee.org', 'acm.org',
        'plos.org', 'bmj.com', 'nejm.org',
        'pubmed.ncbi.nlm.nih.gov', 'scholar.google.com'
    ]
    checklist['credible_journal'] = any(
        domain in url for domain in credible_domains
    )
    
    return checklist
```

---

## 4. Citation Formats

### APA 7th Edition

```python
# Journal Article
apa_article = """
Author, A. A., & Author, B. B. (Year). Title of article.
Journal Name, Volume(Issue), pages. https://doi.org/xxxxx

Example:
Smith, J. A., & Johnson, M. B. (2023). Machine learning in healthcare:
A systematic review. Journal of Medical Systems, 47(3), 45-62.
https://doi.org/10.1007/s10916-023-01234-5
"""

# Book
apa_book = """
Author, A. A. (Year). Title of work: Capital letter also for subtitle.
Publisher. https://doi.org/xxxxx (if available)

Example:
Williams, R. (2022). Research methods in social science.
Oxford University Press.
"""

# Website
apa_website = """
Author, A. A. or Organization Name. (Year, Month Day). Title of page.
Site Name. URL

Example:
World Health Organization. (2023, March 15). Diabetes factsheet.
https://www.who.int/news-room/fact-sheets/detail/diabetes
"""
```

### IEEE Format

```python
# Journal Article
ieee_article = """
[#] A. A. Author, "Title of article," Journal Name, vol. X, no. X, 
pp. XX-XX, Month Year. [Online]. Available: https://doi.org/xxxxx

Example:
[1] J. A. Smith and M. B. Johnson, "Deep learning for medical imaging,"
IEEE Trans. Med. Imag., vol. 42, no. 1, pp. 123-145, Jan. 2023.
```

### MLA 9th Edition

```python
# Journal Article
mla_article = """
Author Last, First. "Article Title." Journal Name, vol. X, no. X, 
Year, pp. XX-XX. Database Name. Accessed Day Month Year.

Example:
Smith, John A. "Machine Learning in Cancer Detection." Nature Medicine,
vol. 29, no. 4, 2023, pp. 345-367. PubMed. Accessed 15 Mar. 2024.
"""
```

### Chicago/Turabian (Notes-Bibliography)

```python
# Book
chicago_book = """
Author Last, First. Title of Book: Subtitle. Place of Publication: 
Publisher, Year.

Example:
Williams, Robert. Research Methods in Social Sciences. Chicago: 
University of Chicago Press, 2022.
"""

# Journal Article
chicago_article = """
Author Last, First. "Article Title." Journal Name Volume, no. Issue 
(Year): pages. DOI or URL.

Example:
Johnson, Mary. "Neural Networks in Healthcare." New England Journal 
of Medicine 389, no. 2 (2023): 123-145. https://doi.org/10.1056/NEJM1234567.
"""
```

---

## 5. Literature Review Structure

### Traditional Literature Review

```python
review_structure = """
# I. INTRODUCTION
   - Background on the topic
   - Significance of the review
   - Scope and organization

# II. BODY
   - Thematic/Chronological/Methodological organization
   
   ## A. Theme/Topic 1
      - Existing research
      - Key findings
      - Gaps identified
   
   ## B. Theme/Topic 2
      - Existing research
      - Key findings
      - Gaps identified
   
   ## C. Synthesis
      - How themes connect
      - Overall state of field

# III. CONCLUSION
   - Summary of key findings
   - Research gaps
   - Implications for future research
"""
```

### Systematic Review Structure

```python
systematic_review = """
# I. ABSTRACT
   - Background, Methods, Results, Conclusions

# II. INTRODUCTION
   - Rationale and objectives
   - Research questions/PICOs

# III. METHODS
   - Search strategy (databases, terms, dates)
   - Inclusion/exclusion criteria
   - Study selection process
   - Data extraction
   - Quality assessment
   - Synthesis method

# IV. RESULTS
   - Study selection (PRISMA flow diagram)
   - Study characteristics
   - Risk of bias assessment
   - Synthesis of findings

# V. DISCUSSION
   - Interpretation of results
   - Limitations
   - Comparisons with other studies

# VI. CONCLUSION
   - Key takeaways
   - Implications for practice/policy
"""
```

---

## 6. Practical Search Workflow

```python
def conduct_literature_review(topic, databases=['scholar', 'pubmed']):
    """
    Step-by-step literature review process.
    """
    steps = []
    
    # Step 1: Define search terms
    search_terms = {
        'primary': [topic],
        'synonyms': generate_synonyms(topic),
        'related': generate_related_terms(topic),
        'filters': ['peer-reviewed', '2019-2024', 'English']
    }
    steps.append(f"1. Search terms: {search_terms}")
    
    # Step 2: Initial search
    initial_results = search_databases(search_terms, databases)
    steps.append(f"2. Initial results: {len(initial_results)} papers")
    
    # Step 3: Screen titles/abstracts
    relevant = screen_papers(initial_results, inclusion_criteria)
    steps.append(f"3. After screening: {len(relevant)} papers")
    
    # Step 4: Full-text review
    final_papers = full_text_review(relevant)
    steps.append(f"4. After full-text: {len(final_papers)} papers")
    
    # Step 5: Snowball (citations forward/backward)
    cited_papers = snowball_search(final_papers)
    steps.append(f"5. Snowball citations: {len(cited_papers)} additional")
    
    # Step 6: Quality assessment
    quality_assessed = assess_quality(final_papers + cited_papers)
    steps.append(f"6. Final papers: {len(quality_assessed)}")
    
    return {
        'steps': steps,
        'papers': quality_assessed,
        'synthesis': synthesize_findings(quality_assessed)
    }
```

### Citation Managers Integration

```python
# Export references in various formats
reference_formats = {
    'bibtex': 'references.bib',
    'ris': 'references.ris', 
    'endnote': 'references.enw',
    'csv': 'references.csv'
}

# BibTeX example
bibtex_template = """
@article{{{cite_key},
  author  = {{{author}}},
  title   = {{{{{title}}}}},
  journal = {{{journal}}},
  year    = {{{year}}},
  volume  = {{{volume}}},
  number  = {{{number}}},
  pages   = {{{pages}}},
  doi     = {{{doi}}}
}}
"""
```

---

## 7. Citation Searching (Beyond Keyword)

### Backward Citation Tracking
```
Paper A → cites Paper B, C, D → review B, C, D
```

### Forward Citation Tracking
```
Paper X is cited by Papers 1, 2, 3 → review newer papers
```

### Author Replication Search
```
Known expert in field → search their other publications
```

### Journal Alert Setup

```python
# Google Scholar Alerts
google_alert_query = """
machine learning healthcare diabetes
"""

# PubMed Alerts via My NCBI
pubmed_alert = """
("machine learning"[Title/Abstract] OR "deep learning"[Title/Abstract])
AND diabetes[Title/Abstract] 
AND ("2024"[PDAT] : "2024"[PDAT])
"""
```

---

## 8. Quality Checklist

### Before Including Any Source

- [ ] Is it peer-reviewed?
- [ ] Is it from a credible journal/publisher?
- [ ] Is it recent (usually within 5-10 years)?
- [ ] Does it relate directly to my research question?
- [ ] Is the methodology appropriate?
- [ ] Are the conclusions supported by data?
- [ ] Are there any conflicts of interest?
- [ ] Can I verify this with another source?

### For Literature Review Writing

- [ ] Organized by theme/topic, not just by paper
- [ ] Critical evaluation (not just summary)
- [ ] Identification of gaps in literature
- [ ] Clear connection to your research question
- [ ] Proper in-text citations
- [ ] Complete reference list in correct format
- [ ] No plagiarism - paraphrased with attribution

---

## Quick Reference

### Common Database Search Fields

| Field | PubMed | Scholar |
|-------|--------|---------|
| Title | [ti] | intitle: |
| Abstract | [ab] | allintitle: |
| Author | [au] | author: |
| Year | [dp] | after:year |
| DOI | [doi] | - |

### Sample Search Strings

```python
# Basic topic search
basic = "machine learning" AND healthcare

# With field tags (PubMed)
advanced = '"machine learning"[tiab] AND "healthcare"[tiab] AND ("2019:2024"[dp])'

# With study type filter
systematic = '"systematic review"[pt] OR "meta-analysis"[pt]'
```