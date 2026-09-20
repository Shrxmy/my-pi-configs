---
name: "document-creator"
description: "Create professional documents using Google Docs or Microsoft Word with proper formatting. Use when creating documents, reports, letters, or formatted content. Covers Google Docs API, python-docx, document templates, and formatting techniques."
compatibility: pi
---

# Document Creator

Create professional documents with Google Docs API or Microsoft Word (python-docx).

## Google Docs

### Setup

```bash
# Install Google libraries
pip install google-api-python-client google-auth-httplib2 google-auth-oauthlib

# Or use gspread for simpler spreadsheet docs
pip install gspread
```

### Create Document

```python
from google.oauth2 import service_account
from googleapiclient.discovery import build
from googleapiclient.http import MediaFileUpload

# Authenticate
SCOPES = ['https://www.googleapis.com/auth/documents', 'https://www.googleapis.com/auth/drive']
creds = service_account.Credentials.from_service_account_file('credentials.json', scopes=SCOPES)

# Build service
service = build('docs', 'v1', credentials=creds)

# Create document
document = service.documents().create(
    body={
        'title': 'My Report'
    }
).execute()

document_id = document.get('documentId')
```

### Add Content with Formatting

```python
# Define requests to insert text
requests = [
    # Title
    {
        'insertText': {
            'location': {'index': 1},
            'text': 'Document Title\n'
        }
    },
    {
        'updateTextStyle': {
            'range': {'startIndex': 1, 'endIndex': 15},
            'textStyle': {
                'bold': True,
                'fontSize': {'magnitude': 24, 'unit': 'pt'},
                'headingId': 'heading_1'
            }
        }
    },
    # Paragraph
    {
        'insertText': {
            'location': {'index': 16},
            'text': 'This is the first paragraph.\n'
        }
    },
    # Bullet list
    {
        'insertText': {
            'location': {'index': 50},
            'text': '• First bullet point\n'
        }
    },
    {
        'insertText': {
            'location': {'index': 75},
            'text': '• Second bullet point\n'
        }
    },
    # Bold text
    {
        'insertText': {
            'location': {'index': 100},
            'text': 'Important: '
        }
    },
    {
        'updateTextStyle': {
            'range': {'startIndex': 100, 'endIndex': 110},
            'textStyle': {'bold': True}
        }
    }
]

# Execute requests
service.documents().batchUpdate(
    documentId=document_id,
    body={'requests': requests}
).execute()
```

### Formatting Guide

| Element | Style Settings |
|---------|----------------|
| Heading 1 | fontSize: 32, bold, headingId: 'heading_1' |
| Heading 2 | fontSize: 24, bold, headingId: 'heading_2' |
| Body | fontSize: 11 |
| Bold | bold: true |
| Italic | italic: true |
| Link | link: {url: 'https://...'} |
| Bullets | Bullet list via insertText with '• ' prefix |

### Full Document Example

```python
from google.oauth2 import service_account
from googleapiclient.discovery import build

SCOPES = ['https://www.googleapis.com/auth/documents']
creds = service_account.Credentials.from_service_account_file('credentials.json', scopes=SCOPES)
service = build('docs', 'v1', credentials=creds)

def create_report(title, paragraphs, service):
    # Create doc
    doc = service.documents().create(body={'title': title}).execute()
    doc_id = doc.get('documentId')
    
    index = 1
    requests = []
    
    # Title
    requests.append({
        'insertText': {'location': {'index': index}, 'text': f'{title}\n'}
    })
    requests.append({
        'updateTextStyle': {
            'range': {'startIndex': 1, 'endIndex': len(title) + 1},
            'textStyle': {'bold': True, 'fontSize': {'magnitude': 24, 'unit': 'pt'}}
        }
    })
    index += len(title) + 2
    
    # Content
    for p in paragraphs:
        requests.append({
            'insertText': {'location': {'index': index}, 'text': f'{p}\n\n'}
        })
        index += len(p) + 2
    
    service.documents().batchUpdate(documentId=doc_id, body={'requests': requests}).execute()
    return doc_id

# Usage
doc_id = create_report('Project Report', [
    'Executive Summary',
    'This is the executive summary of the project...',
    'Key Findings',
    '• Revenue increased by 20%',
    '• Customer satisfaction improved',
], service)
```

