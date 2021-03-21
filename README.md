# Python Code Generator using Transformer 
This is final capstone project for END-1 NLP course from SchoolofAI. It attempts to generate simple python code from an english text sentence as input using Seq-to-Seq transformer architecture. Following sections describes the various steps involved in achieving this objective.

## Data Cleaning
Even though the input file was compiled by student submissions, it still had some syntax errors like missing quotes, brackets, wrong indentations etc. These have to be manually corrected to keep these example records with proper python code. Also multi-line comments using triple quoted strings were removed to simplify the task as there were only handful of them in the entire dataset.

## Training datafile creation
Input file format of english sentence as comment line with # charater in first column followed by pythone code has to be converted into two column csv file. First column with English text as source column and corresponding python code string as second column(target). To remove outliers, the maximum length of strings were limited to 300 chars without loosing too many training records.

## Tagging
For english text, Spacy tokenizer was used after converting them all to lower case.
For python code, python tokenizer was used to parse the code into tokens and then custom tags were used as substitutes for INDENT, DEDENT, NEWLINE. All comments were ignored as they don't add to the code generation task.

Initially SentencePiece tokenizer was tried and eventhough the network was learning, there were lot of noice in the input training records as the tagging was not customized to python code. Hence the effort was shifted to python tokenizer as natively it had the ability to parse the python code structure into tokens.

## Model
Self-attention, Multi-head and scaled dot product  transformer model was used for the task. It was used in conversational chatbot example. To reduce overfit, the dropout factor was increased compared to original architecture.

Tried changing the layer depth unsuccessfully. It could be due to limited amount of training data we used. Increasing the hidden layer dimension saw a slight improvement in the model performance. 

### Loss Function
Label smoothing loss function from OpenNMT library was tried and it didn't help. So, the default cross entropy loss function was used.

## Training
After several runs, it was determined that around 30 epochs were enough for training the model. As it tended to overfit, the dropout factor was increased to minimize this risk. More experimentation is needed to improve performance.

## Inference
For a limited set of input english sentences, the model is capable of generating the python code. As the text size increases you see more erros with the output. One good thing is that for the most part, the generated code seems to adhere to python syntex eventhough it is not perfect! 

## Next Steps
Some of the next steps that could help this effort.

- Better tagging to handle numeric and string literals 
- Increase training data by data augmentation, code from other repos etc.
- Python code specific embeddings 