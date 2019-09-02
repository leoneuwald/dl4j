# DL4J 0.9.1 - LeNet - Cifar10

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
import org.deeplearning4j.spark.impl.multilayer.SparkDl4jMultiLayer
import org.deeplearning4j.spark.impl.paramavg.ParameterAveragingTrainingMaster
import org.deeplearning4j.optimize.listeners._
import org.deeplearning4j.datasets.datavec.RecordReaderMultiDataSetIterator
import org.deeplearning4j.eval.Evaluation
import org.apache.spark.SparkConf
import org.deeplearning4j.zoo.model.LeNet
import org.apache.spark.api.java.JavaRDD
import org.apache.spark.api.java.JavaSparkContext
import org.deeplearning4j.spark.api.TrainingMaster
import org.deeplearning4j.spark.impl.paramavg.ParameterAveragingTrainingMaster
import org.nd4j.linalg.activations.Activation
import org.nd4j.linalg.dataset.DataSet
import org.nd4j.linalg.dataset.api.iterator.DataSetIterator
import org.nd4j.linalg.lossfunctions.LossFunctions
import org.datavec.image.loader.CifarLoader
import org.deeplearning4j.datasets.iterator.impl.CifarDataSetIterator
```

```scala
val numberOfLabels = CifarLoader.NUM_LABELS
val numberOfSamples = CifarLoader.NUM_TRAIN_IMAGES
val numberOfTestSamples = CifarLoader.NUM_TEST_IMAGES
val batchSizePerWorker = 16
```

```scala
import org.datavec.image.loader.BaseImageLoader
import collection.JavaConverters._
import java.io.File

val states = Map("filesFilename" -> "cifar-10-binary.tar.gz", "filesURL" -> "https://storage.googleapis.com/ufrgsneuwald/~kriz/cifar-10-binary.tar.gz", "filesFilenameUnzipped" -> "cifar-10-batches-bin")

BaseImageLoader.downloadAndUntar(states, new File(new File(System.getProperty("user.home")), "cifar"))
```

```scala
val trainingDataSet = new CifarDataSetIterator(batchSizePerWorker, numberOfSamples, true)
val testDataSet = new CifarDataSetIterator(batchSizePerWorker, numberOfTestSamples, false)
import scala.collection.mutable.ListBuffer

var trainDataList = new ListBuffer[DataSet]()
while (trainingDataSet.hasNext()) {
  trainDataList += trainingDataSet.next()
}

val testDataList = new ListBuffer[DataSet]()
while (testDataSet.hasNext()) {
  testDataList += testDataSet.next()
}
val trainData = sc.parallelize(trainDataList)
val testData = sc.parallelize(testDataList)
```

```scala
val seed = 12345
val zooModel = new LeNet(numberOfLabels, seed, 3)
```

```scala
import collection.JavaConverters._
zooModel.setInputShape(Array(Array(3, 32, 32)))
```

```scala
val tm = new ParameterAveragingTrainingMaster.Builder(batchSizePerWorker)
  .workerPrefetchNumBatches(0)
  .saveUpdater(true)
  .averagingFrequency(5)
  .batchSizePerWorker(batchSizePerWorker)
  .build()

