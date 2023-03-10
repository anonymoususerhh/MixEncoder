## Prepare my model

move `./my_transformer/out_vector_modeling_bert.py` to the dictionary `models/bert` under the path where lib transformers is installed, like

```bash
~/anaconda3/envs/your_envs_name/lib/python(3.8)/site-packages/transformers/models/bert`
```

## Prepare data

All datasets are saved in ./dataset/, this dictionary is created by scripts automatically.

### ubuntu
download data form [here](https://www.dropbox.com/s/2fdn26rj6h9bpvl/ubuntudata.zip?dl=0) and put them into ```./raw_data/ubuntu```

then modify the ```root_path``` in  ```./raw_data/data_scripts/process_dstc.py``` before running the following command:
```shell
cd ./raw_data/data_scripts
python process_ubuntu.py
```
### dstc7

download data (include augmented) and put them into ./raw_data/dstc， then

```shell
cd ./raw_data/data_scripts
python process_dstc.py
```

### mnli

Data will be downloaded by scripts automatically.

### hard negative msmarco (hard_msmarco)
data from [here](https://github.com/beir-cellar/beir/blob/main/examples/retrieval/training/train_msmarco_v3.py)

## Training

Commands vary with tasks and models. Some examples are provided in run_command. Readers can refer to nlp_main.py to make clear what arguments mean.

---

There are something should be paid attention to.

1. While training for classification tasks, arg: `label_num` should be set according to tasks.
2. While training PolyEncoder for matching tasks, arg: `label_num` should be set as 0.
3. While training for matching tasks, arg: `step_max_query_num` can be used to tackle cuda out of memory.

## Test

Test will be automatically done after training.

If you want to do test but not train, appending following arguments to the command.

```shell
--no_train --do_test(do_val)
```

do_val means using dev data while do_test means using test data.

If you want to measure the running time of models' doing match in the way like real worlds, please replace `do_test` 
with `do_real_test`, like

```shell
--no_train --do_real_test --query_block_size 100
```

Arg: `query_block_size` controls the number of simultaneously processed queries. This should be set according to cuda memory. Our experiments show that this argument has little influence on testing time.