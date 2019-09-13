# GSP076 -- Tutorial: Cloud ML Engine: Qwik Start #
## Cloud ML Engine
This lab will give you hands-on practice with TensorFlow model training, both locally and on Cloud ML Engine. After training, you will learn how to deploy your model to Cloud ML Engine for serving (prediction). You'll train your model to predict income category of a person using the United States Census Income Dataset.

This lab gives you an introductory, end-to-end experience of training and prediction on Cloud Machine Learning Engine. The lab will use a census dataset to:

* Create a TensorFlow training application and validate it locally.
* Run your training job on a single worker instance in the cloud.
* Run your training job as a distributed training job in the cloud.
* Optimize your hyperparameters by using hyperparameter tuning.
* Deploy a model to support prediction.
* Request an online prediction and see the response.
* Request a batch prediction.

### What you will build ###
The sample builds a wide and deep model for predicting income category based on United States Census Income Dataset. The two income categories (also known as labels) are:

>50K — Greater than 50,000 dollars
<=50K — Less than or equal to 50,000 dollars
Wide and deep models use deep neural nets (DNNs) to learn high-level abstractions about complex features or interactions between such features. These models then combine the outputs from the DNN with a linear regression performed on simpler features. This provides a balance between power and speed that is effective on many structured data problems.

The sample defines the model using TensorFlow's prebuilt DNNCombinedLinearClassifier class. The sample defines the data transformations particular to the census dataset, then assigns these (potentially) transformed features to either the DNN or the linear portion of the model.

## Prerequisites ##

 -  You have a Google Cloud Platform account and a Google Project (note the Google Project Id) provided by Gitlab. You can find this on the left side of the QwikLab page.

## Install Prerequisites ##

1. Verify current account from QwikLab
```bash
gcloud auth list
```

2. Check your current project.
```bash
gcloud config list project
```

## Install TensorFlow ##

3. Run the following command to install TensorFlow:
```bash
pip install --user --upgrade tensorflow
```

4. Verify the installation:
```bash
python -c "import tensorflow as tf; print('TensorFlow version {} is installed.'.format(tf.VERSION))"
```

You can ignore any warnings that the TensorFlow library wasn't compiled to use certain instructions.


5. Clone the example repo
```bash
git clone https://github.com/GoogleCloudPlatform/cloudml-samples.git
```

6. Navigate to the cloudml-samples > census > estimator directory. The commands in this lab must be run from the estimator directory:
```bash
cd cloudml-samples
cd census
cd estimator
```
You are in the /cloudml-samples/census/estimator subdirectory.  If your prompt is too long you can shorten it to the user and last folder with:
You may notbe ableto insert this directly into the shell. If you get an invalid character error.  Use the copy icon and Ctrl+v to paste the commend into the console.  
```bash
PS1='\u:\W\$ '
```

7. Check your current directory 
```bash
pwd
```

## Develop and validate your training application locally ##
Before you run your training application in the cloud, get it running locally. Local environments provide an efficient development and validation workflow so that you can iterate quickly. You also won't incur charges for cloud resources when debugging your application locally.

Get your training data
The relevant data files, adult.data and adult.test, are hosted in a public Google Cloud Storage bucket.

You can read the files directly from Cloud Storage or copy them to your local environment. For this lab you will download the samples for local training, and later upload them to your own Cloud Storage bucket for cloud training.

Run the following command to download the data to a local file directory and set variables that point to the downloaded data files:1. Check your current directory 
```bash
mkdir data
gsutil -m cp gs://cloud-samples-data/ml-engine/census/data/* data/
```


8.  Now set the TRAIN_DATA and EVAL_DATA variables to your local file paths by running the following commands:
```bash
export TRAIN_DATA=$(pwd)/data/adult.data.csv
export EVAL_DATA=$(pwd)/data/adult.test.csv
```

9.  To open the adult.data.csv file, run the following command:
```bash
head data/adult.data.csv
```

