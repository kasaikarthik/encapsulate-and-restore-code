# Encapsulate and Restore Code Scripts

This document provides commands to run the 3 scripts for encapsulating and restoring code files.

## Prerequisites

Install required dependencies:
```bash
pip install python-docx==0.8.11
```

## Scripts Overview

1. **`encapsulate_to_doc.py`** - Encapsulates codebase files into a Word document
2. **`restore_from_doc.py`** - Restores files from a Word document
3. **`create_staged_files.py`** - Encapsulates staged/modified files into a Word document (git integration)

---

## 1. Encapsulate Codebase to Word Document

### Basic Usage
```bash
# Encapsulate entire codebase (excluding .ignoredoc patterns)
python encapsulate_to_doc.py -o my_project.docx

# Use custom ignore file
python encapsulate_to_doc.py -o my_project.docx -i .gitignore

# Default output filename
python encapsulate_to_doc.py
```

### Examples
```bash
# Create project documentation
python encapsulate_to_doc.py -o ai_agents_codebase.docx

# Create documentation excluding specific patterns
python encapsulate_to_doc.py -o clean_codebase.docx -i custom_ignore.txt
```

---

## 2. Restore Files from Word Document

### Basic Usage
```bash
# Restore files to current directory
python restore_from_doc.py my_project.docx

# Restore to specific directory
python restore_from_doc.py my_project.docx -o /path/to/restore/location

# List files in document without restoring
python restore_from_doc.py my_project.docx -l
```

### Examples
```bash
# Restore with confirmation prompt
python restore_from_doc.py ai_agents_codebase.docx

# Restore to new directory
python restore_from_doc.py ai_agents_codebase.docx -o restored_project/

# Preview what would be restored
python restore_from_doc.py ai_agents_codebase.docx -l
```

---

## 3. Create Word Document from Staged/Modified Files

### Git Integration
```bash
# Create Word document from files currently staged in git
python create_staged_files.py --git-staged -o staged_files.docx

# Create Word document from files modified in git (not staged)
python create_staged_files.py --git-modified -o modified_files.docx

# List staged files without creating document
python create_staged_files.py --git-staged --list

# List modified files without creating document
python create_staged_files.py --git-modified --list
```

### Pattern Matching
```bash
# Create Word document with specific file patterns
python create_staged_files.py -f "*.py" "*.md" "*.txt" -o python_docs.docx

# Create with exclusions
python create_staged_files.py -f "*.py" -e "venv/" "__pycache__/" -o clean_python.docx

# Create with all files (with default exclusions)
python create_staged_files.py -o all_files.docx
```

### Examples
```bash
# Create Word document from git staged files
python create_staged_files.py --git-staged -o my_staged_changes.docx

# Create Word document with only Python and Markdown files
python create_staged_files.py -f "*.py" "*.md" -o python_docs.docx

# Create Word document from modified files for review
python create_staged_files.py --git-modified -o review_changes.docx
```

---

## Complete Workflow Examples

### 1. Document Current Git Staged Files
```bash
# Create Word document from staged files
python create_staged_files.py --git-staged -o staged_changes.docx

# Restore specific files from the document
python restore_from_doc.py staged_changes.docx -o restored_files/
```

### 2. Backup and Restore Workflow
```bash
# Create backup of entire codebase
python encapsulate_to_doc.py -o backup_$(date +%Y%m%d).docx

# Later, restore from backup
python restore_from_doc.py backup_20241009.docx -o restored_project/
```

### 3. Selective File Management
```bash
# Create Word document with only Python files
python create_staged_files.py -f "*.py" -o python_files.docx

# Restore Python files to new location
python restore_from_doc.py python_files.docx -o python_backup/
```

### 4. Git Integration Workflow
```bash
# Create Word document from modified files
python create_staged_files.py --git-modified -o modified_files.docx

# Preview what would be restored
python restore_from_doc.py modified_files.docx -l

# Restore files to new directory
python restore_from_doc.py modified_files.docx -o restored_project/
```

### 5. Staged Files Workflow (Tested Example)
```bash
# List staged files
python create_staged_files.py --git-staged --list

# Create Word document from staged files
python create_staged_files.py --git-staged -o test_staged_workflow.docx

# Verify document contents
python restore_from_doc.py test_staged_workflow.docx -l

# Restore files to new directory
python restore_from_doc.py test_staged_workflow.docx -o test_restore/
```

---

## Command Line Options Summary

### encapsulate_to_doc.py
- `-o, --output`: Output Word document filename (default: codebase.docx)
- `-i, --ignore`: Ignore file with patterns (default: .ignoredoc)

### restore_from_doc.py
- `doc_file`: Word document file to restore from (required)
- `-o, --output`: Output directory (default: current directory)
- `-l, --list`: List files in document without restoring

### create_staged_files.py
- `-o, --output`: Output Word document filename (default: staged_files.docx)
- `-f, --files`: File patterns to include
- `-e, --exclude`: File patterns to exclude
- `--git-staged`: Use files currently staged in git
- `--git-modified`: Use files modified in git (not staged)
- `-l, --list`: List files that would be included

---

## Notes

- All scripts use Python standard library except for `python-docx` dependency
- Git integration gracefully falls back to pattern matching if git is not available
- Word documents created by `create_staged_files.py` are fully compatible with `restore_from_doc.py`
- Word documents include file structure, content, and metadata
- The `create_staged_files.py` script now creates Word documents directly instead of staging directories
- Use `restore_from_doc.py` to restore files from any Word document created by either script
