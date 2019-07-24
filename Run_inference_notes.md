Processed CNN/Daily mail data is here
https://drive.google.com/file/d/0BzQ6rtO2VN95a0c3TlZCWkl3aU0/view?usp=sharing  
Thanks to https://github.com/JafferWilson/Process-Data-of-CNN-DailyMail

1. From notebook instance, create a new folder called `cnn_data/`, cd into it and download the Processed data  
> `curl -c /tmp/cookies "https://drive.google.com/uc?export=download&id=0BzQ6rtO2VN95a0c3TlZCWkl3aU0" > /tmp/intermezzo.html`

> `curl -L -b /tmp/cookies "https://drive.google.com$(cat /tmp/intermezzo.html | grep -Po 'uc-download-link" [^>]* href="\K[^"]*' | sed 's/\&amp;/\&/g')" > processed_cnn_dailymail_data.zip`

> `unzip processed_cnn_dailymail_data.zip`

They're chunks of data `train_000.bin`, `train_001.bin`, ...  
instead of just a single `train.bin`, `val.bin` and `test.bin`,  according to [here](https://github.com/abisee/cnn-dailymail/issues/3)  

These chunked files will be saved in `cnn_data/finished_files/chunked`, there's 287 chunks of training data, 13 chunks of eval data, 11 chunks of test data.

2. Infer
```
python run_summarization.py \
--mode=eval \
--data_path=/home/jupyter/cnn_data/finished_files/chunked/val_000.bin \
--vocab_path=/home/jupyter/cnn_data/finished_files/vocab \
--log_root=/home/jupyter/pg-log \
--exp_name=not_changing_anything
```