---

## Microsoft Word (python-docx)

### Setup

```bash
pip install python-docx
```

### Basic Document

```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.enum.style import WD_STYLE_TYPE

doc = Document()

# Title
title = doc.add_heading('Document Title', level=0)
title.alignment = WD_ALIGN_PARAGRAPH.CENTER

# Paragraph
doc.add_paragraph('This is a regular paragraph with some text.')

# Bold and italic
p = doc.add_paragraph()
p.add_run('Bold text').bold = True
p.add_run(' and ')
p.add_run('italic text').italic = True

# Heading
doc.add_heading('Section Heading', level=1)

# Bullet list
doc.add_paragraph('First item', style='List Bullet')
doc.add_paragraph('Second item', style='List Bullet')
doc.add_paragraph('Third item', style='List Bullet')

# Numbered list
doc.add_paragraph('Step 1', style='List Number')
doc.add_paragraph('Step 2', style='List Number')

# Save
doc.save('output.docx')
```

### Professional Report Template

```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor, Cm
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.enum.table import WD_TABLE_ALIGNMENT
from docx.oxml.ns import qn
from docx.oxml import OxmlElement

doc = Document()

# Set page margins
for section in doc.sections:
    section.top_margin = Cm(2.5)
    section.bottom_margin = Cm(2.5)
    section.left_margin = Cm(2.5)
    section.right_margin = Cm(2.5)

# Title page
title = doc.add_heading('', level=0)
run = title.add_run('PROJECT REPORT')
run.font.size = Pt(36)
run.font.bold = True
run.font.color.rgb = RGBColor(0, 51, 102)
title.alignment = WD_ALIGN_PARAGRAPH.CENTER

doc.add_paragraph('')

subtitle = doc.add_paragraph()
subtitle.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = subtitle.add_run('Quarterly Performance Analysis')
run.font.size = Pt(18)
run.font.color.rgb = RGBColor(100, 100, 100)

doc.add_paragraph('')
doc.add_paragraph('')

meta = doc.add_paragraph()
meta.alignment = WD_ALIGN_PARAGRAPH.CENTER
meta.add_run('Prepared by: Your Name\n')
meta.add_run('Date: January 2025\n')
meta.add_run('Version: 1.0')

doc.add_page_break()

# Table of Contents placeholder
doc.add_heading('Table of Contents', level=1)
toc = doc.add_paragraph()
toc.add_run('1. Executive Summary .................... 3\n')
toc.add_run('2. Key Metrics ......................... 4\n')
toc.add_run('3. Analysis ........................... 5\n')

doc.add_page_break()

# Section 1
doc.add_heading('1. Executive Summary', level=1)
doc.add_paragraph(
    'This report provides a comprehensive analysis of the quarterly performance, '
    'highlighting key achievements, challenges, and recommendations for improvement.'
)

# Table
doc.add_heading('Key Metrics', level=2)
table = doc.add_table(rows=4, cols=2)
table.style = 'Table Grid'

# Header row
cells = table.rows[0].cells
cells[0].text = 'Metric'
cells[1].text = 'Value'
for cell in cells:
    cell.paragraphs[0].runs[0].bold = True

# Data rows
data = [
    ('Revenue', '$1.2M'),
    ('Growth', '15%'),
    ('Customers', '5,000'),
]
for i, (metric, value) in enumerate(data, 1):
    table.rows[i].cells[0].text = metric
    table.rows[i].cells[1].text = value

doc.add_paragraph('')

# Bullet points
doc.add_heading('Key Achievements', level=2)
achievements = [
    'Launched new product line with 3 variants',
    'Expanded to 2 new markets',
    'Improved customer satisfaction score to 4.5/5',
    'Reduced operational costs by 10%',
]
for item in achievements:
    doc.add_paragraph(item, style='List Bullet')

doc.save('report.docx')
print('Report saved to report.docx')
```

### Formatting Quick Reference

| Element | Code |
|---------|------|
| Font size | `run.font.size = Pt(12)` |
| Bold | `run.bold = True` |
| Italic | `run.italic = True` |
| Color | `run.font.color.rgb = RGBColor(0, 0, 0)` |
| Heading | `doc.add_heading('Title', level=1)` |
| Bullet | `doc.add_paragraph('text', style='List Bullet')` |
| Numbered | `doc.add_paragraph('text', style='List Number')` |
| Link | `run.add_hyperlink(url, text)` |
| Table | `doc.add_table(rows, cols)` |
| Image | `doc.add_picture('image.png', width=Inches(6))` |
| Page break | `doc.add_page_break()` |

