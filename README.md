# data_generation

OVERVIEW
- This project includes utilities and scripts for automatic dataset generation. It is used in the following papers:
    - Kann, K., Warstadt, A., Williams, A., & Bowman, S. R. (2018). Verb argument structure alternations in word and sentence embeddings. arXiv preprint arXiv:1811.10773.
    - Warstadt, A., Cao, Y., Grosu, I., Peng, W., Blix, H., Nie, Y., ... & Wang, S. F. (2019). Investigating BERT's Knowledge of Language: Five Analysis Methods with NPIs. arXiv preprint arXiv:1909.02597.


PROJECT STRUCTURE
- The project contains the following packages:
    - generation_projects: scripts for generating data, organized into subdirectories by research project.
    - mturk_qc: code for carrying out Amazon mechanical turk quality control.
    - outputs: generated data, organized into subdirectories by research project.
    - results: experiment results files
    - results_processing: scripts for analyzing results and producing figures
    - utils: shared code for generation projects. Includes utilities for proecessing the vocabulary, generating constituents, manipulating generated strings, etc.
- It also contains a vocabulary file and documentation of the vocabulary:
    - vocabulary.csv: the vocab file.
    - vocab_documentation.md: the vocab documentation


VOCABULARY
- The vocabulary file is vocabulary.csv.
- Each row in the .csv is a lexical item. Each column is feature encoding grammatical information about the lexical item. Detailed documentation of the columns can be found in vocab_documentation.md.
- The following notation is used to define selectional restrictions in the arg_1, arg_2, and arg_3 columns:
    <DISJUNCTION> := <CONJUNCTION> | <CONJUNCTION>;<DISJUNCTION>
    <CONJUNCTION> := <CONDITION> | <CONDITION>^<CONJUNCTION>
    <CONDITION> := <COLUMN>=<VALUE>
- In other words, the entire restriction is written in disjunctive normal form where ";" is used for disjunction and "^" is used for conjunction.
- Example 1: arg_1 of lexical item "breaking" is "animate=1". This means any noun appearing as the subject of "breaking" must have value "1" in the column "animate". 
- Example 2: arg_1 of lexical item "buys" is "institution=1^sg=1;animate=1^sg=1". This means any noun appearing as the subject of "breaking" must have value ("1" in column "institution" and value "1" in column "sg") OR ("1" in column "animate" and value "1" in column "sg"). 


UTILS
- The utils package contains the shared code for the various generation projects.
    - utils.conjugate includes functions which conjugate verbs and add selecting auxiliaries/modals
    - utils.constituent_building includes functions which "do syntax". The following are especially useful:
        - verb_args_from_verb: gather all arguments of a verb into a dictionary
        - V_to_VP_mutate: given a verb, modify the expression to contain the string corresponding to a full VP
        - N_to_DP_mutate: given a noun, gather all arguments and a determiner, and modify the expression to contain the string corresponding to a full DP
    - utils.data_generator defines general classes that are instantianted by a particular generation project. The classes contain metadata fields, the main loop for a generating a dataset (generate_paradigm), and functions for logging and exception handling
    - utils.data_type contains the data_type necessary for the numpy structured array data structure used in the vocabulary.
        - if the columns of the vocabulary file are ever modified, this file must be modified to match.
    - utils.string_utils contains functions for cleaning up generated strings (removing extra whitespace, capitalization, etc.)
    - utils.vocab_sets contains constants for accessing commonly used sets of vocab entries. Building these constants takes about a minute at the beginning of running a generation script, but this speeds up generation of large datasets.
    - utils.vocab_table contains functions for creating and accessing the vocabulary table
    - get_all gathers all vocab items with a given restriction
    - get_all_conjunctive gathers all vocab items with the given restrictions
