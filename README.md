# Identify candidate motifs responsible of STR-initiating RNA

## Introduction

This is a part of the study ["Impact of transcription initiation at microsatellites on gene expression" M. Grapotte *et al.*, 2024](linkToTheArticle). In this part, we aim to identify candidate motifs responsible of STR-initiating RNA with HOMER findMotifs.pl.

## Requirements

[Nextflow](https://www.nextflow.io/) need to be installed on your system. If it is not the case,  you can install it with the following the instructions [here](https://www.nextflow.io/docs/latest/getstarted.html#installation).

[Conda](https://docs.conda.io/en/latest/) need to be installed on your system. If it is not the case, you can install it with the following the instructions [here](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

## Download the data

The data used in this study is available [here](liketotheData). The data is composed of 4 files and 1 directory:

- `mnnModels/`: a directory containing the hyperparameters and the parameters of the different MNN models.
- `hg38.hipstr_reference.cage.500bp.around3end.fa`: a fasta file containing the sequences of the 500bp around the 3' end of all STRs.
- `hg38all_names_raw.npy` : a numpy file containing the names of all STRs.
- `hg38all_seqs_raw.npy` : a numpy file containing the one-hot encoded sequences of all STRs.
- `merged_results.txt` : a TSV file listing all the STR-gene association with a correct prediction of the CAGE signal.

Those files need to be downloaded and placed in the `data` directory.

## Launch the pipeline

To launch the pipeline, you need to execute the following command in the repository where the main.nf file is located:

```bash
nextflow main.nf
```

A `local1p` profile is used to run the pipeline on your local machine (option `-profile local1p`). It launches the pipeline with a single process. Some processes need a large amount of memory and can crash if you run the pipeline with too much parallel executions or on a machine with limited memory. The `-resume` option allows you to resume the pipeline from where it stopped if it was stopped for any reason.

## Results

The results of the pipeline are located in the `results` directory. Pregenerated results are available [here](linktotheResults).

## Issues

### IFB cluster

On the IFB cluster, there is a problem with the conda environment and the path to the python interpreter. To solve this problem, we need to use a singularity container. To do so, you need to execute the following command:

```bash
singularity build ./env/GeneExprRegBySTR.simg ./env/Singularity
```

Then, you can launch the pipeline normally.

### out-of-memory while creating the conda environment

The pipeline can crash with the following error:

```text
ERROR ~ Error executing process > 'GET_SEQ_NAMES_AND_ONE_HOT_BY_STR_CLASS (1)'

Caused by:
  Failed to create Conda environment
  command: mamba env create --prefix [DIR] --file [ENV_PATH]
  status : 137
  message:



 -- Check '.nextflow.log' file for details


WARN: Killing running tasks (1)

slurmstepd: error: Detected 1 oom-kill event(s) in StepId=37724626.0. Some of your processes may have been killed by the cgroup out-of-memory handler.
srun: error: cpu-node-3: task 0: Out Of Memory
```

Try to execute the command given after `command:` in the error message.

### strict repo priority while creating the conda environment

When creating the conda environment, you may encounter the following error:

```text
Encountered problems while solving:
  - package pytorch-2.2.0-py3.9_cuda12.1_cudnn8.9.2_0 is excluded by strict repo priority
```

It can be solved by executing the following command:

```bash
conda config --set channel_priority disabled
```

Then, try to create the environment again.

## citation

If you use this pipeline, please cite the following article:

```bibtex
@article{grapotte2024impact,
  title={Impact of transcription initiation at microsatellites on gene expression},
  author={Mathys Grapotte, Charles Lecellier , Christophe Vroland},
  journal={...},
  year={2024},
  publisher={...}
}
```