### Hyperlink in Word

```python
def add_hyperlink(paragraph, url, text):
    """Add a hyperlink to a paragraph."""
    part = paragraph.part
    r_id = part.relate_to(url, 'http://schemas.openxmlformats.org/officeDocument/2006/relationships/hyperlink', is_external=True)
    
    hyperlink = OxmlElement('w:hyperlink')
    hyperlink.set(qn('r:id'), r_id)
    
    new_run = OxmlElement('w:r')
    rPr = OxmlElement('w:rPr')
    
    c = OxmlElement('w:color')
    c.set(qn('w:val'), '0000FF')
    rPr.append(c)
    
    u = OxmlElement('w:u')
    u.set(qn('w:val'), 'single')
    rPr.append(u)
    
    new_run.append(rPr)
    new_run.text = text
    hyperlink.append(new_run)
    
    paragraph._p.append(hyperlink)
    return hyperlink

# Usage
p = doc.add_paragraph()
p.add_run('Click ')
add_hyperlink(p, 'https://example.com', 'here')
p.add_run(' to visit our website.')
```

---

### Two-Column Layout

```python
from docx import Document
from docx.shared import Pt, Twips
from docx.oxml import OxmlElement
from docx.oxml.ns import qn

doc = Document()

# Create section with two columns
section = doc.sections[0]

# Set columns via direct XML
sectPr = section._sectPr
cols = OxmlElement('w:cols')
cols.set(qn('w:num'), '2')
cols.set(qn('w:space'), '720')  # 720 twips = 0.5 inch spacing
sectPr.append(cols)

# Add content
doc.add_heading('Two Column Document', level=0)
doc.add_paragraph('This text appears in the first column. ' * 20)
doc.add_paragraph('This text appears in the second column. ' * 20)

# Single column section
doc.add_page_break()
doc.add_heading('Back to Single Column', level=1)

# Reset to single column
sectPr = section._sectPr
cols = OxmlElement('w:cols')
cols.set(qn('w:num'), '1')
sectPr.append(cols)

doc.save('two-column.docx')
```

### Two-Column with Different Widths

```python
from docx import Document
from docx.oxml import OxmlElement
from docx.oxml.ns import qn

doc = Document()
section = doc.sections[0]
sectPr = section._sectPr

# Two columns with 70/30 split
cols = OxmlElement('w:cols')
cols.set(qn('w:num'), '2')

# First column (70%)
col1 = OxmlElement('w:col')
col1.set(qn('w:space'), '720')
col1.set(qn('w:w'), '7000')  # 7000/10000 = 70%
cols.append(col1)

# Second column (30%)
col2 = OxmlElement('w:col')
col2.set(qn('w:w'), '3000')  # 3000/10000 = 30%
cols.append(col2)

sectPr.append(cols)

doc.add_paragraph('Main content (70% width)')
doc.add_paragraph('')  # Forces column break
doc.add_paragraph('Sidebar content (30% width)')

doc.save('asymmetric-columns.docx')
```

---

## IEEE Format Documents

IEEE papers follow strict formatting guidelines. Here's a complete template:

### IEEE Paper Template