val sparkNet = new SparkDl4jMultiLayer(sc, zooModel.conf(), tm)
```

```scala
val numEpochs = 9
for (i <- 0 to numEpochs) {
  val trained = sparkNet.fit(trainData)
}
```

```scala
val resultado = sparkNet.doEvaluation(testData, 64, new Evaluation(10))(0)
println(resultado)
```

## Resultado
```
Examples labeled as 0 classified by model as 0: 834 times
Examples labeled as 0 classified by model as 1: 2 times
Examples labeled as 0 classified by model as 2: 105 times
Examples labeled as 0 classified by model as 3: 27 times
Examples labeled as 0 classified by model as 4: 5 times
Examples labeled as 0 classified by model as 5: 1 times
Examples labeled as 0 classified by model as 6: 3 times
Examples labeled as 0 classified by model as 7: 1 times
Examples labeled as 0 classified by model as 8: 12 times
Examples labeled as 0 classified by model as 9: 10 times
Examples labeled as 1 classified by model as 1: 989 times
Examples labeled as 1 classified by model as 2: 8 times
Examples labeled as 1 classified by model as 5: 1 times
Examples la...
Examples labeled as 0 classified by model as 0: 834 times
Examples labeled as 0 classified by model as 1: 2 times
Examples labeled as 0 classified by model as 2: 105 times
Examples labeled as 0 classified by model as 3: 27 times
Examples labeled as 0 classified by model as 4: 5 times
Examples labeled as 0 classified by model as 5: 1 times
Examples labeled as 0 classified by model as 6: 3 times
Examples labeled as 0 classified by model as 7: 1 times
Examples labeled as 0 classified by model as 8: 12 times
Examples labeled as 0 classified by model as 9: 10 times
Examples labeled as 1 classified by model as 1: 989 times
Examples labeled as 1 classified by model as 2: 8 times
Examples labeled as 1 classified by model as 5: 1 times
Examples labeled as 1 classified by model as 7: 1 times
Examples labeled as 1 classified by model as 9: 1 times
Examples labeled as 2 classified by model as 0: 5 times
Examples labeled as 2 classified by model as 2: 986 times
Examples labeled as 2 classified by model as 3: 3 times
Examples labeled as 2 classified by model as 4: 2 times
Examples labeled as 2 classified by model as 5: 1 times
Examples labeled as 2 classified by model as 6: 2 times
Examples labeled as 2 classified by model as 7: 1 times
Examples labeled as 3 classified by model as 2: 23 times
Examples labeled as 3 classified by model as 3: 963 times
Examples labeled as 3 classified by model as 4: 6 times
Examples labeled as 3 classified by model as 5: 2 times
Examples labeled as 3 classified by model as 6: 4 times
Examples labeled as 3 classified by model as 7: 2 times
Examples labeled as 4 classified by model as 2: 63 times
Examples labeled as 4 classified by model as 3: 11 times
Examples labeled as 4 classified by model as 4: 913 times
Examples labeled as 4 classified by model as 5: 3 times
Examples labeled as 4 classified by model as 6: 9 times
Examples labeled as 4 classified by model as 8: 1 times
Examples labeled as 5 classified by model as 2: 59 times
Examples labeled as 5 classified by model as 3: 35 times
Examples labeled as 5 classified by model as 4: 7 times
Examples labeled as 5 classified by model as 5: 889 times
Examples labeled as 5 classified by model as 6: 6 times
Examples labeled as 5 classified by model as 7: 3 times
Examples labeled as 5 classified by model as 9: 1 times
Examples labeled as 6 classified by model as 2: 26 times
Examples labeled as 6 classified by model as 3: 8 times
Examples labeled as 6 classified by model as 4: 6 times
Examples labeled as 6 classified by model as 5: 1 times
Examples labeled as 6 classified by model as 6: 959 times
Examples labeled as 7 classified by model as 0: 1 times
Examples labeled as 7 classified by model as 2: 36 times
Examples labeled as 7 classified by model as 3: 10 times
Examples labeled as 7 classified by model as 4: 13 times
Examples labeled as 7 classified by model as 5: 4 times
Examples labeled as 7 classified by model as 7: 935 times
Examples labeled as 7 classified by model as 9: 1 times
Examples labeled as 8 classified by model as 0: 6 times
Examples labeled as 8 classified by model as 1: 3 times
Examples labeled as 8 classified by model as 2: 29 times
Examples labeled as 8 classified by model as 3: 16 times
Examples labeled as 8 classified by model as 4: 2 times
Examples labeled as 8 classified by model as 5: 1 times
Examples labeled as 8 classified by model as 8: 942 times
Examples labeled as 8 classified by model as 9: 1 times
Examples labeled as 9 classified by model as 0: 2 times
Examples labeled as 9 classified by model as 1: 4 times
Examples labeled as 9 classified by model as 2: 13 times
Examples labeled as 9 classified by model as 3: 9 times
Examples labeled as 9 classified by model as 4: 3 times
Examples labeled as 9 classified by model as 5: 1 times
Examples labeled as 9 classified by model as 6: 1 times
Examples labeled as 9 classified by model as 7: 1 times
Examples labeled as 9 classified by model as 8: 8 times
Examples labeled as 9 classified by model as 9: 958 times
==========================Scores========================================
 # of classes:    10
 Accuracy:        0.9368
 Precision:       0.9462
 Recall:          0.9368
 F1 Score:        0.9384
Precision, recall & F1: macro-averaged (equally weighted avg. of 10 classes)
========================================================================
```
