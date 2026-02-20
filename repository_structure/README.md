# Machine-ese Stylometry: A Corpus-Based Analysis of Large Language Model Translation Patterns

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
Copyright (c) 2025
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

This repository contains the code and data for the paper: **"From Translationese to Machine-ese: A Stylometric Analysis of Register Adaptation in Large Language Models"**.

## 📋 Table of Contents

- [Overview](#overview)
- [Key Findings](#key-findings)
- [Installation](#installation)
- [Data](#data)
- [Usage](#usage)
- [Human Evaluation](#human-evaluation)
- [Citation](#citation)
- [License](#license)

## 📖 Overview

This study investigates how Large Language Models (LLMs) differ from traditional Neural Machine Translation (NMT) systems in handling cross-register translation tasks. We conducted a comprehensive analysis using:

- **Automated Metrics**: BLEU, BERTScore, Mean Dependency Distance (MDD)
- **Stylometric Analysis**: Syntactic complexity and discourse patterns
- **Human Evaluation**: Blind assessment by professional translators
- **Cross-Register Corpus**: TED talks, UN documents, and movie subtitles

### Three Evolutionary Paradigms Identified

1. **Normative-Conservative** (DeepL, Google Translate): Traditional NMT approach
2. **Hyper-Fidelity** (Gemini-2.5): Superior register adaptation
3. **Semantic-Reconstructive** (GPT-4, Grok-2): Information density optimization

## 🔍 Key Findings

- LLMs demonstrate **context-aware selectivity**, distinguishing between "stylistic features" and "formatting noise"
- Gemini-2.5 shows **hyper-fidelity** in register adaptation across all domains
- GPT-4 exhibits **summarization bias** in formal documents, trading literal accuracy for semantic density
- Human evaluation validates quantitative findings, with high inter-rater agreement (99.4%+)

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- Git

### Setup

1. Access the repository:
```bash
# This repository is available at:
# https://anonymous.4open.science/r/machine-ese-stylometry-F16C/README.md
# Download the repository files directly from the anonymous repository.
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Download required NLP models:
```bash
python -c "import stanza; stanza.download('zh-hans')"
```

4. Verify repository structure:
```bash
python scripts/verify_structure.py
```

This will check that all required files are present and correctly named.

## 📊 Data

### Corpus Structure

```
datasets/
├── TED/                    # Planned speech
│   └── ted/
│       ├── TED_Source_EN.txt
│       ├── TED_Reference_ZH.txt
│       ├── TED_Trans_DeepL.txt
│       ├── TED_Trans_Google.txt
│       ├── TED_Trans_GPT4.txt
│       ├── TED_Trans_Grok2.txt
│       └── TED_Trans_Gemini2.5.txt
├── UNPC/                   # Formal documents
│   └── sample_600/
│       ├── UNPC_Source_EN.txt
│       ├── UNPC_Reference_ZH.txt
│       ├── UNPC_Trans_DeepL.txt
│       ├── UNPC_Trans_Google.txt
│       ├── UNPC_Trans_GPT4.txt
│       ├── UNPC_Trans_Grok2.txt
│       └── UNPC_Trans_Gemini2.5.txt
└── OpenSubtitles/          # Spontaneous dialogue
    └── opensubsititude/
        ├── OpenSubs_Source_EN.txt
        ├── OpenSubs_Reference_ZH.txt
        ├── OpenSubs_Trans_DeepL.txt
        ├── OpenSubs_Trans_Google.txt
        ├── OpenSubs_Trans_GPT4.txt
        ├── OpenSubs_Trans_Grok2.txt
        └── OpenSubs_Trans_Gemini2.5.txt
```

**Note**: 
- The scripts automatically detect files in subdirectories, so you can run commands with `--input_dir datasets/TED` and the script will find files in `datasets/TED/ted/`.
- `*_Reference_ZH.txt` files are Human Translations (HT) from OPUS corpus, used as reference for evaluation.
- All translation files use consistent naming: `*_Trans_{SystemName}.txt` where SystemName is DeepL, Google, GPT4, Grok2, or Gemini2.5.

**Data Alignment Note**: Sentence counts may differ between Source Text (ST) and Human Translation (HT) due to natural translation segmentation differences (e.g., TED: ST=599 vs HT=394; OpenSubtitles: ST=600 vs HT=344). This reflects that human translators may merge multiple source sentences into one target sentence or split one source sentence into multiple target sentences. For sentence-level metrics (MDD, sentence length, connective frequency), we use independent samples analysis since sentences cannot be matched one-to-one. For paragraph-level metrics (BLEU, BERTScore), we use paragraph-level alignment where each reference paragraph corresponds to one system output paragraph.

### Data Sources

- **TED Talks**: Parallel corpus from OPUS (2018-2023)
- **OpenSubtitles**: Movie/TV subtitles from OPUS (2015-2020)
- **UNPC**: United Nations Parallel Corpus (2015-2020)

## 🛠️ Usage

### 1. Reproduce MDD Calculations

```bash
# Calculate MDD for all translations (scripts auto-detect subdirectories)
python scripts/calculate_mdd_chinese.py --input_dir datasets/ --output results/mdd_results.csv

# Uniform sentence segmentation (specify full path to subdirectory)
python scripts/text_segmentation_tool.py --input datasets/TED/ted/TED_Trans_GPT4.txt --output segmented/
```

### 2. Compute Quality Metrics

```bash
# Calculate BLEU and BERTScore (scripts auto-detect files in subdirectories)
python scripts/calculate_metrics.py --input_dir datasets/ --output results/quality_metrics.csv

# Or specify individual corpus directory
python scripts/calculate_metrics.py --input_dir datasets/TED --reference_suffix TED_Reference_ZH.txt --output results/ted_metrics.csv
```

### 3. Generate Research Workflow Diagram

```bash
# Generate the research workflow figure (Figure 1 in paper)
python scripts/generate_research_workflow.py
```

The figure will be saved to `figures/research_workflow.png`.

### 4. Run Full Analysis Pipeline

```bash
# Process all corpora and generate stylometric features
python scripts/analyze_all_corpora.py --output results/full_analysis.csv
```

## 👥 Human Evaluation

### Evaluation Protocol

- **60 segments** randomly sampled (20 per register)
- **6 systems** evaluated per segment
- **3 dimensions**: Fluency, Adequacy, Register Appropriateness
- **Blind review** with randomized system ordering

### Results

See `human_evaluation/human_evaluation_results.csv` for detailed scores.

**Inter-rater Agreement** (Agreement Rates):
- Fluency: 99.4%
- Adequacy: 100.0%
- Register Appropriateness: 99.4%

**Note**: High agreement rates among three expert translators indicate substantial inter-rater reliability. All evaluation followed strict blind review protocol with clear operational definitions.

## 📈 Results Summary

### Key Findings Summary

| System | Best Performance Domain | Key Characteristics |
|--------|------------------------|-------------------|
| Gemini-2.5 | All domains | Hyper-fidelity register adaptation, highest BLEU in TED (0.2926) |
| GPT-4 | Formal documents | Semantic reconstruction, summarization bias in UNPC |
| DeepL | Formal documents | Normative-conservative approach, strong BLEU in UNPC (0.3973) |
| Google Translate | Balanced | Highest BLEU in UNPC (0.6586), consistent performance |

**Note**: BLEU scores vary significantly by register (TED: 0.021-0.293, UNPC: 0.353-0.659, OpenSubtitles: 0.019-0.179). See paper Table 2 for detailed metrics.

**Important Update**: BLEU scores have been recalculated with proper Chinese tokenization (`tokenize='zh'` parameter in SacreBLEU), providing accurate lexical evaluation.

### Human Evaluation Rankings

1. **TED Talks**: Gemini-2.5 (4.19) > HT (4.18) > GPT-4/Grok-2 (4.05)
2. **UNPC**: Gemini-2.5 (3.95) > HT (3.93) > GPT-4 (3.86)
3. **OpenSubtitles**: Gemini-2.5/HT (4.02) > GPT-4 (3.91) > Grok-2 (3.90)

## 📝 Citation

If you use this code or data in your research, please cite:

```bibtex
@article{your-paper-2025,
  title={From Translationese to Machine-ese: A Stylometric Analysis of Register Adaptation in Large Language Models},
  author={Your Name},
  journal={Journal Name},
  year={2025},
  publisher={Publisher}
}
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Contact

For questions or issues, please open an issue on GitHub or contact the authors.

---

**Note**: This repository contains the code and data supporting the research findings. For the full paper, please refer to the published version.

## 📦 Data Availability

The datasets and Python scripts generated and analyzed during the current study are available in the anonymous repository:
https://anonymous.4open.science/r/machine-ese-stylometry-F16C/README.md