```python
from docx import Document
from docx.shared import Pt, Inches, Cm
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.oxml import OxmlElement
from docx.oxml.ns import qn
from docx.enum.table import WD_TABLE_ALIGNMENT

doc = Document()

# ============================================
# PAGE SETUP (IEEE Standard)
# ============================================
section = doc.sections[0]
section.page_width = Inches(8.5)
section.page_height = Inches(11)
section.top_margin = Cm(2.54)    # 1 inch
section.bottom_margin = Cm(2.54)
section.left_margin = Cm(1.78)   # 0.7 inch
section.right_margin = Cm(1.78)  # 0.7 inch
section.header_distance = Cm(1.27)  # 0.5 inch from top
section.footer_distance = Cm(1.27)  # 0.5 inch from bottom

# Two-column layout
sectPr = section._sectPr
cols = OxmlElement('w:cols')
cols.set(qn('w:num'), '2')
cols.set(qn('w:space'), '708')  # 0.5 inch between columns
sectPr.append(cols)

# ============================================
# TITLE (Full-width in first column)
# ============================================
title = doc.add_heading('', level=0)
run = title.add_run('Paper Title: A Comprehensive Study on Machine Learning Applications')
run.font.size = Pt(24)
run.font.bold = True
run.font.name = 'Times New Roman'

doc.add_paragraph()

# ============================================
# AUTHOR BLOCK (Centered)
# ============================================
author_para = doc.add_paragraph()
author_para.alignment = WD_ALIGN_PARAGRAPH.CENTER
author_run = author_para.add_run('Author Name¹, Co-Author², and Supervisor³')
author_run.font.size = Pt(10)
author_run.font.name = 'Times New Roman'

affiliation = doc.add_paragraph()
affiliation.alignment = WD_ALIGN_PARAGRAPH.CENTER
affil_run = affiliation.add_run('¹Department of Computer Science, University Name\n'
                                  '²Research Institute, Another University\n'
                                  '³Company or Lab Name')
affil_run.font.size = Pt(10)
affil_run.font.name = 'Times New Roman'

doc.add_paragraph()

# Abstract (full-width)
doc.add_heading('Abstract', level=2)
abstract = doc.add_paragraph()
abstract_run = abstract.add_run(
    'This is the abstract section. It should be between 150-250 words and '
    'provide a concise summary of the paper. The abstract should state the '
    'problem, methodology, results, and conclusions. IEEE papers require a '
    'brief abstract that appears at the beginning of the paper.'
)
abstract_run.font.size = Pt(10)
abstract_run.font.name = 'Times New Roman'

doc.add_paragraph()

# Keywords
kw_para = doc.add_paragraph()
kw_run = kw_para.add_run('Index Terms—Keyword1, Keyword2, Keyword3, Keyword4, Keyword5')
kw_run.font.size = Pt(10)
kw_run.font.name = 'Times New Roman'
kw_run.italic = True

doc.add_page_break()

# ============================================
# I. INTRODUCTION (Column-width)
# ============================================
doc.add_heading('I. INTRODUCTION', level=2)

intro_para = doc.add_paragraph()
intro_text = (
    'The introduction section should provide background information on the '
    'topic, state the problem being addressed, and outline the contributions '
    'of the paper. It typically spans 1-2 pages and sets up the motivation for '
    'the research. Reference existing work [1], [2] and explain the gap in '
    'the literature that this paper addresses.'
)
intro_run = intro_para.add_run(intro_text)
intro_run.font.size = Pt(10)
intro_run.font.name = 'Times New Roman'

# Citation example
cite = doc.add_paragraph()
cite_run = cite.add_run('This follows IEEE citation style [3].')
cite_run.font.size = Pt(10)
cite_run.font.name = 'Times New Roman'

# ============================================
# II. RELATED WORK
# ============================================
doc.add_heading('II. RELATED WORK', level=2)

doc.add_paragraph(
    'This section reviews existing literature and previous work in the area. '
    'Organize it thematically rather than chronologically. Discuss the '
    'strengths and limitations of each approach.', style=None
)

# Reference list format
doc.add_paragraph('[1] A. Author and B. Co-Author, "Title of related paper," '
                   'Journal Name, vol. 10, pp. 100-120, 2023.', style=None)

doc.add_paragraph('[2] C. Another Author, "Another paper title," Conference Name, '
                   'pp. 50-60, 2022.', style=None)

# ============================================
# III. METHODOLOGY
# ============================================
doc.add_heading('III. METHODOLOGY', level=2)

doc.add_paragraph(
    'Describe your methodology in detail. Include diagrams if needed. '
    'Explain the experimental setup, algorithms used, and data sources.', style=None
)

# Add a figure reference
fig_para = doc.add_paragraph()
fig_para.add_run('Fig. 1. ').bold = True
fig_para.add_run('Architecture diagram showing the proposed system components.')

# ============================================
# IV. RESULTS
# ============================================
doc.add_heading('IV. RESULTS', level=2)

doc.add_paragraph(
    'Present your results clearly with tables and figures. Reference them '
    'as Fig. 2, Table I, etc. Use SI units and proper significant figures.', style=None
)

# Table in IEEE format
doc.add_heading('TABLE I', level=3)
doc.add_paragraph('COMPARISON OF RESULTS', style=None)

table = doc.add_table(rows=4, cols=3)
table.style = 'Table Grid'

# Header
headers = ['Method', 'Accuracy (%)', 'Time (ms)']
for i, h in enumerate(headers):
    cell = table.rows[0].cells[i]
    cell.text = h
    cell.paragraphs[0].runs[0].bold = True

# Data
data = [
    ['Proposed', '95.2', '120'],
    ['Baseline A', '87.5', '95'],
    ['Baseline B', '89.0', '110'],
]
for row_idx, row_data in enumerate(data, 1):
    for col_idx, cell_data in enumerate(row_data):
        table.rows[row_idx].cells[col_idx].text = cell_data

# ============================================
# V. DISCUSSION
# ============================================
doc.add_heading('V. DISCUSSION', level=2)

doc.add_paragraph(
    'Analyze your results and explain their implications. Address any '
    'limitations and suggest future work. This section should connect your '
    'findings back to the problem statement.', style=None
)

# ============================================
# VI. CONCLUSION
# ============================================
doc.add_heading('VI. CONCLUSION', level=2)

doc.add_paragraph(
    'Summarize the main contributions and key findings. Restate the '
    'significance of your work and suggest potential applications or '
    'future research directions.', style=None
)

# ============================================
# REFERENCES
# ============================================
doc.add_page_break()
doc.add_heading('REFERENCES', level=2)

references = [
    '[1] J. Smith and A. Johnson, "Deep learning for image classification,"
        IEEE Trans. Pattern Anal. Mach. Intell., vol. 45, no. 3, pp. 123-145, 2023.',
    '[2] K. Williams et al., "Neural network architectures: A survey,"
        Comput. Surveys, vol. 52, no. 4, pp. 1-38, 2022.',
    '[3] M. Brown, "Optimization algorithms for machine learning," Ph.D.
        dissertation, MIT, Cambridge, MA, 2021.',
    '[4] R. Davis and S. Miller, "Transformer models for NLP tasks," in
        Proc. IEEE Int. Conf. Comput. Vis. Pattern Recognit., 2023, pp. 100-110.',
    '[5] L. Wilson, "Survey of reinforcement learning methods," IEEE Commun.
        Mag., vol. 60, no. 2, pp. 45-52, 2024.',
]

for ref in references:
    p = doc.add_paragraph(ref, style=None)
    p.paragraph_format.left_indent = Inches(0.5)

doc.save('ieee-paper.docx')
```

