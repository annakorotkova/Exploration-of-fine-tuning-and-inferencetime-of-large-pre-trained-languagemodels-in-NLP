# Exploration of fine-tuninung and inference time of large pre-trained language models in NLP

This repository contains all datasets, Python scripts, sweep configuration files and plots that were used in the scope of this master's thesis.

To run the analyses, the following requirements must be met:
- Python, version 3.8
- Visual Studio Code, version 1.48 
- Jupyter Notebook, version 6.1.4

This repository contains the following folders and files:

- *Master's Thesis\_Korotkova.pdf*:  the scientific work in .pdf format
- *GLUE.ipynb*
	- download GLUE tasks data following the instructions on https://github.com/nyu-mll/GLUE-baselines
	- some superficial analyses of the GLUE tasks
- https://github.com/annakorotkova/transformers: modified `transformers` module
	- the module `transformers` by *huggingface* (version 3.0.0) was forked from https://github.com/huggingface/transformers
	- added argument *finetuning_iters* in *training\_args.py* to pass number of fine-tuning iterations (default: 3)
	- measurement of the fine-tuning time was integrated in the script *trainer.py* inside the *train()* function
	- measurement of inference time was integrated to the script *trainer.py* inside the *_prediction\_loop()* function, which was later used by the *evaluate()* function
- *sweep files* contains two folders
	- the sub-folder *finetuning time* contains the sweep configuration files used in the analysis to perform the measuring of fine-tuning time
	- the sub-folder *inference time* contains the sweep configuration files used in the analysis to perform the measuring of fine-tuning time. 
	  Those files are additionally marked with "inf" in their file name
	- files that include "unc" in their name incorporate the configuration for time measurement for all regarded models, except for "distilbert-base-cased" and "bert-base-cased"
	- files that include "cased" in their name incorporate the time measurement for "distilbert-base-cased" and "bert-base-cased"
	-> this distinction was performed since fine-tuning "distilbert-base-cased" and "bert-base-cased" failes on several GLUE tasks
	- files that include "all" in their name incorporate the configuration for time measurement for all regarded models
	- files that include "xlnet" in their name incorporate the configuration for time measurement for \texttt{xlnet-base-cased}
	-> this distinction was performed since \texttt{xlnet-base-cased} takes long fine-tuning times and could therefore not be fine-tuned for all GLUE tasks
	- Note! For some configurations there only exists the configuration file for fine-tuning time measurement for "distilbert-base-cased" and "bert-base-cased". 
	  This is the case for tasks where the fine-tuning of these models already failed. Hence, the inference time could not be measured.
	- files containing the word "crashed" are configuration files created for runs that crashed before and should be rerun
	- files containing the word "reduced" contain a limited sweep since it is run for try out reasons (if prior runs failed for example)

Hereby, the fine-tuning and the measurement of the fine-tuning as well as inference time was performed on a virtual machine.
To execute the measurement of the fine-tuning and inference time, respectively, the following steps were executed:
- Cloning of the forked and modified repository https://github.com/annakorotkova/transformers
- Adding downloaded GLUE files to workspace folder in Visual Studio Code
- Creating sweep configurations
- Run sweeps with the following commands in the terminal:
	- To enable an execution of the measurements without crashing if the computer turn to standby mode, the command `screen -S mysession` should be used
        - `wandb sweep {}` with {} being the placeholder for the respective sweep name, e.g. sweep_wnli_cased.yaml
           -> `wandb sweep sweep_wnli_cased.yaml`
        - The terminal outputs some sweep characteristics (such as sweep ID or the link where the sweep can be viewed). 
	  It also puts out a line of the form `Run sweep agent with: wandb agent anna\_korotkova/transformers-examples\_text-classification/o7ifco7k`
          This line should be copied and executed in the terminal

After following these steps, the fine-tuning for the respective sweep should be performed. Hereby should be noted that a wandb account is required in order to enable this process. The logged runs from my analysis can be retrieved at https://wandb.ai/anna_korotkova/transformers-examples_text-classification/sweeps?workspace=user-anna_korotkova

Furthermore, the electronic annex incorporates the following files and sub-folders:

- *finetuning*: contains contains all Excel datasets which incorporate the fine-tuning time data that was used in the analysis
- *inference time*: contains all Excel datasets which incorporate the inference time data which was used in the analysis
- *Analysis finetuning time.ipynb*
	- Descriptive analysis of the minimum fine-tuning time of finished runs for different hyperparameter combinations
	- ANOVAs of fine-tuning time, grouped by task, model, maximum sequence length and batch size, respectively
	- Log-linear regression with log. fine-tuning time as the dependent variable 
	- Regression Random Forest with log. fine-tuning time as the dependent variable
	- Exploration of the relation between fine-tuning time and accuracy
- *Analysis inference time.ipynb*
	- Descriptive analysis of the minimum inference time of finished runs for different hyperparameter combinations
	- ANOVAs of fine-tuning time, grouped by task, model, maximum sequence length and batch size, respectively
	- Log-linear regression with log. inference time as the dependent variable
	- Regression Random Forest with log. inference time as the dependent variable 
	- Exploration of the relation between inference time and accuracy
- *plot* contains two sub-folders
	- the sub-folder *fine-tuning time* contains all plots that were generated during the analysis of the fine-tuning time (run script *Analysis finetuning time.ipynb*)
	- the sub-folder *inference time* contains all plots that were generated during the analysis of the inference time (run script *Analysis inference time.ipynb*)
- *finetuning_time_table.xlsx*: Table of fine-tuning times grouped by model, number of examples (task size), maximum sequence length and batch size 
- *inference_time_table.xlsx*: Table of inference times grouped by model, number of examples (task size), maximum sequence length and batch size 
