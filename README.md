# SAE Batch Mode - Personal Learning Repository

This is a personal learning and development repository for exploring Security Analytics Engine (SAE) batch processing, rule creation, and threat detection model development.

## 🎯 Purpose

This repository serves as a sandbox for:
- Understanding SAE filter, rule, and model architecture
- Experimenting with batch processing workflows
- Developing cybersecurity detection capabilities
- Learning threat detection methodologies and MITRE ATT&CK framework integration

## 🔍 About SAE (Security Analytics Engine)

SAE is a sophisticated threat detection system that uses:
- **Filters**: YAML-based behavior detection patterns
- **Rules**: JSON configurations that correlate multiple filters
- **Models**: JSON files defining relationships between rules with risk scoring

## 📚 Learning Focus Areas

- Threat detection logic development
- YAML and JSON schema understanding
- Cybersecurity behavior analysis
- MITRE ATT&CK technique mapping
- Batch processing and automation

## 🛠️ Repository Structure

```
├── model/          # Detection models (JSON)
│   ├── beta/      # Testing phase models
│   ├── stable/    # Production-ready models
│   └── drop/      # Deprecated models
├── rule/          # Correlation rules (JSON)
│   ├── stable/    # Production rules
│   └── drop/      # Deprecated rules
└── README.md      # This file
```

## 🎓 Key Concepts

### Risk Levels
- **Critical**: Immediate action required
- **High**: Serious threat requiring prompt attention
- **Medium**: Notable security concern
- **Low**: Potential security issue

### Model Phases
1. **Beta** (visibleLevel: 51, silent: true) - Initial testing
2. **Silent** (visibleLevel: 51, silent: true) - Alert generation without workbench
3. **Workbench Testing** (visibleLevel: 51, silent: false) - Full alert flow testing
4. **Production** (visibleLevel: 1-9, silent: false) - Customer release

## 📖 Guidelines Reference

This repository follows structured guidelines for:
- Model quality and accuracy requirements
- Threshold and scoring configurations
- MITRE ATT&CK technique alignment
- Playbook associations
- Beta to production migration processes

## 🚀 Personal Growth Goals

- Master threat detection pattern creation
- Understand security event correlation
- Learn batch processing optimization
- Develop cybersecurity analytical skills
- Build expertise in behavioral threat detection

## 📝 Notes

This is a personal learning repository maintained by Amit Verma (amyvrm@gmail.com) for professional development in cybersecurity and threat detection.

---

**Disclaimer**: This repository contains experimental security detection rules and models for educational purposes. Use in controlled environments only.
