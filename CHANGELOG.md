# nextflow-workshop-lnu: Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## v1.0.0 - [Unreleased]

### `Added`

- Pipeline versions of Trim Galore (and Cutadapt) and BBMap in `software_versions.yml` and the MultiQC report.
- Trim Galore trimming reports in the MultiQC report (Cutadapt section).
- `--ref_genome` in the parameter schema, so it is documented in `--help` and validated.
- BBMap logs published next to the alignments.
- Repository documentation: README, usage and output pages, and citations for all tools used.

### `Changed`

- `--ref_genome` is the validated reference parameter (it used to check `--fasta`/`--genome`, which the workflow does not use).
- Minimum Nextflow version is now 24.04.0. Resource limits use `process.resourceLimits`; the `--max_cpus`, `--max_memory` and `--max_time` parameters and `check_max()` were removed.
- Alignments are published as copies (`--publish_dir_mode`) instead of symlinks into the work directory.
- The repository was renamed internally from the nf-core `testflow` template to `nextflow-workshop-lnu`.
- CI runs the test profile on the minimum and the latest Nextflow versions.

### `Fixed`

- Config no longer fails to parse on recent Nextflow versions.
- Test profile no longer sets an unrelated `genome` to bypass reference validation.

### `Removed`

- iGenomes configuration and the unused `--genome`, `--igenomes_ignore`, `--skip_fastqc`, `--skip_qc` and `--skip_trimming` options.
- nf-core community boilerplate that does not apply to this repository: AWS, linting and release-announcement workflows, Tower and Dockstore files, Gitpod/devcontainer setup, issue and PR templates, code of conduct, and the `test_full` profile.
