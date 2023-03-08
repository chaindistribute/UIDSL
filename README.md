# LayoutDM: Discrete Diffusion Model for Controllable Layout Generation (CVPR2023)

## Setup
Here we describe the setup required for the model training and evaluation.

### Requirements
We check the reproducibility under this environment.
- Python3.7
- CUDA 11.3
- [PyTorch](https://pytorch.org/get-started/locally/) 1.12

We recommend using Poetry (all settings and dependencies in [pyproject.toml](pyproject.toml)).
Pytorch-geometry provides independent pre-build wheel for a *combination* of PyTorch and CUDA version (see [PyG:Installation](https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html
) for details). If your environment does not match the one above, please update the dependencies.


### How to install
1. Install poetry. We recommend to make a virtualenv and install poetry inside it.)

```bash
pip3 install poetry
```

2. Install dependencies (it may be slow..)

```bash
poetry install
```

3. Download resources and unzip

``` bash
wget https://storage.googleapis.com/ailab-public/layoutdm/layoutdm_starter.zip
unzip layoutdm_starter.zip
```

The data is decompressed to the following structure:
```
download
- clustering_weights
- datasets
- fid_weights
- pretrained_weights
```

## Experiment

Note: our main framework is based on [hydra](https://hydra.cc/). It is convenient to handle dozens of arguments hierarchically but may require some additional efforts if one is new to hydra.

### Demo
We provide the trained models [here](download/pretrained_weights). Please run a jupyter notebook in [notebooks/demo.ipynb](notebooks/demo.ipynb). You can get and render the results of six layout generation tasks on two datasets (Rico and PubLayNet).

### Training
You can also train your own model from scratch, for example by

```bash
bash bin/train.sh rico25 layoutdm
```

, where the first and second argument specifies the dataset ([choices](src/trainer/trainer/config/dataset)) and the type of experiment ([choices](src/trainer/trainer/config/experiment)), respectively.
Note that for training/testing, style of the arguments is `key=value` because we use hydra, unlike popular `--key value` (e.g., [argparse](https://docs.python.org/3/library/argparse.html)).

### Testing

```bash
poetry run python3 -m src.trainer.trainer.test \
    cond=<COND> \
    job_dir=<JOB_DIR> \
    result_dir=<RESULT_DIR>
```
`<COND>` can be: (unconditional, c, cwh, partial, refinement, relation)

For example, if you want to test the provided LayoutDM model on `C->S+P`, the command is as follows:
```
poetry run python3 -m src.trainer.trainer.test cond=c dataset_dir=./download/datasets job_dir=./download/pretrained/layoutdm_rico result_dir=tmp/dummy_results
```

Please refer to [TestConfig](src/trainer/trainer/hydra_configs.py#L12) for more options available.

### Evaluation
```bash
poetry run python3 eval.py <RESULT_DIR>
```

## Citation

If you find this code useful for your research, please cite our paper:

```
@inproceedings{inoue2023layout,
    title={{LayoutDM: Discrete Diffusion Model for Controllable Layout Generation}},
    author={Naoto Inoue and Kotaro Kikuchi and Edgar Simo-Serra and Mayu Otani and Kota Yamaguchi},
    booktitle={The IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    year={2023},
    pages={XXXX-XXXX},
    doi={XXXX}
  }
```