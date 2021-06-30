# Overview

The following training pipelines, and their options, are available in the shared `/srv` directory:

- `classify.py` with options:
  - `x_train`: training input (in NumPy array, Pandas DataFrame, etc. formats)
  - `y_train`: training output
  - `out_model_localpath`: path to store the trained model to
  - `out_model_s3objectkey`: path (aka object-key) on a predefined, shared S3 bucket to upload the trained model to (exclusive with `out_model_localpath` option)

A classification example:
```sh
%run /srv/classify.py \
    --x_train 'x_train.npy' \
    --y_train 'y_train.npy' \
    --out_model_s3objectkey 'my_model'
```

## Custom model storage location

You may upload the trained model to a different S3 bucket, providing your own AWS credentials to `boto3`, or a different storage altogether. To do so, store the trained model on your local path by providing the training pipeline the `--out_model_localpath` option:

```sh
out_model_localpath = !(echo "$HOME/my_model")
%run /srv/classify.py \
    ...
    --out_model_s3objectkey out_model_localpath[0]
```

Then upload the trained model to a different bucket.
```py
import boto3
...
(boto3.Session().resource('s3')
    .Bucket(s3bucket)
    .Object(s3key)
    .upload_file(model_path))
```