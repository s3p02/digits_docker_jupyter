# digits_docker_jupyter

# Step 1 : Interact with the DIGITS Docker Container using Jupyter


```
jupyter notebook --no-browser --port=8889
```
Save/copy the token presented.

<kbd>
  <img src="/d_4_docker_jupyter_token.png">
</kbd>


# Step 2 : Port Forward on Local System

```
ssh -i ~/.ssh/gcp_new -N -f -L localhost:8888:localhost:8889 __user__name__@__external__ip__address
```

Say 'yes' for ECDSA key fingerprint.

# Step 3 : Launch Jupyter Notebook on Local System

Open a web-browser and go to [localhost:8888](localhost:8888)


## Enter the token, and click 'Login'

<kbd>
  <img src="/d_5_docker_jupyter_token_login.png">
</kbd>

## You will enter the jupyter notebook dashboard.

<kbd>
  <img src="/d_6_jupyter_dashboard.png">
</kbd>

## Click the New dropdown menu and seleck Python 2 Notebook.

<kbd>
  <img src="/d_7_jupyter_new_notebook.png">
</kbd>

## Enter the following in the new notebook.

```
!curl localhost:5000/index.json
```

## The following response indicates that digits is running on 'localhost:5000'

<kbd>
  <img src="/d_8_jupyter_new_notebook.png">
</kbd>


# Step 4 : Login to DIGITS

In order to create a dataset, you will first need to log in. The following command will log us in as user, you may enter your name.

```
!curl localhost:5000/login -c digits.cookie -XPOST -F username=user
```

<kbd>
  <img src="/d_9_jupyter_notebook_digits_login.png">
</kbd>

# Step 5 : Creating a Dataset

For our dataset, we shall use google's creative-commons licensed flower photos.

```
!curl http://download.tensorflow.org/example_images/flower_photos.tgz \
    | tar xz -C ~/
```

<kbd>
  <img src="/d_10_jupyter_notebook_digits_download_flowers.png">
</kbd>

## Check for dataset

```
!ls ~/flower_photos
```

## Our dataset consists of 5 classes, with each class having >500 images.

<kbd>
  <img src="/d_11_jupyter_notebook_digits_flower_classes.png">
</kbd>

## Set Path to create Dataset

```
FLOWER_PHOTO_PATH='/home/srpa3180/flower_photos/'
```

```
!curl localhost:5000/datasets/images/classification.json -b digits.cookie -XPOST -F folder_train=$FLOWER_PHOTO_PATH -F encoding=jpg -F resize_channels=1 -F resize_width=240 -F resize_height=240 -F method=folder -F dataset_name=google_flowers_dataset
```
## The Following indicates that the dataset in initialized

<kbd>
  <img src="/d_12_jupyter_notebook_digits_initialize_dataset.png">
</kbd>

## Its Important we note down the dataset id as shown from the above response

```
!curl localhost:5000/datasets/20171024-072846-e9f2/status
```
## The Following indicates the status of the dataset, "done".

<kbd>
  <img src="/d_13_jupyter_notebook_digits_dataset_status.png">
</kbd>

```
!curl localhost:5000/datasets/20171024-072846-e9f2.json
```
## To get a detailed description of the dataset, we can call the json using the dataset ID.

<kbd>
  <img src="/d_14_jupyter_notebook_digits_dataset_detailed_status.png">
</kbd>

## Its Important we note down the dataset id as shown from the above response

#####

```
!curl localhost:5000/models/images/classification.json -b digits.cookie -XPOST -F model_name=google_flowers_model -F dataset=20171024-072846-e9f2 -F method=standard -F standard_networks=googlenet -F train_epochs=30 -F framework=caffe 
```
## The Following indicates that the dataset in initialized

<kbd>
  <img src="/d_15_jupyter_notebook_digits_initialize_model.png">
</kbd>

## Its Important we note down the JOB ID of our model as shown from the above response

```
!curl localhost:5000/models/20171024-074102-fe73/status
```
## The Following indicates the status of the model, "running".

<kbd>
  <img src="/d_16_jupyter_notebook_digits_model_status.png">
</kbd>

```
!curl localhost:5000/models/20171024-074102-fe73.json
```
## To get a detailed description of the model, we can call the json using the JOB ID.

<kbd>
  <img src="/d_17_jupyter_notebook_digits_model_detailed_status.png">
</kbd>

## Its Important we note down the JOB ID as shown from the above response

##

```
!nvidia-smi
```
## To get check on GPU utilization and Memory, we can call the above command, which shows the below description.

<kbd>
  <img src="/d_18_jupyter_notebook_digits_gpu_util.png">
</kbd>


```
!curl localhost:5000/models/20171024-074102-fe73.json
```

## To check on number of epoch's completed we call the description of the model through the json, which shows "snapshots" as 1, indicationg the completion of 1 epoch as shown below.

<kbd>
  <img src="/d_19_jupyter_notebook_digits_model_detailed_epoch_status.png">
</kbd>

##

```
!wget https://upload.wikimedia.org/wikipedia/commons/thumb/7/7b/Yonina_Tulip.jpg/220px-Yonina_Tulip.jpg -P ~/
```

## To check for accuracy of our running model, we first download a smaple test image from wikipidia, as shown below.

<kbd>
  <img src="/d_20_jupyter_notebook_digits_download_sample.png">
</kbd>

##

```
!curl localhost:5000/models/images/classification/classify_one.json -XPOST -F job_id=20171024-074102-fe73 -F image_file=@/home/srpa3180/220px-Yonina_Tulip.jpg
```

## To test for accuracy of our running model, we run the above, which returns a json as shown below.

<kbd>
  <img src="/d_21_jupyter_notebook_digits_test_model.png">
</kbd>

##

```
!curl localhost:5000/models/images/classification/classify_one.json -XPOST -F job_id=20171024-074102-fe73 -F image_file=@/home/srpa3180/220px-Yonina_Tulip.jpg
```

## To test for accuracy of our running model, we run the above, which returns a json as shown below.

<kbd>
  <img src="/d_21_jupyter_notebook_digits_test_model.png">
</kbd>

## We notice that the prediction is poor, however the prediction get better with future epochs.

```
!curl localhost:5000/models/20171024-074102-fe73.json
```

## As the Epochs increase, the predictions may get better as shown below.

<kbd>
  <img src="/d_22_jupyter_notebook_future_epochs.png">
</kbd>

###

```
!curl localhost:5000/models/images/classification/classify_one.json -XPOST -F job_id=20171024-074102-fe73 -F image_file=@/home/srpa3180/220px-Yonina_Tulip.jpg
```

## To test for accuracy of our running model, we run the above, which returns a json as shown below.

<kbd>
  <img src="/d_21_jupyter_notebook_digits_test_model_better_prediction.png">
</kbd>


##
```
!curl localhost:5000/models/20171024-074102-fe73.json
```

## As the Epoch reaches 30, the json, which shows "snapshots" as 1, indicates the completion of 30 epoch as shown below.

<kbd>
  <img src="/d_21_jupyter_notebook_digits_complete.png">
</kbd>


