# dcos-jupyter

### Deploy services
Deploy HDFS
```
http://api.hdfs.marathon.l4lb.thisdcos.directory/v1/endpoints
```
Deploy Jupyter Notebook
Set the storage to /tmp and turn on tensorboard

### Accessing the Jupyter Notebook
```
/jupyterlab-notebook
```

### Accessing the Tensorflow Board
```
/jupyterlab-notebook/tensorflowboard
```

### Clone Project One
```
git clone https://github.com/mamcgrath/TensorBoard-TF-Dev-Summit-Tutorial.git
cd TensorBoard-TF-Dev-Summit-Tutorial/
python mnist.py
```

### Clone Project Two
```
git clone https://github.com/yahoo/TensorFlowOnSpark.git
curl -fsSL -O https://s3.amazonaws.com/vishnu-mohan/tensorflow/mnist/mnist.zip
unzip mnist.zip
eval spark-submit ${SPARK_OPTS} --verbose $(pwd)/TensorFlowOnSpark/examples/mnist/mnist_data_setup.py --output mnist/csv --format csv
hdfs dfs -ls mnist/
eval spark-submit ${SPARK_OPTS} --verbose \
  --conf spark.mesos.executor.docker.image=dcoslabs/dcos-jupyterlab:1.2.0-0.33.7 \
  --py-files $(pwd)/TensorFlowOnSpark/examples/mnist/spark/mnist_dist.py \
  $(pwd)/TensorFlowOnSpark/examples/mnist/spark/mnist_spark.py \
  --cluster_size 5 --image mnist/csv/train/images \
  --labels mnist/csv/train/labels \
  --format csv --mode train --model mnist/mnist_csv_model
```
