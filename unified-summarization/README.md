# Unified Summarization

This code is written in Tensorflow (version 1.1.0).


## CNN/Daily Mail dataset

Codes for generating the dataset is in `data_preprocess` folder.

The usage is same as https://github.com/abisee/cnn-dailymail


## How to train

Use the sample scripts in `scripts` folder. 

### Pretrain the exatrctor

```
sh scripts/selector.sh
```
The trained models will be saved in `log/selector/${YOUR_EXP_NAME}` directory.

### Pretrain the abstracter

```
sh scripts/rewriter.sh
```
The trained models will be saved in `log/rewriter/${YOUR_EXP_NAME}` directory.

### End-to-end training the unified model

Set the path of pretrained extractor and abstractor to `SELECTOR_PATH` and `REWRITER_PATH` in line 18 and 19.

```
sh scripts/end2end.sh
```

The trained models will be saved in `log/end2end/${YOUR_EXP_NAME}` directory.

## How to evaluate (concurrent)

To evaluate the model during training, change the `MODE` in the script to `eval` (i.e., `MODE='eval'`) and run the script simutanously with train script (i.e., `MODE='train'`).

For evaluating the abstracter and the unified model, you can choose to evaluate the loss or ROUGE scores. Just switch the `EVAL_METHOD` in the script between `loss` and `rouge`. 

For the ROUGE evaluation, you can use greedy search or beam search. Just switch the `DECODE_METHOD` in the script between `greedy` and `beam`.

We highly recommend you to use **greedy search** for concorrent ROUGE evaluation since greedy search is much faster than beam search.
It takes about 30 minutes for greedy search and 7 hours for beam search on CNN/Daily Mail test set.

The current best models will be saved in `log/${MODEL}/${YOUR_EXP_NAME}/eval_${DATA_SPLIT}(_${EVAL_METHOD})`.

## How to evaluate with ROUGE on test set

Change the `MODE` in the script to `evalall` (i.e., `MODE='evalall'`) and set the `CKPT_PATH` as the model path that you want to test. 