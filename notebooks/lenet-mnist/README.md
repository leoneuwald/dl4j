# DL4J 0.9.1 - LeNet - MNIST

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
import org.deeplearning4j.spark.api.TrainingMaster
import org.deeplearning4j.spark.impl.multilayer.SparkDl4jMultiLayer
import org.deeplearning4j.spark.impl.paramavg.ParameterAveragingTrainingMaster
import org.deeplearning4j.optimize.listeners._
import org.deeplearning4j.datasets.datavec.RecordReaderMultiDataSetIterator
import org.deeplearning4j.eval.Evaluation
import org.deeplearning4j.zoo.model.LeNet
import org.apache.spark.SparkConf
import org.apache.spark.api.java.JavaRDD
import org.apache.spark.api.java.JavaSparkContext
import org.deeplearning4j.datasets.iterator.impl.MnistDataSetIterator
import org.deeplearning4j.eval.Evaluation
import org.deeplearning4j.spark.api.TrainingMaster
import org.deeplearning4j.spark.impl.multilayer.SparkDl4jMultiLayer
import org.deeplearning4j.spark.impl.paramavg.ParameterAveragingTrainingMaster
import org.nd4j.linalg.activations.Activation
import org.nd4j.linalg.dataset.DataSet
import org.nd4j.linalg.dataset.api.iterator.DataSetIterator
import org.nd4j.linalg.lossfunctions.LossFunctions
```

```scala
val batchSizePerWorker = 16
val iterTrain = new MnistDataSetIterator(batchSizePerWorker, true, 12345)
val iterTest = new MnistDataSetIterator(batchSizePerWorker, false, 12345)

import scala.collection.mutable.ListBuffer
var trainDataList = new ListBuffer[DataSet]()
while (iterTrain.hasNext()) {
  trainDataList += iterTrain.next()
}

val testDataList = new ListBuffer[DataSet]()
while (iterTest.hasNext()) {
  testDataList += iterTest.next()
}

val trainData = sc.parallelize(trainDataList)
val testData = sc.parallelize(testDataList)
```

```scala
val classesNum = 10
val seed = 123

val zooModel = new LeNet(classesNum, seed, 3)
```

```scala
import collection.JavaConverters._
zooModel.setInputShape(Array(Array(1, 28, 28)))
```

```scala
val tm = new ParameterAveragingTrainingMaster.Builder(batchSizePerWorker)
  .workerPrefetchNumBatches(0)
  .saveUpdater(true)
  .averagingFrequency(5)
  .batchSizePerWorker(batchSizePerWorker)
  .build()

val sparkNet = new SparkDl4jMultiLayer(sc, zooModel.conf(), tm)
```

```scala
val numEpochs = 5
for (i <- 0 to numEpochs) {
  println(numEpochs)
  val trained = sparkNet.fit(trainData)
}
```

```scala
val resultado = sparkNet.doEvaluation(testData, 64, new Evaluation(10))(0)
println(resultado)
```

## Resultado
```
Examples labeled as 0 classified by model as 0: 976 times
Examples labeled as 0 classified by model as 6: 2 times
Examples labeled as 0 classified by model as 7: 1 times
Examples labeled as 0 classified by model as 8: 1 times
Examples labeled as 1 classified by model as 1: 1131 times
Examples labeled as 1 classified by model as 2: 2 times
Examples labeled as 1 classified by model as 6: 1 times
Examples labeled as 1 classified by model as 7: 1 times
Examples labeled as 2 classified by model as 0: 3 times
Examples labeled as 2 classified by model as 1: 1 times
Examples labeled as 2 classified by model as 2: 1025 times
Examples labeled as 2 classified by model as 4: 1 times
Examples labeled as 2 classified by model as 7: 2 times
Examples lab...
Examples labeled as 0 classified by model as 0: 976 times
Examples labeled as 0 classified by model as 6: 2 times
Examples labeled as 0 classified by model as 7: 1 times
Examples labeled as 0 classified by model as 8: 1 times
Examples labeled as 1 classified by model as 1: 1131 times
Examples labeled as 1 classified by model as 2: 2 times
Examples labeled as 1 classified by model as 6: 1 times
Examples labeled as 1 classified by model as 7: 1 times
Examples labeled as 2 classified by model as 0: 3 times
Examples labeled as 2 classified by model as 1: 1 times
Examples labeled as 2 classified by model as 2: 1025 times
Examples labeled as 2 classified by model as 4: 1 times
Examples labeled as 2 classified by model as 7: 2 times
Examples labeled as 3 classified by model as 3: 1003 times
Examples labeled as 3 classified by model as 5: 3 times
Examples labeled as 3 classified by model as 7: 1 times
Examples labeled as 3 classified by model as 8: 2 times
Examples labeled as 3 classified by model as 9: 1 times
Examples labeled as 4 classified by model as 4: 979 times
Examples labeled as 4 classified by model as 9: 3 times
Examples labeled as 5 classified by model as 0: 1 times
Examples labeled as 5 classified by model as 3: 4 times
Examples labeled as 5 classified by model as 5: 884 times
Examples labeled as 5 classified by model as 6: 1 times
Examples labeled as 5 classified by model as 9: 2 times
Examples labeled as 6 classified by model as 0: 7 times
Examples labeled as 6 classified by model as 1: 2 times
Examples labeled as 6 classified by model as 4: 2 times
Examples labeled as 6 classified by model as 5: 5 times
Examples labeled as 6 classified by model as 6: 942 times
Examples labeled as 7 classified by model as 1: 2 times
Examples labeled as 7 classified by model as 2: 5 times
Examples labeled as 7 classified by model as 3: 1 times
Examples labeled as 7 classified by model as 4: 1 times
Examples labeled as 7 classified by model as 7: 1015 times
Examples labeled as 7 classified by model as 8: 1 times
Examples labeled as 7 classified by model as 9: 3 times
Examples labeled as 8 classified by model as 0: 3 times
Examples labeled as 8 classified by model as 1: 1 times
Examples labeled as 8 classified by model as 2: 1 times
Examples labeled as 8 classified by model as 3: 2 times
Examples labeled as 8 classified by model as 4: 2 times
Examples labeled as 8 classified by model as 5: 2 times
Examples labeled as 8 classified by model as 6: 1 times
Examples labeled as 8 classified by model as 7: 2 times
Examples labeled as 8 classified by model as 8: 957 times
Examples labeled as 8 classified by model as 9: 3 times
Examples labeled as 9 classified by model as 0: 1 times
Examples labeled as 9 classified by model as 1: 2 times
Examples labeled as 9 classified by model as 3: 1 times
Examples labeled as 9 classified by model as 4: 7 times
Examples labeled as 9 classified by model as 5: 1 times
Examples labeled as 9 classified by model as 7: 3 times
Examples labeled as 9 classified by model as 9: 994 times
==========================Scores========================================
 # of classes:    10
 Accuracy:        0.9906
 Precision:       0.9906
 Recall:          0.9905
 F1 Score:        0.9905
Precision, recall & F1: macro-averaged (equally weighted avg. of 10 classes)
========================================================================
```