10.  You will see that the data is stored in comma-separated value format that resembles the following:
```console
39, State-gov, 77516, Bachelors, 13, Never-married, Adm-clerical, Not-in-family, White, Male, 2174, 0, 40, United-States, <=50K
50, Self-emp-not-inc, 83311, Bachelors, 13, Married-civ-spouse, Exec-managerial, Husband, White, Male, 0, 0, 13, United-States, <=50K
38, Private, 215646, HS-grad, 9, Divorced, Handlers-cleaners, Not-in-family, White, Male, 0, 0, 40, United-States, <=50K
53, Private, 234721, 11th, 7, Married-civ-spouse, Handlers-cleaners, Husband, Black, Male, 0, 0, 40, United-States, <=50K |
```
Now that you have downloaded and inspected your training data, you will install the necessary dependencies.

11. Install dependencies
Although TensorFlow is installed on Cloud Shell, you must run the sample's requirements.txt file to ensure you are using the same version of TensorFlow required by the sample:
```bash
pip install --user -r ../requirements.txt
```
It will take a couple minutes for this command to complete. You will receive a similar output when it does:
Output | |
-------|-|
Successfully installed Keras-2.2.4 future-0.16.0 numexpr-2.6.9 numpy-1.14.5 pandas-0.24.1 python-dateutil-2.8.0 pyyaml-3.13 scipy-1.2.1 setuptools-39.1.0 tensorboard-1.10.0 tensorflow-1.10.0 | |


12. Run a local training job
A local training job loads your Python training program and starts a training process in an environment that's similar to that of a live Cloud ML Engine cloud training job.

Specify an output directory and set a MODEL_DIR variable by running the following command: 
```bash
export MODEL_DIR=output
```

13. Run this training job locally by running the following command:
```bash
gcloud ai-platform local train \
    --module-name trainer.task \
    --package-path trainer/ \
    --job-dir $MODEL_DIR \
    -- \
    --train-files $TRAIN_DATA \
    --eval-files $EVAL_DATA \
    --train-steps 1000 \
    --eval-steps 100
```

Note:
When you run the same training job on CMLE later in the lab, you'll see that the command is not much different from the above.

By default, verbose logging is turned off. You can enable it by setting the --verbosity tag to DEBUG. A later example shows you how to enable it.

Inspect the summary logs using Tensorboard
To see the evaluation results, you can use the visualization tool called TensorBoard. With TensorBoard, you can visualize your TensorFlow graph, plot quantitative metrics about the execution of your graph, and show additional data like images that pass through the graph. Tensorboard is available as part of the TensorFlow installation.

Follow the steps below to launch TensorBoard and point it at the summary logs produced during training, both during and after execution.

## Launch TensorBoard:
```bash
tensorboard --logdir=$MODEL_DIR --port=8080
```
Click on the Web Preview icon `walkthrough web-preview-icon`, then Preview on port 8080. A new tab will open with TensorBoard running.
<walkthrough-spotlight-pointer spotlightId="devshell-web-preview-button"
                               text="Open Web Preview">
