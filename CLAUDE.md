# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ARENA 3.0 is an educational curriculum for AI safety training by Callum McDougall. It covers ML fundamentals, transformer interpretability, reinforcement learning, and LLM evaluations through hands-on exercises.

## Installation

```bash
# Using conda (recommended)
conda create -n arena-env python=3.11
conda activate arena-env
pip install -r requirements.txt

# Or run the install script (installs Miniconda if needed)
./install.sh
```

## Running Tests

Each exercise set has its own `tests.py` file. Tests are designed to validate student implementations by calling test functions with the student's function as an argument.

```bash
# Run tests for a specific exercise (from repo root)
cd chapter0_fundamentals/exercises/part0_prereqs && python -c "from tests import *; from solutions import *; test_einsum_trace(einsum_trace)"

# Or use pytest
pytest chapter0_fundamentals/exercises/part0_prereqs/tests.py
```

## Linting

Ruff is configured in `pyproject.toml`:
- Line length: 100
- Import sorting enabled (I)
- Ignored rules: E722, E731, F722, E402, E741

```bash
ruff check .
ruff format .
```

## Repository Architecture

### Single Source of Truth System

**All content edits must be made in `/infrastructure/master_files/`**. Master files generate all other files:

```
infrastructure/master_files/
├── master_X_Y.ipynb    # Master Jupyter notebooks (authoritative source)
├── master_X_Y.py       # Auto-generated Python versions
├── main.py             # Conversion orchestrator
└── arena_material_conversion.py  # Conversion utilities
```

The conversion pipeline (run `main.py` as a notebook):
1. `master_ipynb_to_py()` - Convert master notebook to Python
2. `generate_files()` - Generate: solutions.py, Colab notebooks, Streamlit pages
3. `master_py_to_ipynb()` - Reconstruct master notebook

### Chapter Structure

Each chapter follows this pattern:
```
chapterN_name/
├── exercises/
│   └── partX_name/
│       ├── solutions.py    # Generated reference solutions
│       ├── tests.py        # Test suite
│       └── utils.py        # Helpers
└── instructions/
    └── pages/              # Streamlit content (generated)
```

### Chapters

- **Chapter 0**: ML Fundamentals (einops, CNNs, backprop, VAEs/GANs)
- **Chapter 1**: Transformer Interpretability (TransformerLens, mechanistic interp, SAEs)
- **Chapter 2**: Reinforcement Learning (DQN, PPO, RLHF)
- **Chapter 3**: LLM Evaluations (Inspect framework, LLM agents)

## Key Dependencies by Chapter

- **Ch1**: `transformer_lens==2.11.0`, `sae-lens>=4.0.0`, `CircuitsVis`
- **Ch2**: `gymnasium[atari]==0.29.0`, `pygame`, `mujoco`
- **Ch3**: `inspect_ai`, `anthropic>=0.42.0`, `instructor`

## Contributing

When submitting PRs:
1. Edit only the master Python file in `infrastructure/master_files/master_X_Y.py`
2. Do not edit generated files (solutions.py, Streamlit pages, Colabs)
3. Report issues in the #errata Slack channel
