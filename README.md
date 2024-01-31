# Pass-Tuning: Towards Structure-Aware Parameter-Efficient Tuning for Code Representation Learning

This is the repository of EMNLP 2023 paper Pass-Tuning: Towards Structure-Aware Parameter-Efficient Tuning for Code Representation Learning.

## Introduction

We propose a structure-aware parameter-efficient tuning method for code representation learning, using a plug-and-play GAT module to exploit code structural information and can serve as an alternative to full fine-tuning.

![Pass-Tuning-Overview](https://osspicgo.oss-cn-shanghai.aliyuncs.com/img/Pass-Tuning-Overview.pdf)

More details are provided in our EMNLP'23 paper and [our paper](https://aclanthology.org/2023.findings-emnlp.42/).

## Environment & Preparing

```shell
conda create --name cat python=3.7
conda activate cat
pip install -r requirements.txt
git clone https://github.com/nchen909/CodePrompt
cd CodePrompt/evaluator/CodeBLEU/parser
bash build.sh
cd ../../../
cp evaluator/CodeBLEU/parser/my-languages.so build/
#make sure git-lfs installed like 'apt-get install git-lfs'
bash get_models.sh
```

for cuda11.0+,

```
pip install torch==1.7.0+cu110 torchvision==0.8.1+cu110 torchaudio===0.7.0 -f https://download.pytorch.org/whl/torch_stable.html
```

for torch geometric,

https://pytorch-geometric.com/whl/torch-1.6.0%2Bcu101.html


## Preparing data

The dataset comes from [CodeXGLUE](https://github.com/microsoft/CodeXGLUE).

```shell
mkdir data
cd data
pip install gdown
gdown https://drive.google.com/uc?export=download&id=1BBeHFlKoyanbxaqFJ6RRWlqpiokhDhY7
unzip data.zip
rm data.zip
```

### Preparing local path

Direct WORKDIR, HUGGINGFACE_LOCALS in run.sh, run_few_shot.sh to your path.

## Finetuning tasks

```bash
export MODEL_NAME=
export TASK=
export SUB_TASK=
# to run one task
bash run.sh $MODEL_NAME $TASK $SUB_TASK
# to run few shot
bash run_few_shot.sh $MODEL_NAME $TASK $SUB_TASK
# to run multi task
bash run_multi_task.sh
```

  `MODEL_NAME` can be any one of `["roberta", "codebert", "graphcodebert", "unixcoder","t5","codet5","bart","plbart"]`.

  `TASK` can be any one of `['summarize', 'translate', 'refine', 'generate', 'defect', 'clone']`. (generate refers concode in codexglue, and we don't consider complete)

  `SUB_TASK` can be in picture below

![Finetuning tasks](https://osspicgo.oss-cn-shanghai.aliyuncs.com/img/20221027202855.png)

| Category | Dataset   | Task              | Sub_task(LANG)                                     | Type           | Category | Description                                                                                                                  |
| -------- | --------- | ----------------- | -------------------------------------------------- | -------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| C2C      | BCB       | clone             | [] (java)                                          | bi-directional | encoder  | code summarization task on[CodeSearchNet](https://arxiv.org/abs/1909.09436) data with six PLs                                   |
| C2C      | Devign    | defect            | [] (c)                                             | bi-directional | encoder  | text-to-code generation on[Concode](https://aclanthology.org/D18-1192.pdf) data                                                 |
| C2C      | CodeTrans | translate         | ['java-cs', 'cs-java’]                            | end2end        | en2de    | code-to-code translation between[Java and C#](https://arxiv.org/pdf/2102.04664.pdf)                                             |
| C2C      | Bugs2Fix  | refine(repair)    | ['small','medium'] (java)                          | end2end        | en2de    | code refinement on[code repair data](https://arxiv.org/pdf/1812.08693.pdf) with small/medium functions                          |
| C2T      | CodeSN    | summarize         | ['java', 'python', 'javascript','php','ruby','go'] | end2end        | en2de    | code defect detection in[C/C++ data](https://proceedings.neurips.cc/paper/2019/file/49265d2447bc3bbfe9e76306ce40a31f-Paper.pdf) |
| T2C      | CONCODE   | generate(concode) | [] (java)                                          | end2end        | en2de    | code clone detection in[Java data](https://arxiv.org/pdf/2002.08653.pdf)                                                        |

## Scripts for ablation studies

### PEL strategies

run.sh（full finetuning）

run_adapter.sh

run_bitfit.sh

run_prefix_tuning.sh (for P-Tuning V2)

### Prefix

run_prefix_tuning.sh (MLP)

run_gcn_tuning.sh (GCN)

run_pass_tuning.sh (GAT)

### Initialization

run_random_selection.sh (without retriving)

## Citation

Please consider citing us if you find this repository useful.👇

```bibtex
@inproceedings{chen2023passtuning,
  title={Pass-Tuning: Towards Structure-Aware Parameter-Efficient Tuning for Code Representation Learning},
  author={Chen, Nuo and Sun, Qiushi and Wang, Jianing and Li, Xiang and Gao, Ming},
  booktitle = {EMNLP},
  year={2023}
}
```
