# DL4J 0.9.1 - VGG16 - Cifar 10

## Código executado Zeppelin

```scala
%spark.dep
z.reset()
z.load("org.nd4j:nd4j-native-platform:0.9.1")
z.load("org.deeplearning4j:deeplearning4j-core:0.9.1")
z.load("org.datavec:datavec-spark_2.11:0.9.1_spark_2")
z.load("org.deeplearning4j:dl4j-spark_2.11:0.9.1_spark_2")
z.load("org.deeplearning4j:deeplearning4j-zoo:0.9.1")
```

```scala
import scala.collection.JavaConversions._
import org.deeplearning4j.datasets.iterator._
import org.deeplearning4j.datasets.iterator.impl._
import org.deeplearning4j.eval.Evaluation
import org.deeplearning4j.zoo.model.VGG16
import org.apache.spark.SparkConf
import org.deeplearning4j.nn.graph.ComputationGraph
import org.apache.spark.api.java.JavaRDD
import org.apache.spark.api.java.JavaSparkContext
import org.nd4j.linalg.dataset.DataSet
```

```scala
import org.datavec.image.loader.CifarLoader
numberOfLabels: Int = 10
numberOfSamples: Int = 50000
numberOfTestSamples: Int = 10000
batchSizePerWorker: Int = 16
```

```scala
import org.datavec.image.loader.BaseImageLoader
import collection.JavaConverters._
import java.io.File
states: scala.collection.immutable.Map[String,String] = Map(filesFilename -> cifar-10-binary.tar.gz, filesURL -> https://storage.googleapis.com/ufrgsneuwald/~kriz/cifar-10-binary.tar.gz, filesFilenameUnzipped -> cifar-10-batches-bin)
```

testDataSet: org.deeplearning4j.datasets.iterator.impl.CifarDataSetIterator = org.deeplearning4j.datasets.iterator.impl.CifarDataSetIterator@3542253
import scala.collection.mutable.ListBuffer
testDataList: scala.collection.mutable.ListBuffer[org.nd4j.linalg.dataset.DataSet] = ListBuffer()
testData: org.apache.spark.rdd.RDD[org.nd4j.linalg.dataset.DataSet] = ParallelCollectionRDD[0] at parallelize at <console>:53
FINISHED   
Took 12 sec. Last updated by anonymous at September 03 2019, 1:24:36 AM.

seed: Int = 123456
zooModel2: org.deeplearning4j.zoo.model.VGG16 = org.deeplearning4j.zoo.model.VGG16@44e69ab0
import org.deeplearning4j.zoo.PretrainedType
m: org.deeplearning4j.nn.graph.ComputationGraph = org.deeplearning4j.nn.graph.ComputationGraph@72a0c73d
FINISHED   
Took 20 sec. Last updated by anonymous at September 03 2019, 1:24:50 AM.

