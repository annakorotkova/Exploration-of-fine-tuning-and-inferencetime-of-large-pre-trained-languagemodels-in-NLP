# Exploration of fine-tuninung and inference time of large pre-trained language models in NLP

This repository contains all datasets, Python scripts, sweep configuration files and plots that were used in the scope of this master's thesis.\\

To run the analyses, the following requirements must be met:
\begin{itemize}
    \item Python, version 3.8 (\cite{python3})
    \item Visual Studio Code, version 1.48 
    \item Jupyter Notebook, version 6.1.4 (\cite{Jupyter.2016})
\end{itemize}

The enclosed USB flash drive contains the following folders and files which are located in the folder \textit{Master's Thesis Korotkova}:

\begin{itemize}
	\item \textit{Master's Thesis\_Korotkova.pdf}:  the present scientific word in .pdf format
	\item \textit{GLUE.ipynb}
	    \begin{itemize}
	        \item download GLUE tasks data following the instructions on \url{https://github.com/nyu-mll/GLUE-baselines}
	        \item some superficial analyses of the GLUE tasks
	    \end{itemize}
	\item \url{https://github.com/annakorotkova/transformers}: modified \texttt{transformers} module
	    \begin{itemize}
	        \item the module \texttt{transformers} by \textit{huggingface} (version 3.0.0) was forked from \url{https://github.com/huggingface/transformers}
	        \item Added argument \textit{finetuning\_iters} in \textit{training\_args.py}\footnote{\label{src}can be found in the folder \textit{src/transformers}} to pass number of iterations (default: 3)
	        \item measurement of the fine-tuning time was integrated in the script \textit{trainer.py}\footref{src} in \textit{train()} function
	        \item measurement of inference time was integrated to the script in \textit{\_prediction\_loop()} function, which was later used by the \textit{evaluate()} function\footnote{all modifications to the codes are marked with a comment "\# added by Anna"}
	    \end{itemize}
	\item \textit{sweep.zip}: contains all sweeps that were used in the analyses\footnote{Important to note: this file also contains sweep files of runs that crashed in order to rerun them. Hence, the content of some files might overlap. They were added to match the runs that are logged in wandb}
\end{itemize}

Hereby, the fine-tuning and the measurement of the fine-tuning as well as inference time was performed on a virtual machine.
To execute the measurement of the fine-tuning and inference time, respectively, the following steps were executed:
\begin{itemize}
    \item Cloning of the forked and modified repository \url{https://github.com/annakorotkova/transformers}
    \item Adding downloaded GLUE files to workspace folder in Visual Studio Code
    \item Creating sweep configurations
    \item Run sweeps with the following commands in the terminal:
        \begin{itemize}
            \item To enable an execution of the measurements without crashing if the computer turn to standby mode, the command "screen -S mysession" should be used\footnote{the session can be checked with the command "screen -r -d mysession"}
            \item "wandb sweep \{\}" with \{\} being the placeholder for the respective sweep name, e.g. sweep\_wnli\_cased.yaml.\\
            $\rightarrow$ \textit{wandb sweep sweep\_wnli\_cased.yaml}
            \item The terminal outputs some sweep characteristics (such as sweep ID or the link where the sweep can be viewed). It also puts out a line of the following form: "Run sweep agent with: wandb agent anna\_korotkova/transformers-examples\_text-classification/o7ifco7k"\footnote{o7ifco7k is the sweep ID (in wandb the runs are logged under this name)}\\
            This line should be copied and executed in the terminal
        \end{itemize}
\end{itemize}

\newpage
Figure \ref{fig:wandb_command} illustrates the last key point 

\begin{figure}[H]
    \begin{center}
    \includegraphics[width = \linewidth, height = 0.15\linewidth]{figures/wandb_command.PNG}
    \end{center}
    \vspace*{-5mm}
    \caption[Run sweep wandb]{Run a sweep, which is logged at wandb}
    \label{fig:wandb_command}
\end{figure}

After following these steps, the fine-tuning for the respective sweep should be performed. Hereby should be noted that a wandb account is required in order to enable this process. The logged runs from my analysis can be retrieved at \url{https://wandb.ai/anna_korotkova/transformers-examples_text-classification/sweeps?workspace=user-anna_korotkova}.\\

Furthermore, the electronic annex incorporates the following files and sub-folders:

\begin{itemize}
    \item \textit{finetuning.zip}: contains contains all Excel datasets which incorporate the fine-tuning time data that was used in the analysis (within the script \textit{Analysis finetuning time.ipynb})
	\item \textit{inference time.zip}: contains all Excel datasets which incorporate the inference time data which was used in the analysis (within the Python script \textit{Analysis inference time.ipynb})\footnote{all Excel files (in both subfolders) were downloaded from wandb. It should be noted that the files incorporated in both subfolder do not match exactly the wandb sweeps since some files were revised. This was done, so that no runs were included two times since some sweeps were rerun because they crashed the first time.}
    \item \textit{Analysis finetuning time.ipynb}
	    \begin{itemize}
	        \item Descriptive analysis of the minimum fine-tuning time of finished runs for different hyperparameter combinations
	        \item ANOVAs of fine-tuning time, grouped by task, model, maximum sequence length and batch size, respectively
	        \item Log-linear regression with log. fine-tuning time as the dependent variable 
	        \item Regression Random Forest with log. fine-tuning time as the dependent variable
		    \item Exploration of the relation between fine-tuning time and accuracy
	    \end{itemize}
	\item \textit{Analysis inference time.ipynb}
	    \begin{itemize}
	        \item Descriptive analysis of the minimum inference time of finished runs for different hyperparameter combinations
	        \item ANOVAs of fine-tuning time, grouped by task, model, maximum sequence length and batch size, respectively
	        \item Log-linear regression with log. inference time as the dependent variable
	        \item Regression Random Forest with log. inference time as the dependent variable 
	        \item Exploration of the relation between inference time and accuracy
	    \end{itemize}
	\item \textit{plot.zip} contains two sub-folders
	    \begin{itemize}
	        \item \textit{fine-tuning time}: This sub-folder contains all plots that were generated during the analysis of the fine-tuning time (run script \textit{Analysis finetuning time.ipynb})
	        \item \textit{inference time}: This sub-folder contains all plots that were generated during the analysis of the inference time (run script \textit{Analysis inference time.ipynb})
	    \end{itemize}
\end{itemize}
