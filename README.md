# Contact-force correction for reflective SpO2 estimation

ECE 532 final project proposal — Zifei He, Department of Electrical and Computer Engineering, Rutgers University–New Brunswick.

Proposal PDF is supplied separately with the course submission. [LaTeX source](proposal.tex) · [Overleaf project](https://www.overleaf.com/project/6ae3ca744cdba3e977fd982b)

## Research question
Can correction along the contact-force → optical coupling/perfusion → PPG AC/DC → dual-wavelength ratio pathway reduce SpO2 error and force-dependent bias compared with direct feature fusion? The model uses a corrected ratio, a decreasing physical mapping, and bounded residual compensation. It does not claim identification of individual counterfactual oxygen saturation.

## Status and scope
Completed work comprises contact-force experiments, influence analysis, and data collection. Model implementation, evaluation, two independent verification procedures, failure injection, and recovery are planned course work. No project performance or measured runtime is claimed in this proposal. The thesis Chapter 5 correction design is part of the planned implementation.

## Proposal materials
- proposal.pdf and proposal.tex: five-section proposal with five annotated references.
- config/smoke.yaml: proposed small CPU plumbing check.
- config/cpu.yaml: proposed full subject-disjoint CPU evaluation.
- DATA_ACCESS.md: author-confirmed data access and disclosure boundary.
- OPTIONAL_SCALE_UP.md: no additional compute requested.

The YAML files are proposed implementation contracts. Entry points, calibration manifests, cached replay records, and experiment outputs referenced by them are future artifacts; these configurations are not currently runnable experiments. The evidence bundle is excluded from this package at the author's request and must be supplied separately if required by the course.

## Available data
CF-controlled export: 1,620 five-second segments, nine subjects, two 1,250-sample PPG channels, segment-level force 0.2–3.0 N and reference SpO2 95–97%. No nonfinite values were found in these exported arrays. This narrow label range limits primary claims to normal oxygenation; constant-label folds require special handling. Raw force timestamps are absent from the export, so timing faults use synthetic metadata.

The local WF-PPG copy has 27 subject folders and 165 preprocessed recordings, with wrist PPG, pressure, ECG, BP and SpO2 columns. Pressure units, reference-label provenance and preprocessing require review before external evaluation; pressure is not assumed interchangeable with force in newtons.

## Evaluation and verification plan
Compare mean, ridge, PPG-only and PPG-plus-force baselines with the correction model under leave-one-subject-out and force-band holdouts. Report subject-level error, force-dependent bias, prediction variability, correlation, and Bland–Altman agreement. Fit preprocessing and acceptance thresholds without test subjects or test labels.

Input-quality verification checks source integrity, channel validity and synchronization. Independent applicability verification checks model integrity, validated feature/force support and output constraints. Report only when both pass and provenance is complete; unknown results and exceptions defer reporting. Restore only a verified cached source or select an existing valid synchronized segment, then recheck both procedures. Inject missing force, timing offsets, clipping, channel swaps, unsupported force, model tampering and checker exceptions.

A1 evidence records, A2 calibrated decision policies and A3 typed causal claims will inform the source-linked JSON interface. Integration with the actual starter schemas remains implementation work.

## CPU feasibility and milestones
Four CPU cores and 16 GB RAM; estimated baseline 1–3 hours, evaluation under 1 hour, peak memory below 8 GB. These are estimates, to be measured against the 4-hour baseline and 2-hour evaluator limits. No GPU, paid API, or network after setup is required.

Nov 13: audit data and baselines. Dec 1: CPU pipeline, verification, JSON records, faults, recovery and measured resources. Dec 11: ablations and poster. Dec 15: final report and reproducibility appendix.

## Planned reproduction interface
The following are future course-required targets, not commands that pass today:

```text
pytest -q
python -m src.run --config config/smoke.yaml --device cpu
python -m src.evaluate --config config/cpu.yaml --device cpu
python -m utilities.verification.validate --strict experiments/receipt.json
```
