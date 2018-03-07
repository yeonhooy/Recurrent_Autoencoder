# Recurrent Autoencoder v1.0.1

## Overview
Recurrent autoencoder for time-series analysis.

## Description
This program implements a recurrent autoencoder for time-series analysis. The input to the program is a .csv file with feature columns. The time-series input is encoded with a single LSTM layer and decoded with a second LSTM layer to recreate the input. The output of the encoder layer feeds in to a single latent layer, which contains a compressed representation of the feature vector.

![Tensorboard Graph](https://raw.githubusercontent.com/jonzia/Autoencoder/master/Graph_101.PNG)

## To Run
1. The program is designed to accept .csv files with the following format.

Timestamps | Features | Labels
--- | --- | ---
t = 1 | Feature 1 ... Feature N | Label 1 ... Label M
... | ... | ...
t = T | Feature 1 ... Feature N | Label 1 ... Label M

2. Set LSTM parameters/architecture and learning rate decay in *network.py*.
3. Set training parameters in *autoencoder.py*:
```python
# Training
NUM_TRAINING = 10		# Number of training batches (balanced minibatches)
NUM_VALIDATION = 10		# Number of validation batches (balanced minibatches)
# Load File
LOAD_FILE = False 		# Load initial LSTM model from saved checkpoint?
```
4. Set file paths in *autoencoder.py*:
```python
# Specify filenames
# Root directory:
dir_name = "ROOT_DIRECTORY"
with tf.name_scope("Training_Data"):	# Training dataset
	tDataset = os.path.join(dir_name, "trainingdata.csv")
with tf.name_scope("Validation_Data"):	# Validation dataset
	vDataset = os.path.join(dir_name, "validationdata.csv")
with tf.name_scope("Model_Data"):		# Model save/load paths
	load_path = os.path.join(dir_name, "checkpoints/model")		# Load previous model
	save_path = os.path.join(dir_name, "checkpoints/model")		# Save model at each step
	save_path_op = os.path.join(dir_name, "checkpoints/model_op")	# Save optimal model
with tf.name_scope("Filewriter_Data"):	# Filewriter save path
	filewriter_path = os.path.join(dir_name, "output")
with tf.name_scope("Output_Data"):		# Output data filenames (.txt)
	# These .txt files will contain loss data for Matlab analysis
	training_loss = os.path.join(dir_name, "training_loss.txt")
	validation_loss = os.path.join(dir_name, "validation_loss.txt")
```
5. Run *autoencoder.py*. **(1)** Outputs of the program include training and validation loss .txt files as well as the Tensorboard graph and summaries. The model is saved at each timestep, and the optimal model as per the validation loss is saved separately.
6. *(Optional)* Run *test_bench.py* to obtain prediction and target values on a test dataset for the trained model. Ensure that proper filepaths are set before running.

## Notes
(1) Ensure that you have [Tensorflow](https://www.tensorflow.org/) and [Pandas](https://pandas.pydata.org) installed. This program was built on Python 3.6 and Tensorflow 1.5.
