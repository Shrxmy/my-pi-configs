---
name: "jupyter-notebook"
description: "Expert in Jupyter Notebook workflows, data exploration, interactive computing, and notebook automation. Use when working with JupyterLab, IPython, data analysis, visualization, or running interactive Python code blocks. Covers pandas analysis, matplotlib/seaborn plotting, SQL integration via %%sql, and notebook-to-Python conversion workflows."
compatibility: pi
---

# Jupyter Notebook Expert

Mastery of Jupyter Notebook environments for data science, exploration, and interactive computing.

## Core Workflows

### 1. Running Code

Execute Python code in notebook cells with proper syntax and best practices.

```python
# Single cell execution
import pandas as pd
import numpy as np

# Quick data exploration
df = pd.read_csv('data.csv')
df.head()
df.info()
df.describe()
```

### 2. SQL Integration

Use SQL directly in notebooks for database queries.

```sql
%%sql
SELECT * FROM users WHERE created_at >= '2024-01-01'
LIMIT 10
```

### 3. Visualization

Create interactive visualizations.

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Distribution plot
sns.histplot(data=df, x='value', kde=True)
plt.title('Distribution')
plt.show()
```

### 4. Notebook Management

```bash
# Convert notebook to Python script
jupyter nbconvert --to script notebook.ipynb

# Convert to HTML
jupyter nbconvert --to html notebook.ipynb

# Convert to PDF
jupyter nbconvert --to pdf notebook.ipynb

# Execute notebook non-interactively
jupyter nbconvert --to notebook --execute notebook.ipynb
```

## Magic Commands

| Command | Description |
|---------|-------------|
| `%%time` | Time cell execution |
| `%%writefile` | Write cell content to file |
| `%%capture` | Capture cell output |
| `%%bash` | Run bash commands |
| `%load_ext` | Load extension |
| `%who` | List variables |
| `%reset` | Clear all variables |

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Shift+Enter` | Run cell, move to next |
| `Ctrl+Enter` | Run cell, stay |
| `Alt+Enter` | Run cell, insert below |
| `dd` | Delete cell |
| `a` | Insert cell above |
| `b` | Insert cell below |
| `m` | Markdown cell |
| `y` | Code cell |

## Best Practices

1. **Organize notebooks** with clear sections and markdown headings
2. **Restart and run all** before sharing or exporting
3. **Use virtual environments** for reproducibility
4. **Clear outputs** before version control
5. **Export to HTML/PDF** for sharing

## Extensions

```bash
# Install useful extensions
pip install jupyterlab-nvdashboard
pip install jupyter_contrib_nbextensions
pip install ipywidgets

# Enable extensions
jupyter labextension install @jupyterlab/geojson-extension
```

## Quick Reference

```python
# Essential imports
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Auto-reload modules
%load_ext autoreload
%autoreload 2

# Display options
pd.set_option('display.max_columns', 50)
pd.set_option('display.width', 200)
```