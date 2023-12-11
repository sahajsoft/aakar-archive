# aakaar


## Installation instructions

* Install python3 and virtualenv if it is not installed for your platform

> For Ubuntu: `sudo apt-get install virtualenv`

> For OSX: `brew install virtualenv`

* Clone this repository

> `git clone git@gitlab.com:sahajsoft/aakaar.git`

* Inside aakaar directory create a virtualenv with python3

> `cd aakaar/`
> `virtualenv -p python3 venv`

* Source virtualenv

> `source venv/bin/activate~

* Install all required pip dependencies

> `pip install -r requirements.txt`

* Outside aakaar directory clone tensorflow models repo

> `git clone https://github.com/tensorflow/models`

* In your shell rc file import the path for the slim form models/research directory

> `vi ~/.bashrc` or `vi ~/.zshrc` depending on the shell you are using
> Add line at the end `export PYTHONPATH=$PYTHONPATH:<Directory_To_Models_Parent_Folder>/models/research:<Directory_To_Models_Folder>/models/research/slim`

* Git clone cocoapi and make

> `git clone https://github.com/cocodataset/cocoapi.git`
> `cd cocoapi/PythonAPI`
> `make`

* Copy pycoco tools to the models

> `cp -r pycocotools <Directory_To_Models_Parent_Folder>/models/research/`

* Generate protobuf files from the config

Ensure that correct version of protobuf is installed. I had 3.5+ installed.

> `cd <Directory_To_Models_Parent_Folder>/models/research/`
> `protoc object_detection/protos/*.proto --python_out=.`

* Run simple tensorflow test to ensure everything is correctly installed

> cd `<Directory_To_Models_Parent_Folder>/models/research`
> `python object_detection/builders/model_builder_test.py`

## Execution 

Local Machine

```
PIPELINE_CONFIG_PATH="/Users/priyank/Projects/aakaar/model/config"
MODEL_DIR="/Users/priyank/Projects/aakaar/model/"
NUM_TRAIN_STEPS=50000
SAMPLE_1_OF_N_EVAL_EXAMPLES=1
python object_detection/model_main.py \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --model_dir=${MODEL_DIR} \
    --num_train_steps=${NUM_TRAIN_STEPS} \
    --sample_1_of_n_eval_examples=$SAMPLE_1_OF_N_EVAL_EXAMPLES \
    --alsologtostderr
```

AWS GPU Instance

```
PIPELINE_CONFIG_PATH="/home/ubuntu/projects/aakaar/model/config.server"
MODEL_DIR="/home/ubuntu/projects/aakaar/model/"
NUM_TRAIN_STEPS=50000 
SAMPLE_1_OF_N_EVAL_EXAMPLES=1
python object_detection/model_main.py \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \    
    --model_dir=${MODEL_DIR} \
    --num_train_steps=${NUM_TRAIN_STEPS} \
    --sample_1_of_n_eval_examples=$SAMPLE_1_OF_N_EVAL_EXAMPLES \
    --alsologtostderr
```

Exporting frozen reference graph

```
python /Users/priyank/PetProjects/models/research/object_detection/export_inference_graph.py --input_type image_tensor --pipeline_config_path ~/Projects/aakaar/model/config --trained_checkpoint_prefix ~/Projects/aakaar/model/model.ckpt-50000.data-00000-of-00001 --output_directory model_output
```