import org.deeplearning4j.spark.impl.graph.SparkComputationGraph
sparkNetCG: org.deeplearning4j.spark.impl.graph.SparkComputationGraph = org.deeplearning4j.spark.impl.graph.SparkComputationGraph@43975341
resultado: Array[org.deeplearning4j.eval.Evaluation] =
Array(
Examples labeled as 0 classified by model as 0: 924 times
Examples labeled as 0 classified by model as 1: 13 times
Examples labeled as 0 classified by model as 2: 10 times
Examples labeled as 0 classified by model as 3: 20 times
Examples labeled as 0 classified by model as 5: 13 times
Examples labeled as 0 classified by model as 6: 1 times
Examples labeled as 0 classified by model as 7: 4 times
Examples labeled as 0 classified by model as 8: 11 times
Examples labeled as 0 classified by model as 9: 4 times
Examples labeled as 1 classified by model as 0: 81 times
Examples labeled as 1 classified by model as 1: 838 times
Examples labeled as 1 classified by model as 2: 2 times
Examples labeled as 1 classified by model as 3: 12 ti...
Examples labeled as 0 classified by model as 0: 924 times
Examples labeled as 0 classified by model as 1: 13 times
Examples labeled as 0 classified by model as 2: 10 times
Examples labeled as 0 classified by model as 3: 20 times
Examples labeled as 0 classified by model as 5: 13 times
Examples labeled as 0 classified by model as 6: 1 times
Examples labeled as 0 classified by model as 7: 4 times
Examples labeled as 0 classified by model as 8: 11 times
Examples labeled as 0 classified by model as 9: 4 times
Examples labeled as 1 classified by model as 0: 81 times
Examples labeled as 1 classified by model as 1: 838 times
Examples labeled as 1 classified by model as 2: 2 times
Examples labeled as 1 classified by model as 3: 12 times
Examples labeled as 1 classified by model as 5: 8 times
Examples labeled as 1 classified by model as 6: 2 times
Examples labeled as 1 classified by model as 7: 1 times
Examples labeled as 1 classified by model as 8: 22 times
Examples labeled as 1 classified by model as 9: 34 times
Examples labeled as 2 classified by model as 0: 379 times
Examples labeled as 2 classified by model as 1: 13 times
Examples labeled as 2 classified by model as 2: 437 times
Examples labeled as 2 classified by model as 3: 41 times
Examples labeled as 2 classified by model as 4: 3 times
Examples labeled as 2 classified by model as 5: 112 times
Examples labeled as 2 classified by model as 7: 13 times
Examples labeled as 2 classified by model as 8: 2 times
Examples labeled as 3 classified by model as 0: 318 times
Examples labeled as 3 classified by model as 1: 22 times
Examples labeled as 3 classified by model as 2: 26 times
Examples labeled as 3 classified by model as 3: 365 times
Examples labeled as 3 classified by model as 5: 206 times
Examples labeled as 3 classified by model as 6: 3 times
Examples labeled as 3 classified by model as 7: 40 times
Examples labeled as 3 classified by model as 8: 8 times
Examples labeled as 3 classified by model as 9: 12 times
Examples labeled as 4 classified by model as 0: 444 times
Examples labeled as 4 classified by model as 1: 28 times
Examples labeled as 4 classified by model as 2: 61 times
Examples labeled as 4 classified by model as 3: 135 times
Examples labeled as 4 classified by model as 4: 80 times
Examples labeled as 4 classified by model as 5: 148 times
Examples labeled as 4 classified by model as 6: 7 times
Examples labeled as 4 classified by model as 7: 94 times
Examples labeled as 4 classified by model as 8: 2 times
Examples labeled as 4 classified by model as 9: 1 times
Examples labeled as 5 classified by model as 0: 183 times
Examples labeled as 5 classified by model as 1: 14 times
Examples labeled as 5 classified by model as 2: 28 times
Examples labeled as 5 classified by model as 3: 47 times
Examples labeled as 5 classified by model as 4: 1 times
Examples labeled as 5 classified by model as 5: 694 times
Examples labeled as 5 classified by model as 6: 3 times
Examples labeled as 5 classified by model as 7: 26 times
Examples labeled as 5 classified by model as 8: 1 times
Examples labeled as 5 classified by model as 9: 3 times
Examples labeled as 6 classified by model as 0: 415 times
Examples labeled as 6 classified by model as 1: 152 times
Examples labeled as 6 classified by model as 2: 32 times
Examples labeled as 6 classified by model as 3: 67 times
Examples labeled as 6 classified by model as 5: 68 times
Examples labeled as 6 classified by model as 6: 216 times
Examples labeled as 6 classified by model as 7: 18 times
Examples labeled as 6 classified by model as 8: 20 times
Examples labeled as 6 classified by model as 9: 12 times
Examples labeled as 7 classified by model as 0: 179 times
Examples labeled as 7 classified by model as 1: 18 times
Examples labeled as 7 classified by model as 2: 9 times
Examples labeled as 7 classified by model as 3: 15 times
Examples labeled as 7 classified by model as 5: 126 times
Examples labeled as 7 classified by model as 7: 647 times
Examples labeled as 7 classified by model as 8: 1 times
Examples labeled as 7 classified by model as 9: 5 times
Examples labeled as 8 classified by model as 0: 390 times
Examples labeled as 8 classified by model as 1: 62 times
Examples labeled as 8 classified by model as 2: 12 times
Examples labeled as 8 classified by model as 3: 29 times
Examples labeled as 8 classified by model as 5: 11 times
Examples labeled as 8 classified by model as 7: 7 times
Examples labeled as 8 classified by model as 8: 484 times
Examples labeled as 8 classified by model as 9: 5 times
Examples labeled as 9 classified by model as 0: 111 times
Examples labeled as 9 classified by model as 1: 163 times
Examples labeled as 9 classified by model as 2: 1 times
Examples labeled as 9 classified by model as 3: 16 times
Examples labeled as 9 classified by model as 5: 14 times
Examples labeled as 9 classified by model as 7: 8 times
Examples labeled as 9 classified by model as 8: 6 times
Examples labeled as 9 classified by model as 9: 681 times
==========================Scores========================================
 # of classes:    10
 Accuracy:        0.5366
 Precision:       0.7001
 Recall:          0.5366
 F1 Score:        0.5267
Precision, recall & F1: macro-averaged (equally weighted avg. of 10 classes)
========================================================================
FINISHED   
Took 55 sec. Last updated by anonymous at September 03 2019, 1:25:32 AM.
