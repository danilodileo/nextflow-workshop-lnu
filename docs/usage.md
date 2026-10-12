# nextflow-workshop-lnu: Usage

## Requirements

- [Nextflow](https://www.nextflow.io/docs/latest/install.html) `>= 24.04.0` and Java 17 or newer.
- A container or environment engine: Docker, Singularity/Apptainer, Podman or Conda/Mamba.

> Nextflow 25.10 and later default to the strict syntax parser, which cannot load the nf-core 2023
> template this pipeline is based on. Set `export NXF_SYNTAX_PARSER=v1` before running.

## Samplesheet input

The `--input` parameter points to a comma-separated file with a header row and three columns:

| Column    | Description                                                                      |
| --------- | -------------------------------------------------------------------------------- |
| `sample`  | Sample name, no spaces. Output files get a `_T<n>` suffix (e.g. `SAMPLE1_T1`), `n` counting repeated names. |
| `fastq_1` | Path or URL to the (forward) reads, ending in `.fq.gz` or `.fastq.gz`.           |
| `fastq_2` | Path or URL to the reverse reads. Leave empty for single-end data.               |

```csv
sample,fastq_1,fastq_2
SAMPLE_PAIRED_END,/path/to/fastq/sample1_R1.fastq.gz,/path/to/fastq/sample1_R2.fastq.gz
SAMPLE_SINGLE_END,/path/to/fastq/sample2_R1.fastq.gz,
```

An example is available in [`assets/samplesheet.csv`](../assets/samplesheet.csv) and a working one
that uses public test data in [`samplesheet.csv`](../samplesheet.csv).

## Running the pipeline

```bash
nextflow run danilodileo/nextflow-workshop-lnu \
    -profile docker \
    --input samplesheet.csv \
    --ref_genome genome.fasta \
    --outdir results
```

Nextflow creates the following in the directory you launch it from:

```bash
work/        # Intermediate files; safe to delete once the run is finished
results/     # Final results (set with --outdir)
.nextflow*   # Nextflow history and logs
```

Use `-resume` to continue a previous run from the last successful step.

To reuse the same parameters, put them in a YAML or JSON file and pass it with `-params-file`:

```yaml
input: "samplesheet.csv"
ref_genome: "genome.fasta"
outdir: "results"
```

```bash
nextflow run danilodileo/nextflow-workshop-lnu -profile docker -params-file params.yaml
```

> Do not provide pipeline parameters with `-c`: custom config files are for resources and
> infrastructure settings only, parameters must come from the command line or `-params-file`.

## Parameters

| Parameter           | Description                                                      |
| ------------------- | ---------------------------------------------------------------- |
| `--input`           | Samplesheet (required).                                          |
| `--ref_genome`      | Reference genome FASTA, local path or URL (required).            |
| `--outdir`          | Output directory (required).                                     |
| `--multiqc_title`   | Title of the MultiQC report.                                     |
| `--multiqc_config`  | Custom MultiQC configuration file.                               |
| `--publish_dir_mode`| How results are published (`copy` by default).                   |
| `--email`           | Email address for the completion summary.                        |

The full list, with descriptions, is printed by `nextflow run danilodileo/nextflow-workshop-lnu --help`.

## Profiles

Select a profile with `-profile` (comma-separated to combine, e.g. `-profile test,docker`):

- `docker`, `singularity`, `apptainer`, `podman`, `shifter`, `charliecloud`: container engines.
  On Apple Silicon, add `arm` (`-profile arm,docker`) so images are run as `linux/amd64`.
- `conda`, `mamba`: run with Conda environments instead of containers.
- `test`: run a minimal public dataset, ideal to check your setup.
- `debug`: keep the work directory and print the host name for each task.

## Resources

Process resources are defined by labels in [`conf/base.config`](../conf/base.config). To cap what a
run may request (for example on a laptop), pass a small config file with `-c`:

```groovy
process.resourceLimits = [cpus: 4, memory: '16.GB', time: '4.h']
```

Individual tools can be tuned in the same way, for instance extra FastQC arguments:

```groovy
process {
    withName: FASTQC {
        ext.args = '--quiet --nogroup'
    }
}
```

## Reproducibility

Pin a release or commit with `-r` so that a run can be repeated exactly:

```bash
nextflow run danilodileo/nextflow-workshop-lnu -r <release-or-commit> -profile docker ...
```
