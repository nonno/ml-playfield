# Machine-learning with Python cheatsheet

## Pandas

### Inspections

```py
foo_df.info() # list of fields with types, and memory usage

foo_df[['field_1', 'field_2']].describe() # statistics about specified fields in the dataset

foo_df[['field_1', 'field_2', 'field_3']].isna().any() # returns the list of the field, saying if there are null values

aggregated_df = foo_df['field_1'].value_counts() # list of values in the fields, with number of occurrencies
```

### Manipulations

```py
foo_df["field_1"] = True # sets the same value for all the rows

foo_df = pd.concat([foo_1_df, foo_2_df]) # concatenation of dataframes

foo_df = foo_df.dropna(subset=['field_1']) # drops rows having null values on the selected fields

foo_df = foo_df.fillna({'field_1': ''}) # fills null values on the selected fields

foo_df = foo_df.drop_duplicates(['field_1', 'field_2']) # distinct on the specified fields

foo_df[:50].index.tolist() # first 50 values

foo_df['field_1_encoded'] = foo_df.apply(lambda row: row['field_1'] + "_suffix", axis=1) # apply lambda to process a field

# balance training set - get the same number of labelled and non labelled
labelled_df = foo_df[foo_df['label'] == True]
non_labelled_df = foo_df[foo_df['label'] == False][:labelled_df.size]
balanced_df = pd.concat(labelled_df, non_labelled_df)
```

### SQL on dataframes

```py
agg_funcs = {}
agg_funcs['field_3'] = custom_fun # custom defined function
agg_funcs['field_4'] = 'max' # max function

# group-by with specified fields
aggregated_df = foo_df[['field_1', 'field_2', 'field_3', 'field_4']]
    .groupby(['field_1', 'field_2'])
    .agg(concatenate_strings)
    .reset_index()

# quick group-by to check how a dataset is balanced on a field
foo_df.groupby('field_1').count()

# merge/join on specified fields
merged_df = foo_1_df.merge(foo_2_df[['field_1', 'field_2', 'field_3']], how='left', on=['field_1', 'field_2'])

# sort by specified fields
sorted_df = foo_df.sort_values(['field_1', 'field_2'], ascending=[True, False])
```

### Misc

```py
csv_values_df = pd.read_csv('values.csv').query(f"field != 'invalid_value'") # load CSV
```

## Working with S3

```py
region = boto3.Session().region_name
assert region == 'us-east-1'

os.environ['AWS_REGION'] = region
smclient = boto3.Session().client('sagemaker')
s3_cli = boto3.client('s3')

foo_df = wr.s3.read_parquet(path=f"s3://bucket-name/file.parquet", map_types=False)

wr.s3.to_parquet(foo_df, f"s3://bucket-name/file.parquet")
```

## SKLearn

```py
# split rain dataset into actual 75% used for traiing and 25% used for validation
train_features, val_features, train_labels, val_labels = train_test_split(features, labels, train_size=0.75, random_state=42, stratify=labels)

# create Pools as some classifiers (Catboost) perform better with them
train_pool = Pool(train_features, train_labels, feature_names=feature_names)
validate_pool = Pool(val_features, val_labels, feature_names=feature_names)
test_pool = Pool(test_features, test_labels, feature_names=feature_names)
```

## CatBoostClassifier

```py
model = CatBoostClassifier(
    random_seed=42,
#   logging_level='Silent',
    logging_level='Verbose',
    iterations=100,
#   task_type='GPU',
    task_type='CPU'
)

# model.load_model('model_file_name')

# train model
time_zero = time.time()
model.fit(
    train_pool, 
    eval_set=val_pool,
    logging_level='Verbose', 
);
duration = time.time() - time_zero
print(f'time : {duration} s')

# check all model parameters
model.get_all_params()

# save model
model.save_model('model_file_name')

# check its size below
!ls -l model_file_name
```

### Model evaluation

```py
# get metrics about trained model using the test_pool (previously split)
metrics_names=['Logloss', 'Accuracy', 'Precision', 'Recall', 'F1']
metrics_values = model.eval_metrics(test_pool, metrics_names)
for n in metrics_names:
    print(f"{n}: {metrics_values[n][-1]}")

# get feature importance (keeping only top 10)
feature_importances = model.get_feature_importance(test_pool)
for score, name in sorted(zip(feature_importances, feature_names), reverse=True)[:10]:
    print('{}: {}'.format(name, score))
```

### Inference

```py
time_zero = time.time()
pred = model.predict(test_features)
proba = model.predict_proba(test_features)
duration = time.time() - time_zero
print(f'time : {duration} s')
```

### Cross-validation of train data

```py
# run 10-fold cross-validation
cv_dataset = Pool(data=features, label=labels)

params = {"iterations": 500, "loss_function": "Logloss", "task_type": "CPU"}

result = cv(cv_dataset, params,
    fold_count=10, return_models=True, as_pandas=True, iterations=500, verbose = False, save_snapshot=False
)
result[0]
```

## Misc

```py
del foo_df # delete variable, facilitating garbate collector

set(foo_list) # conversion to set => distinct values

foo_df['field_1'] = foo_df['field_1'].map({'True': True, 'False': False}) # remap values, from string to boolean
```

## Usual imports

```py
!conda install -y dask

!pip install s3fs==2021.08.0
!pip install dask-ml
!pip install awswrangler

import io
import os
import sys

import sagemaker
import boto3

import numpy as np
import pandas as pd
import dask.dataframe as dd
from dask_ml.preprocessing import Categorizer, DummyEncoder
from sklearn.pipeline import make_pipeline
import pickle
import tqdm

import awswrangler as wr
```