### IEEE Formatting Rules Summary

| Element | IEEE Standard |
|---------|---------------|
| Paper size | 8.5" x 11" (Letter) |
| Columns | Two columns |
| Margins | Top/Bottom: 1", Left/Right: 0.7" |
| Column spacing | 0.5" |
| Title font | 24pt, Bold |
| Body font | 10pt, Times New Roman |
| Abstract | 150-250 words, full-width |
| Keywords | Italic, Index Terms— prefix |
| Section headers | Uppercase, Roman numerals (I, II, ...) |
| Figure captions | "Fig. X." + description |
| Table captions | "TABLE X" + title above |
| References | Numbered, hanging indent |

---

## Best Practices

1. **Google Docs**: Use for collaborative, cloud-based documents
2. **Word**: Use for offline, formatted documents with complex layouts
3. **Templates**: Create templates with placeholders for repeated documents
4. **Styles**: Use built-in styles for consistency
5. **Save often**: Save documents after major sections

## Quick Template

```python
from docx import Document
from docx.shared import Pt
from docx.enum.text import WD_ALIGN_PARAGRAPH

def create_letter( recipient, subject, body):
    doc = Document()
    doc.add_paragraph(f'Date: {date.today()}')
    doc.add_paragraph(f'To: {recipient}')
    doc.add_paragraph(f'Subject: {subject}')
    doc.add_paragraph('')
    for para in body:
        doc.add_paragraph(para)
    return doc

letter = create_letter(
    'John Doe',
    'Project Update',
    ['Dear John,', 'Here is your update...', 'Best regards']
)
letter.save('letter.docx')
```