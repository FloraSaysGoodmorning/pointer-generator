 `export EXPERIMENT_NAME=pretrained_model_tf1.2.1`
This should be the same as the model name in gcs

### Step 1 Get the eval data  
From notebook instance, create a new folder called `cnn_data/`, cd into it and download the Processed data  
> `curl -c /tmp/cookies "https://drive.google.com/uc?export=download&id=0BzQ6rtO2VN95a0c3TlZCWkl3aU0" > /tmp/intermezzo.html`

> `curl -L -b /tmp/cookies "https://drive.google.com$(cat /tmp/intermezzo.html | grep -Po 'uc-download-link" [^>]* href="\K[^"]*' | sed 's/\&amp;/\&/g')" > processed_cnn_dailymail_data.zip`

> `unzip processed_cnn_dailymail_data.zip`

They're chunks of data `train_000.bin`, `train_001.bin`, ...  
instead of just a single `train.bin`, `val.bin` and `test.bin`,  according to [here](https://github.com/abisee/cnn-dailymail/issues/3)  

These chunked files will be saved in `cnn_data/finished_files/chunked`, there's 287 chunks of training data, 13 chunks of eval data, 11 chunks of test data. Vocabulary which is used as input and output vocab is in `finished_files/vocab`

### Step 2 Install pyrouge  
> `pip3 install pyrouge`  

Download the rouge 1.5.5 folder from gcs if it's not already under root of this repo [Origin of the folder](https://github.com/andersjo/pyrouge/tree/master/tools/ROUGE-1.5.5)  
> `gsutil cp -r gs://amlg-dev-packages/ROUGE-1.5.5/ /home/jupyter/pointer-generator/`  

Set the path in terminal according to [here](https://github.com/bheinzerling/pyrouge#installation)   
> `set pyrouge_set_rouge_path /home/jupyter/pointer-generator/ROUGE-1.5.5`

### Step 3 Download trained model
Make a new folder for logs `pg-log` under root, download pretrained pointer-generator from gcs  
`gsutil cp gs://amlg-dev-packages/pointer-generator/pretrained_pg_in_tf1.2.1.zip /home/jupyter/pg-log/`  
`cd /home/jupyter/pg-log/`
`unzip pretrained_pg_in_tf1.2.1.zip`

### Finally: infer
```
python3 run_summarization.py \
--mode=decode \
--data_path=/home/jupyter/cnn_data/finished_files/chunked/val_000.bin \
--vocab_path=/home/jupyter/cnn_data/finished_files/vocab \
--log_root=/home/jupyter/pg-log \
--exp_name=$EXPERIMENT_NAME
```

### Credits  
Processed CNN/Daily mail data is here
https://drive.google.com/file/d/0BzQ6rtO2VN95a0c3TlZCWkl3aU0/view?usp=sharing  
Thanks to [JafferWilson](https://github.com/JafferWilson/Process-Data-of-CNN-DailyMail)
