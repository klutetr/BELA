# Bi-encoder Entity Linking Architecture (BELA)

The end to end transformer based model for entity linking in 98 languages. The BELA architecture is described in the following paper: [Multilingual End to End Entity Linking](https://arxiv.org/pdf/2306.08896.pdf).
## Install package and requirements
```
pip install -r requirements.txt
```

Then install the package (`-e` for dev mode)
```
pip install -e .
```


## Runing tests

```
PYTHONPATH=.:$PYTHONPATH python -m unittest
```

## Bela
### Pretrain disambiguation model on wikipedia data

The default path to the data is `/fsx/movb/data/matcha`. You need to modify config with appropriate paramters in `bela/conf/joint_el_disambiguation_only.yaml`. This is Hydra config in YAML format. Please read more about [hydra here](https://hydra.cc/) and [yaml here](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html). To run training (you should be on machine with GPU):

```
PYTHONPATH=.:$PYTHONPATH python bela/main.py --config-name joint_el_disambiguation_only
```

### Train model on entity linkind data

Config is stored in `bela/conf/joint_el.yaml`. To run training (you should be on machine with GPU):

```
PYTHONPATH=.:$PYTHONPATH python bela/main.py --config-name joint_el
```

### Train model using SLURM

```
PYTHONPATH=.:$PYTHONPATH python bela/main.py -m --config-name joint_el_disambiguation_only trainer=slurm trainer.num_nodes=1 trainer.gpus=8
```

## MBlink (Multilingual Blink)
### Train on in-batch negatives (english, xlmr-base)

```
PYTHONPATH=.:$PYTHONPATH python mblink/main.py --config-name blink_xlmr trainer.gpus=8
```

Available configs:
* blink_xlmr - English, xlmr-base
* blink_xlmr_large - English, xlmr-large

### Train model using SLURM

```
PYTHONPATH=.:$PYTHONPATH python mblink/main.py -m --config-name blink_xlmr trainer=slurm
```

## Evaluation Data
### TAC-KBP and LORELEI
Create an account on ldc.upenn.edu (your organization needs to have a membership) and download the following files:
```
url2filename = {
    "https://catalog.ldc.upenn.edu/download/620b6e19e16a4e269a3fe538e66448e5e4fe2ba0ab40e6f5f879cd2379c5": "lorelei_ukrainian_repr_lang_pack_LDC2020T24.zip",  # LDC2020T24
    "https://catalog.ldc.upenn.edu/download/992f6486049740ec9112d4b618122b6138efa2ca852d26dc1075890dfbf4": "lorelei_tigrinya_incident_lang_pack_LDC2020T22.zip",  # LDC2020T22
    "https://catalog.ldc.upenn.edu/download/4b033265a3e6f384b365af321dfce154447e7ec4ebe339e3995f4ad823ba": "lorelei_edl_kb_LDC2020T10.zip",  # LDC2020T10
    "https://catalog.ldc.upenn.edu/download/00284035a9a26715ff4ff6c995f4bc083ccec32f3dc5459e4c872d1bdfca": "lorelei_vietnamese_repr_lang_pack_LDC2020T17.zip",  # LDC2020T17
    "https://catalog.ldc.upenn.edu/download/ded6a54371683dded832b402bd3fc761a83ad4414ce7c9e3e831f622135b": "lorelei_amharic_lang_pack_mono_para_txt_LDC2018T04.zip",  # LDC2018T04
    "https://catalog.ldc.upenn.edu/download/b7725efb67a41040c11d0665d3397e3dae111f592ce2f2974c3bcb5dad74": "lorelei_somali_mono_para_txt_LDC2018T11.zip",  # LDC2018T11
    "https://catalog.ldc.upenn.edu/download/539e454f92a540e577fbe3200ab22ada71aa6000477d3ac47099763ebeb8": "lorelei_oromo_incident_lang_pack_LDC2020T11.tgz",  # LDC2020T11
    "https://catalog.ldc.upenn.edu/download/c57f38a07e4fb3b73712d0edbe9f4765776a177af2a75783266df069e604": "tac_kbp_ent_disc_link_comp_train_eval_2014-2015_LDC2019T02.zip",  # LDC2019T02
}
```
WARNING: The extension is marked as zip here based on previous code, but it's should probably be .tar.gz


Then they can be extracted using
```
tar -xzvf tac_kbp_ent_disc_link_comp_train_eval_2014-2015_LDC2019T02.zip
```

## Troubleshooting Instalation
If you encounter issues during installation, try the following:
•	Dependency issues: Ensure you’re using the correct Python version (3.8+) and compatible PyTorch version. Try updating dependencies with pip install --upgrade -r requirements.txt.
•	GPU issues: Make sure CUDA is installed and correctly set up for your machine. Use nvidia-smi to check GPU status.

## Expanded Usage Examples
Example 1: Training for Entity Linking (English)
PYTHONPATH=.:$PYTHONPATH python bela/main.py --config-name joint_el

Example 2: Training for Multilingual Disambiguation
PYTHONPATH=.:$PYTHONPATH python bela/main.py --config-name joint_el_disambiguation_only

## Contribution Guidelines
To contribute, follow these steps:

1.	Fork the repository.
2.	Clone your fork:
    git clone https://github.com/<your-username>/BELA.git
3.  Create a new branch:
    git checkout -b new-feature-branch
4.	Make your changes and ensure all tests pass:
    PYTHONPATH=.:$PYTHONPATH python -m unittest
5.	Submit a pull request to the original repository.

## License
BELA is MIT licensed. See the [LICENSE](LICENSE) file for details.
