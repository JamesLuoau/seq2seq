## Generate Data
```bash
DATA_TYPE=reverse ./bin/data/toy.sh

export VOCAB_SOURCE=${HOME}/nmt_data/toy_reverse/train/vocab.sources.txt
export VOCAB_TARGET=${HOME}/nmt_data/toy_reverse/train/vocab.targets.txt
export TRAIN_SOURCES=${HOME}/nmt_data/toy_reverse/train/sources.txt
export TRAIN_TARGETS=${HOME}/nmt_data/toy_reverse/train/targets.txt
export DEV_SOURCES=${HOME}/nmt_data/toy_reverse/dev/sources.txt
export DEV_TARGETS=${HOME}/nmt_data/toy_reverse/dev/targets.txt

export DEV_TARGETS_REF=${HOME}/nmt_data/toy_reverse/dev/targets.txt
export TRAIN_STEPS=1000

```

## Training Toy Data
```bash
export MODEL_DIR=./models/toy/
mkdir -p $MODEL_DIR

python -m bin.train \
  --config_paths="
      ./example_configs/nmt_small.yml,
      ./example_configs/train_seq2seq.yml,
      ./example_configs/text_metrics_bpe.yml" \
  --model_params "
      vocab_source: ${HOME}/nmt_data/toy_reverse/train/vocab.sources.txt
      vocab_target: ${HOME}/nmt_data/toy_reverse/train/vocab.targets.txt" \
  --input_pipeline_train "
    class: ParallelTextInputPipeline
    params:
      source_files:
        - ${HOME}/nmt_data/toy_reverse/train/sources.txt
      target_files:
        - ${HOME}/nmt_data/toy_reverse/train/targets.txt" \
  --input_pipeline_dev "
    class: ParallelTextInputPipeline
    params:
       source_files:
        - ${HOME}/nmt_data/toy_reverse/dev/sources.txt
       target_files:
        - ${HOME}/nmt_data/toy_reverse/dev/targets.txt" \
  --batch_size 32 \
  --train_steps 1000 \
  --output_dir ./models/toy
  
  
  
  

python -m bin.train \
  --config_paths="
      ./example_configs/nmt_small.yml,
      ./example_configs/train_seq2seq.yml,
      ./example_configs/text_metrics_bpe.yml" \
  --model_params "
      vocab_source: $VOCAB_SOURCE
      vocab_target: $VOCAB_TARGET" \
  --input_pipeline_train "
    class: ParallelTextInputPipeline
    params:
      source_files:
        - $TRAIN_SOURCES
      target_files:
        - $TRAIN_TARGETS" \
  --input_pipeline_dev "
    class: ParallelTextInputPipeline
    params:
       source_files:
        - $DEV_SOURCES
       target_files:
        - $DEV_TARGETS" \
  --batch_size 32 \
  --train_steps $TRAIN_STEPS \
  --output_dir $MODEL_DIR
```