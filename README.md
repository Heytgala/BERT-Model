# BERT

PRE-TRAINING MODEL:

1) py -3.6 -m pip install six 
2) py -3.6 -m pip install protobuf==3.6.1
3) py -3.6 -m pip install tensorflow==1.15
4) py -3.6 create_pretraining_data.py --input_file=./sample_text.txt --output_file=./tf_examples.tfrecord --vocab_file=./vocab.txt --do_lower_case=True  --max_seq_length=128 --max_predictions_per_seq=20 --masked_lm_prob=0.15 --random_seed=12345 --dupe_factor=5
5) py -3.6 run_pretraining.py \ --input_file=./tf_examples.tfrecord \ --output_dir=./pretraining_output \ --do_train=True \ --do_eval=True \ --bert_config_file=./bert_config.json \ --train_batch_size=32 \ --max_seq_length=128 \ --max_predictions_per_seq=20 \ --num_train_steps=20 \ --num_warmup_steps=10 \ --learning_rate=2e-5           

Fine-Tune Model:
1) py -3.6 run_squad.py \ --vocab_file=./vocab.txt \ --bert_config_file=./bert_config.json \  --init_checkpoint=./bert_model.ckpt \ --do_train=True \ --train_file=./train-v1.1.json \ --do_predict=True \ --predict_file=./dev-v1.1.json \  --train_batch_size=12 \ --learning_rate=3e-5 \  --num_train_epochs=2.0 \ --max_seq_length=384 \ --doc_stride=128 \
 --output_dir=./squad_base/  

Sentence Classification:
1) py -3.6 run_classifier.py \ --task_name=MRPC \ --do_train=true \ --do_eval=true \ --data_dir=./MRPC \ --vocab_file=./vocab.txt \ --bert_config_file=./bert_config.json \ --init_checkpoint=./bert_model.ckpt \ --max_seq_length=128 \ --train_batch_size=32 \ --learning_rate=2e-4 \ --num_train_epochs=3.0 \ --output_dir=./mrpc_output/

--- MRPC Data ---
https://github.com/MegEngine/Models/tree/master/official/nlp/bert/glue_data/MRPC