</walkthrough-spotlight-pointer>
![](https://cdn.qwiklabs.com/a6YnJv8GlGae4rnJIbjA27J8c7YApa%2B6noPFkkKxZjk%3D)

Click on Accuracy to see graphical representations of how accuracy changes as your job progresses.

Type CTRL+C in Cloud Shell to shut down TensorBoard.

## Running model prediction locally (in Cloud Shell) ##
A local training job loads your Python training program and starts a training process in an environment that's similar to that of a live Cloud ML Engine cloud training job.

Specify an output directory and set a MODEL_DIR variable by running the following command:
```bash
export MODEL_DIR=output
```

The output/export/census directory holds the model exported as a result of running training locally. List that directory to see the generated timestamp subdirectory:
```bash
ls output/export/census/
```
You should receive a similar output:
```console
1547669983
```

Copy the timestamp that was generated.

Run the following command in Cloud Shell, replacing <timestamp> with your timestamp but do not press enter until the next step.:
```bash
gcloud ai-platform local predict --model-dir output/export/census/<timestamp> 
```
Append the next modifyer into the console and enter.
```bash
--json-instances ../test.json
```
You should see a result that looks something like the following:
```console
CLASS_IDS  CLASSES  LOGISTIC              LOGITS                PROBABILITIES
[0]        [u'0']   [0.0583675391972065]  [-2.780855178833008]  [0.9416324496269226, 0.0583675354719162]
```
Where class 0 means income <= 50k and class 1 means income >50k.

## Use your trained model for prediction ##
Once you've trained your TensorFlow model, you can use it for prediction on new data. In this case, you've trained a census model to predict income category given some information about a person.

For this section we'll use a predefined prediction request file, test.json, included in the GitHub repository in the census directory. You can inspect the JSON file in the Cloud Shell editor if you're interested in learning more.



Now that you've validated your model by running it locally, you will now get practice training is using Cloud ML Engine. You'll remain in the Cloud Shell for this section.

Note: The initial job request will take several minutes to start, but subsequent jobs run more quickly. This enables quick iteration as you develop and validate your training job.

Set up a Google Cloud Storage bucket
The Cloud ML Engine services need to access Google Cloud Storage (GCS) to read and write data during model training and batch prediction.

First, set the following variables:
```bash
PROJECT_ID=$(gcloud config list project --format "value(core.project)")
BUCKET_NAME=${PROJECT_ID}-mlengine
echo $BUCKET_NAME
REGION=us-central1
```
Create the new bucket:
```bash
gsutil mb -l $REGION gs://$BUCKET_NAME
```
Test Completed Task
Click Check my progress to verify your performed task.
Go back to the qwiklabs page and look for the **Check My Progress**

## Upload the data files to your Cloud Storage bucket.

Use gsutil to copy the two files to your Cloud Storage bucket:
```bash
gsutil cp -r data gs://$BUCKET_NAME/data
```
Set the TRAIN_DATA and EVAL_DATA variables to point to the files:
```bash
TRAIN_DATA=gs://$BUCKET_NAME/data/adult.data.csv
EVAL_DATA=gs://$BUCKET_NAME/data/adult.test.csv
```
Use gsutil again to copy the JSON test file test.json to your Cloud Storage bucket:
```bash
gsutil cp ../test.json gs://$BUCKET_NAME/data/test.json
```
Set the TEST_JSON variable to point to that file:
```bash
TEST_JSON=gs://$BUCKET_NAME/data/test.json
```
Test Completed Task
Click Check my progress to verify your performed task.
Go back to the qwiklabs page and look for the **Check My Progress**

## Run a single-instance trainer in the cloud
With a validated training job that runs in both single-instance and distributed mode, you're now ready to run a training job in the cloud. You'll start by requesting a single-instance training job.

Use the default BASIC scale tier to run a single-instance training job. The initial job request can take a few minutes to start, but subsequent jobs run more quickly. This enables quick iteration as you develop and validate your training job.

Select a name for the initial training run that distinguishes it from any subsequent training runs. For example, you can append a number to represent the iteration:
```bash
JOB_NAME=census_single_1
```
Specify a directory for output generated by Cloud ML Engine by setting an OUTPUT_PATH variable to include when requesting training and prediction jobs. The OUTPUT_PATH represents the fully qualified Cloud Storage location for model checkpoints, summaries, and exports. You can use the BUCKET_NAME variable you defined in a previous step. It's a good practice to use the job name as the output directory:
```bash
OUTPUT_PATH=gs://$BUCKET_NAME/$JOB_NAME
```
Run the following command to submit a training job in the cloud that uses a single process. This time, set the --verbosity tag to DEBUG so that you can inspect the full logging output and retrieve accuracy, loss, and other metrics. The output also contains a number of other warning messages that you can ignore for the purposes of this sample:
```bash
gcloud ai-platform jobs submit training $JOB_NAME \
    --job-dir $OUTPUT_PATH \
    --runtime-version 1.10 \
    --module-name trainer.task \
    --package-path trainer/ \
    --region $REGION \
    -- \
    --train-files $TRAIN_DATA \
    --eval-files $EVAL_DATA \
    --train-steps 1000 \
    --eval-steps 100 \
    --verbosity DEBUG
```
You can monitor the progress of your training job by watching the logs on the command line:
```bash
gcloud ai-platform jobs stream-logs $JOB_NAME
```
Or monitor in the Console: AI Platform > Jobs.

Test Completed Task
Click Check my progress to verify your performed task.

## Inspect the output
If you have been monitoring the job in Cloud Shell, you will know you're done when your output resembles the following:
```console
INFO    2019-01-16 12:58:34 -0800     master-replica-0      Task completed successfully.
```
In cloud training, outputs are produced in Cloud Storage. In this sample, outputs are saved to OUTPUT_PATH; to list them, run:
```bash
gsutil ls -r $OUTPUT_PATH
```
The outputs should be similar to the outputs from training locally (above).

Optional: You can point TensorBoard to this directory, either during or after training using the Web Preview menu at the top of the Cloud Shell and again select Preview on port 8080:
```bash
tensorboard --logdir=$OUTPUT_PATH --port=8080
```
You can shut down TensorBoard by typing ctrl + c on the command line.

## Deploy a model to support prediction.

Deploy your model to support prediction
By deploying your trained model to Cloud ML Engine to serve online prediction requests, you get the benefit of scalable serving. This is useful if you expect your trained model to be hit with many prediction requests in a short period of time.

Wait until your CMLE training job is done. It is finished when you see a green check mark by the jobname in the Cloud Console, or when you see the message Job completed successfully in the Cloud Shell command line.

Create a Cloud ML Engine model:
```bash
MODEL_NAME=census
```
Create a Cloud ML Engine model:
```bash
gcloud ai-platform models create $MODEL_NAME --regions=$REGION
```
Test Completed Task
Click Check my progress to verify your performed task.

Create a Cloud ML Engine model.
Select the exported model to use, by looking up the full path of your exported trained model binaries.
```bash
gsutil ls -r $OUTPUT_PATH/export
```
Scroll through the output to find the value of $OUTPUT_PATH/export/census/<timestamp>/. Copy timestamp and add it to the following command to set the environment variable MODEL_BINARIES to its value:
```bash
MODEL_BINARIES=$OUTPUT_PATH/export/census/<timestamp>/
```
Note: The timestamp for this training run will not be the same as the timestamp generated during your local training run above. Be sure to scroll through the gsutil ls output to find this new timestamp.

You'll deploy this trained model.

Run the following command to create a version v1 of your model:
```bash
gcloud ai-platform versions create v1 \
--model $MODEL_NAME \
--origin $MODEL_BINARIES \
--runtime-version 1.10
```
Test Completed Task
Click Check my progress to verify your performed task.

Create a version v1 of your model.
It may take several minutes to deploy your trained model. When done, you can see a list of your models using the models list command:
```bash
gcloud ai-platform models list
```
Send an online prediction request to your deployed model
You can now send prediction requests to your deployed model. The following command sends a prediction request using the test.json file included in the GitHub repository.
```bash
gcloud ai-platform predict \
--model $MODEL_NAME \
--version v1 \
--json-instances ../test.json
```
The response includes the probabilities of each label (>50K and <=50K) based on the data entry in test.json, thus indicating whether the predicted income is greater than or less than 50,000 dollars
```console
CLASS_IDS  CLASSES  LOGISTIC               LOGITS                PROBABILITIES
[0]        [u'0']   [0.06142739951610565]  [-2.726504325866699]  [0.9385725855827332, 0.061427392065525055]
```
Where class 0 means income <= 50k and class 1 means income >50k.

Note: CMLE supports batch prediction too, but it's not included in this lab. See the documentation for more info.

Congratulations!
In this lab, you've learned how to train a TensorFlow model both locally and on Cloud ML Engine, and then how to use your trained model for prediction.

Go back to the Qwicklabs page to verify the lab shows as complete.

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>