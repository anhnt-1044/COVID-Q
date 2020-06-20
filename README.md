# COVID-Q: 1,690 Questions about COVID-19

Full dataset for the paper "[What are People Asking About COVID-19? A Question Classification Dataset](https://openreview.net/forum?id=qd51R0JNLl)"

The dataset CSV file can be found at [final_master_dataset.csv](https://github.com/JerryWei03/COVID-Q/blob/master/final_master_dataset.csv).

This dataset consists of COVID-19 questions which have been annotated into a broad category (e.g. Transmission, Prevention) and a more specific class such that questions in the same class are all asking the same thing.

Note: Formal definitions of our categories can be found at [this link](https://docs.google.com/document/d/1SgFB3qFo5Wb7PaoziaUtowfT6pQuVDcMrFfOydyV24s/edit?usp=sharing).

## Folders included in this respository:
1. `code` - all code needed to split dataset into training/testing datasets and to run basic BERT baselines.
2. `data` - contains original data (TSVs, CSVs, PDFs) to document all question sources
3. `dataset_categories` - contains usable training and testing data for question-category classification.
4. `dataset_classes` - contains usable training and testing data for question-class classification.


## Datasets
### Question-Category Classification
The *question-category classification* task assigns each question to one of 15 broad categories (e.g. Transmission, Prevention). The goal is to be able to match a given question to the category that best describes what type of information the question is asking for.

In the `dataset_categories` folder, the following files are included:
  1. `question_embeddings_pooled.pickle` - dictionary of BERT embeddings for every question in the dataset. Please note that augmented questions' embeddings are not included in this pickle and require recreating the pickle file.
  2. `testA.csv` - contains testing questions from real sources. Column 1 is the question and Column 2 is the category.
  3. `testB.csv` - contains testing questions that were generated by the author. Column 1 is the question and Column 2 is the category.
  4. `train20_augmented.csv` - contains training questions with 20 questions given per category for a total of 300 real questions. Data augmentation is used to generate 16 more examples for each question. Column 1 is the question and Column 2 is the category.
  5. `train_20.csv` - contains training questions with 20 questions given per category for a total of 300 questions. Column 1 is the question and Column 2 is the category.
  
 ### Question-Class Classification
The *question-class classification* task requires a new test question to be grouped into a question-class that asks the same thing, similar to retrieval QA contexts. The dataset is currently split such that for all question classes with at least 4 questions, 3 questions are randomly selected for the training set and the rest of the questions in the class are moved to the testing set.

In the `dataset_classes` folder, the following files are included:
  1. `question_embeddings_pooled.pickle` - dictionary of BERT embeddings for every question in the dataset. Please note that augmented questions' embeddings are not included in this pickle and require recreating the pickle file.
  2. `testA.csv` - contains testing questions from real sources. Column 1 is the question and Column 2 is the class.
  3. `testB.csv` - contains testing questions that were generated by the author. Column 1 is the question and Column 2 is the class.
  4. `train3_augmented.csv` - contains training questions with 3 questions given per class for a total of 267 real questions. Data augmentation is used to generate 16 more examples for each question. Column 1 is the question and Column 2 is the class.
  5. `train_3.csv` - contains training questions with 3 questions given per class for a total of 267 questions. Column 1 is the question and Column 2 is the class.

### Creating a Custom Dataset Split
To split your own dataset, choose whether to split the dataset by category (question-category classification) or by class (question-class classification).

If splitting by category:

Using `code/split_category_dataset.py`, change `20` on line 53 to the number of questions per category that you want in your training dataset.

If splitting by class:

Using `code/split_class_dataset.py`, change `3` on lines 38 and 41 to the number of questions per class that you want in your training dataset. Additionally, to change the threshold for filtering questions (which is currently set to 4 because we only use question classes with at least four questions), change `4` on line 35 to your desired number.

### Data Augmentation
Code has been provided to generate augmented data.

Using `code/eda.py`, set change the `read_csv` method on line 225 to the path to the file contains data to augment. This file should be formatted such that the first column is the question and the second column is the label (either category label or class label). Change `num_aug` on line 231 to the number of augmented questions for each inputted question that you desire.

When an augmented dataset is created, the BERT embeddings must be extracted for each of these augmented questions. To do this, run `code/get_bert_embeddings.py` and use the `combine_with_augmented_dataset` method, changing parameters as necessary.

## Baseline BERT Evaluation
We have provided three baseline evaluation methods: k-Nearest Neighbor for both category and class classification and SVM for category classification.

1. Running k-NN for category classification:
  - Open `code/test_category_knn.py` and change the arguments to the paths that you are using.
  - Top-N accuracy can be set by changing the `k` argument on line 70.
  - Distance measurement can be changed between Euclidean Distance and Cosine Similarity on line 70.
  
2. Running SVM for category classification:
  - Open `code/test_category_svm.py` and change the arguments to the paths that you are using.
  
3. Running k-NN for class classification:
  - Open `code/test_class_knn.py` and change the arguments to the paths that you are using.
  - Top-N accuracy can be set by changing the `k` argument on line 121/131.
  - Distance measurement can be changed between Euclidean Distance and Cosine Similarity on line 91.
  

# Citation
COVID-Q is an open-source dataset and is licensed under the GNU General Public License (v3). If you are using this dataset, please cite:

