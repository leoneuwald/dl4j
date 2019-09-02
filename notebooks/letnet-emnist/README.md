# DL4J 0.9.1 - LeNet - EMNIST Letters

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
import org.deeplearning4j.datasets.iterator.impl.EmnistDataSetIterator
import org.deeplearning4j.eval.Evaluation
import org.deeplearning4j.spark.api.TrainingMaster
import org.deeplearning4j.spark.impl.paramavg.ParameterAveragingTrainingMaster
import org.nd4j.linalg.activations.Activation
import org.nd4j.linalg.dataset.DataSet
import org.nd4j.linalg.dataset.api.iterator.DataSetIterator
import org.nd4j.linalg.lossfunctions.LossFunctions
```

```scala
val emnistSet = EmnistDataSetIterator.Set.LETTERS

val numberOfLabels = EmnistDataSetIterator.numLabels(emnistSet)

val batchSize = 48

val emnistTrain = new EmnistDataSetIterator(emnistSet, batchSize, true)
val emnistTest = new EmnistDataSetIterator(emnistSet, batchSize, false)

import scala.collection.mutable.ListBuffer

var trainDataList = new ListBuffer[DataSet]()
while (emnistTrain.hasNext()) {
  trainDataList += emnistTrain.next()
}

val testDataList = new ListBuffer[DataSet]()
while (emnistTest.hasNext()) {
  testDataList += emnistTest.next()
}
val trainData = sc.parallelize(trainDataList)
val testData = sc.parallelize(testDataList)
```

```scala
val seed = 123
val iterations = 3
val zooModel = new LeNet(numberOfLabels, seed, 3)
```

```scala
import collection.JavaConverters._
zooModel.setInputShape(Array(Array(1, 28, 28)))
```

```scala
val tm = new ParameterAveragingTrainingMaster.Builder(batchSize)
  .workerPrefetchNumBatches(0)
  .saveUpdater(true)
  .averagingFrequency(5)
  .batchSizePerWorker(batchSize)
  .build()

val sparkNet = new SparkDl4jMultiLayer(sc, zooModel.conf, tm)
```

```scala
val numEpochs = 9
for (i <- 0 to numEpochs) {
  println(numEpochs)
  val trained = sparkNet.fit(trainData)
}
```

```scala
val resultado = sparkNet.doEvaluation(testData, 64, new Evaluation(numberOfLabels))(0)
println(resultado)
```

## Resultado
```
Examples labeled as 0 classified by model as 0: 680 times
Examples labeled as 0 classified by model as 1: 2 times
Examples labeled as 0 classified by model as 2: 1 times
Examples labeled as 0 classified by model as 3: 13 times
Examples labeled as 0 classified by model as 4: 8 times
Examples labeled as 0 classified by model as 5: 1 times
Examples labeled as 0 classified by model as 6: 8 times
Examples labeled as 0 classified by model as 7: 13 times
Examples labeled as 0 classified by model as 9: 1 times
Examples labeled as 0 classified by model as 10: 2 times
Examples labeled as 0 classified by model as 11: 1 times
Examples labeled as 0 classified by model as 12: 4 times
Examples labeled as 0 classified by model as 13: 9 times
Examples lab...
Examples labeled as 0 classified by model as 0: 680 times
Examples labeled as 0 classified by model as 1: 2 times
Examples labeled as 0 classified by model as 2: 1 times
Examples labeled as 0 classified by model as 3: 13 times
Examples labeled as 0 classified by model as 4: 8 times
Examples labeled as 0 classified by model as 5: 1 times
Examples labeled as 0 classified by model as 6: 8 times
Examples labeled as 0 classified by model as 7: 13 times
Examples labeled as 0 classified by model as 9: 1 times
Examples labeled as 0 classified by model as 10: 2 times
Examples labeled as 0 classified by model as 11: 1 times
Examples labeled as 0 classified by model as 12: 4 times
Examples labeled as 0 classified by model as 13: 9 times
Examples labeled as 0 classified by model as 14: 10 times
Examples labeled as 0 classified by model as 15: 5 times
Examples labeled as 0 classified by model as 16: 23 times
Examples labeled as 0 classified by model as 17: 2 times
Examples labeled as 0 classified by model as 19: 1 times
Examples labeled as 0 classified by model as 20: 3 times
Examples labeled as 0 classified by model as 22: 2 times
Examples labeled as 0 classified by model as 23: 1 times
Examples labeled as 0 classified by model as 25: 10 times
Examples labeled as 1 classified by model as 0: 5 times
Examples labeled as 1 classified by model as 1: 721 times
Examples labeled as 1 classified by model as 2: 1 times
Examples labeled as 1 classified by model as 3: 7 times
Examples labeled as 1 classified by model as 4: 8 times
Examples labeled as 1 classified by model as 6: 12 times
Examples labeled as 1 classified by model as 7: 18 times
Examples labeled as 1 classified by model as 8: 3 times
Examples labeled as 1 classified by model as 9: 1 times
Examples labeled as 1 classified by model as 11: 3 times
Examples labeled as 1 classified by model as 13: 2 times
Examples labeled as 1 classified by model as 14: 6 times
Examples labeled as 1 classified by model as 15: 1 times
Examples labeled as 1 classified by model as 16: 2 times
Examples labeled as 1 classified by model as 17: 1 times
Examples labeled as 1 classified by model as 18: 2 times
Examples labeled as 1 classified by model as 25: 7 times
Examples labeled as 2 classified by model as 0: 3 times
Examples labeled as 2 classified by model as 1: 1 times
Examples labeled as 2 classified by model as 2: 726 times
Examples labeled as 2 classified by model as 3: 2 times
Examples labeled as 2 classified by model as 4: 31 times
Examples labeled as 2 classified by model as 5: 1 times
Examples labeled as 2 classified by model as 6: 5 times
Examples labeled as 2 classified by model as 8: 3 times
Examples labeled as 2 classified by model as 11: 2 times
Examples labeled as 2 classified by model as 14: 4 times
Examples labeled as 2 classified by model as 15: 2 times
Examples labeled as 2 classified by model as 16: 3 times
Examples labeled as 2 classified by model as 17: 10 times
Examples labeled as 2 classified by model as 19: 2 times
Examples labeled as 2 classified by model as 20: 4 times
Examples labeled as 2 classified by model as 22: 1 times
Examples labeled as 3 classified by model as 0: 7 times
Examples labeled as 3 classified by model as 1: 8 times
Examples labeled as 3 classified by model as 2: 5 times
Examples labeled as 3 classified by model as 3: 707 times
Examples labeled as 3 classified by model as 6: 2 times
Examples labeled as 3 classified by model as 7: 2 times
Examples labeled as 3 classified by model as 8: 2 times
Examples labeled as 3 classified by model as 9: 4 times
Examples labeled as 3 classified by model as 10: 2 times
Examples labeled as 3 classified by model as 11: 1 times
Examples labeled as 3 classified by model as 13: 5 times
Examples labeled as 3 classified by model as 14: 34 times
Examples labeled as 3 classified by model as 15: 5 times
Examples labeled as 3 classified by model as 16: 6 times
Examples labeled as 3 classified by model as 18: 1 times
Examples labeled as 3 classified by model as 19: 3 times
Examples labeled as 3 classified by model as 20: 1 times
Examples labeled as 3 classified by model as 21: 2 times
Examples labeled as 3 classified by model as 24: 2 times
Examples labeled as 3 classified by model as 25: 1 times
Examples labeled as 4 classified by model as 0: 7 times
Examples labeled as 4 classified by model as 1: 1 times
Examples labeled as 4 classified by model as 2: 27 times
Examples labeled as 4 classified by model as 4: 741 times
Examples labeled as 4 classified by model as 5: 4 times
Examples labeled as 4 classified by model as 6: 4 times
Examples labeled as 4 classified by model as 8: 1 times
Examples labeled as 4 classified by model as 9: 1 times
Examples labeled as 4 classified by model as 11: 1 times
Examples labeled as 4 classified by model as 14: 2 times
Examples labeled as 4 classified by model as 15: 2 times
Examples labeled as 4 classified by model as 16: 2 times
Examples labeled as 4 classified by model as 17: 3 times
Examples labeled as 4 classified by model as 19: 1 times
Examples labeled as 4 classified by model as 20: 1 times
Examples labeled as 4 classified by model as 23: 1 times
Examples labeled as 4 classified by model as 25: 1 times
Examples labeled as 5 classified by model as 0: 1 times
Examples labeled as 5 classified by model as 1: 1 times
Examples labeled as 5 classified by model as 2: 1 times
Examples labeled as 5 classified by model as 3: 1 times
Examples labeled as 5 classified by model as 4: 3 times
Examples labeled as 5 classified by model as 5: 712 times
Examples labeled as 5 classified by model as 6: 6 times
Examples labeled as 5 classified by model as 8: 5 times
Examples labeled as 5 classified by model as 9: 2 times
Examples labeled as 5 classified by model as 11: 1 times
Examples labeled as 5 classified by model as 12: 1 times
Examples labeled as 5 classified by model as 15: 29 times
Examples labeled as 5 classified by model as 16: 6 times
Examples labeled as 5 classified by model as 17: 6 times
Examples labeled as 5 classified by model as 18: 2 times
Examples labeled as 5 classified by model as 19: 21 times
Examples labeled as 5 classified by model as 21: 1 times
Examples labeled as 5 classified by model as 25: 1 times
Examples labeled as 6 classified by model as 0: 21 times
Examples labeled as 6 classified by model as 1: 14 times
Examples labeled as 6 classified by model as 2: 10 times
Examples labeled as 6 classified by model as 3: 2 times
Examples labeled as 6 classified by model as 4: 4 times
Examples labeled as 6 classified by model as 5: 7 times
Examples labeled as 6 classified by model as 6: 593 times
Examples labeled as 6 classified by model as 8: 1 times
Examples labeled as 6 classified by model as 9: 7 times
Examples labeled as 6 classified by model as 10: 1 times
Examples labeled as 6 classified by model as 11: 1 times
Examples labeled as 6 classified by model as 13: 1 times
Examples labeled as 6 classified by model as 14: 2 times
Examples labeled as 6 classified by model as 15: 1 times
Examples labeled as 6 classified by model as 16: 115 times
Examples labeled as 6 classified by model as 17: 2 times
Examples labeled as 6 classified by model as 18: 7 times
Examples labeled as 6 classified by model as 19: 2 times
Examples labeled as 6 classified by model as 22: 2 times
Examples labeled as 6 classified by model as 23: 3 times
Examples labeled as 6 classified by model as 24: 4 times
Examples labeled as 7 classified by model as 0: 6 times
Examples labeled as 7 classified by model as 1: 3 times
Examples labeled as 7 classified by model as 3: 3 times
Examples labeled as 7 classified by model as 4: 1 times
Examples labeled as 7 classified by model as 6: 1 times
Examples labeled as 7 classified by model as 7: 715 times
Examples labeled as 7 classified by model as 8: 1 times
Examples labeled as 7 classified by model as 10: 9 times
Examples labeled as 7 classified by model as 11: 12 times
Examples labeled as 7 classified by model as 12: 8 times
Examples labeled as 7 classified by model as 13: 24 times
Examples labeled as 7 classified by model as 17: 2 times
Examples labeled as 7 classified by model as 19: 4 times
Examples labeled as 7 classified by model as 20: 4 times
Examples labeled as 7 classified by model as 22: 3 times
Examples labeled as 7 classified by model as 23: 1 times
Examples labeled as 7 classified by model as 24: 3 times
Examples labeled as 8 classified by model as 1: 1 times
Examples labeled as 8 classified by model as 2: 1 times
Examples labeled as 8 classified by model as 3: 1 times
Examples labeled as 8 classified by model as 4: 1 times
Examples labeled as 8 classified by model as 5: 1 times
Examples labeled as 8 classified by model as 6: 2 times
Examples labeled as 8 classified by model as 7: 2 times
Examples labeled as 8 classified by model as 8: 551 times
Examples labeled as 8 classified by model as 9: 14 times
Examples labeled as 8 classified by model as 10: 2 times
Examples labeled as 8 classified by model as 11: 206 times
Examples labeled as 8 classified by model as 17: 4 times
Examples labeled as 8 classified by model as 18: 3 times
Examples labeled as 8 classified by model as 19: 1 times
Examples labeled as 8 classified by model as 21: 1 times
Examples labeled as 8 classified by model as 22: 1 times
Examples labeled as 8 classified by model as 23: 2 times
Examples labeled as 8 classified by model as 25: 6 times
Examples labeled as 9 classified by model as 0: 1 times
Examples labeled as 9 classified by model as 1: 1 times
Examples labeled as 9 classified by model as 3: 17 times
Examples labeled as 9 classified by model as 6: 8 times
Examples labeled as 9 classified by model as 8: 30 times
Examples labeled as 9 classified by model as 9: 703 times
Examples labeled as 9 classified by model as 11: 5 times
Examples labeled as 9 classified by model as 16: 3 times
Examples labeled as 9 classified by model as 18: 5 times
Examples labeled as 9 classified by model as 19: 17 times
Examples labeled as 9 classified by model as 20: 2 times
Examples labeled as 9 classified by model as 21: 1 times
Examples labeled as 9 classified by model as 24: 3 times
Examples labeled as 9 classified by model as 25: 4 times
Examples labeled as 10 classified by model as 0: 2 times
Examples labeled as 10 classified by model as 1: 3 times
Examples labeled as 10 classified by model as 2: 4 times
Examples labeled as 10 classified by model as 3: 2 times
Examples labeled as 10 classified by model as 4: 2 times
Examples labeled as 10 classified by model as 5: 3 times
Examples labeled as 10 classified by model as 7: 21 times
Examples labeled as 10 classified by model as 8: 1 times
Examples labeled as 10 classified by model as 10: 727 times
Examples labeled as 10 classified by model as 11: 3 times
Examples labeled as 10 classified by model as 12: 2 times
Examples labeled as 10 classified by model as 13: 2 times
Examples labeled as 10 classified by model as 15: 1 times
Examples labeled as 10 classified by model as 17: 5 times
Examples labeled as 10 classified by model as 19: 6 times
Examples labeled as 10 classified by model as 20: 3 times
Examples labeled as 10 classified by model as 23: 10 times
Examples labeled as 10 classified by model as 24: 2 times
Examples labeled as 10 classified by model as 25: 1 times
Examples labeled as 11 classified by model as 1: 1 times
Examples labeled as 11 classified by model as 2: 5 times
Examples labeled as 11 classified by model as 3: 1 times
Examples labeled as 11 classified by model as 5: 2 times
Examples labeled as 11 classified by model as 7: 4 times
Examples labeled as 11 classified by model as 8: 131 times
Examples labeled as 11 classified by model as 9: 2 times
Examples labeled as 11 classified by model as 10: 2 times
Examples labeled as 11 classified by model as 11: 640 times
Examples labeled as 11 classified by model as 15: 1 times
Examples labeled as 11 classified by model as 16: 2 times
Examples labeled as 11 classified by model as 17: 4 times
Examples labeled as 11 classified by model as 19: 1 times
Examples labeled as 11 classified by model as 20: 2 times
Examples labeled as 11 classified by model as 21: 1 times
Examples labeled as 11 classified by model as 24: 1 times
Examples labeled as 12 classified by model as 0: 2 times
Examples labeled as 12 classified by model as 7: 5 times
Examples labeled as 12 classified by model as 10: 1 times
Examples labeled as 12 classified by model as 12: 754 times
Examples labeled as 12 classified by model as 13: 23 times
Examples labeled as 12 classified by model as 15: 2 times
Examples labeled as 12 classified by model as 19: 3 times
Examples labeled as 12 classified by model as 20: 2 times
Examples labeled as 12 classified by model as 22: 3 times
Examples labeled as 12 classified by model as 23: 2 times
Examples labeled as 12 classified by model as 24: 3 times
Examples labeled as 13 classified by model as 0: 14 times
Examples labeled as 13 classified by model as 3: 6 times
Examples labeled as 13 classified by model as 7: 15 times
Examples labeled as 13 classified by model as 9: 2 times
Examples labeled as 13 classified by model as 10: 2 times
Examples labeled as 13 classified by model as 12: 25 times
Examples labeled as 13 classified by model as 13: 710 times
Examples labeled as 13 classified by model as 15: 3 times
Examples labeled as 13 classified by model as 16: 1 times
Examples labeled as 13 classified by model as 17: 3 times
Examples labeled as 13 classified by model as 19: 1 times
Examples labeled as 13 classified by model as 20: 1 times
Examples labeled as 13 classified by model as 21: 5 times
Examples labeled as 13 classified by model as 22: 8 times
Examples labeled as 13 classified by model as 23: 4 times
Examples labeled as 14 classified by model as 0: 3 times
Examples labeled as 14 classified by model as 1: 2 times
Examples labeled as 14 classified by model as 2: 4 times
Examples labeled as 14 classified by model as 3: 14 times
Examples labeled as 14 classified by model as 4: 1 times
Examples labeled as 14 classified by model as 6: 2 times
Examples labeled as 14 classified by model as 13: 1 times
Examples labeled as 14 classified by model as 14: 765 times
Examples labeled as 14 classified by model as 15: 1 times
Examples labeled as 14 classified by model as 16: 2 times
Examples labeled as 14 classified by model as 20: 4 times
Examples labeled as 14 classified by model as 22: 1 times
Examples labeled as 15 classified by model as 0: 2 times
Examples labeled as 15 classified by model as 3: 4 times
Examples labeled as 15 classified by model as 4: 1 times
Examples labeled as 15 classified by model as 5: 5 times
Examples labeled as 15 classified by model as 6: 3 times
Examples labeled as 15 classified by model as 10: 1 times
Examples labeled as 15 classified by model as 11: 2 times
Examples labeled as 15 classified by model as 12: 1 times
Examples labeled as 15 classified by model as 13: 3 times
Examples labeled as 15 classified by model as 15: 764 times
Examples labeled as 15 classified by model as 16: 4 times
Examples labeled as 15 classified by model as 17: 7 times
Examples labeled as 15 classified by model as 19: 1 times
Examples labeled as 15 classified by model as 21: 1 times
Examples labeled as 15 classified by model as 24: 1 times
Examples labeled as 16 classified by model as 0: 43 times
Examples labeled as 16 classified by model as 1: 1 times
Examples labeled as 16 classified by model as 3: 7 times
Examples labeled as 16 classified by model as 4: 4 times
Examples labeled as 16 classified by model as 5: 10 times
Examples labeled as 16 classified by model as 6: 92 times
Examples labeled as 16 classified by model as 8: 3 times
Examples labeled as 16 classified by model as 9: 1 times
Examples labeled as 16 classified by model as 11: 1 times
Examples labeled as 16 classified by model as 12: 1 times
Examples labeled as 16 classified by model as 13: 1 times
Examples labeled as 16 classified by model as 14: 6 times
Examples labeled as 16 classified by model as 15: 7 times
Examples labeled as 16 classified by model as 16: 598 times
Examples labeled as 16 classified by model as 17: 1 times
Examples labeled as 16 classified by model as 19: 7 times
Examples labeled as 16 classified by model as 20: 3 times
Examples labeled as 16 classified by model as 22: 1 times
Examples labeled as 16 classified by model as 24: 12 times
Examples labeled as 16 classified by model as 25: 1 times
Examples labeled as 17 classified by model as 0: 21 times
Examples labeled as 17 classified by model as 2: 1 times
Examples labeled as 17 classified by model as 3: 2 times
Examples labeled as 17 classified by model as 4: 10 times
Examples labeled as 17 classified by model as 5: 7 times
Examples labeled as 17 classified by model as 8: 2 times
Examples labeled as 17 classified by model as 10: 8 times
Examples labeled as 17 classified by model as 11: 2 times
Examples labeled as 17 classified by model as 12: 2 times
Examples labeled as 17 classified by model as 13: 1 times
Examples labeled as 17 classified by model as 15: 6 times
Examples labeled as 17 classified by model as 16: 2 times
Examples labeled as 17 classified by model as 17: 706 times
Examples labeled as 17 classified by model as 18: 2 times
Examples labeled as 17 classified by model as 19: 5 times
Examples labeled as 17 classified by model as 21: 10 times
Examples labeled as 17 classified by model as 23: 5 times
Examples labeled as 17 classified by model as 24: 6 times
Examples labeled as 17 classified by model as 25: 2 times
Examples labeled as 18 classified by model as 0: 1 times
Examples labeled as 18 classified by model as 4: 3 times
Examples labeled as 18 classified by model as 6: 8 times
Examples labeled as 18 classified by model as 7: 1 times
Examples labeled as 18 classified by model as 8: 2 times
Examples labeled as 18 classified by model as 9: 7 times
Examples labeled as 18 classified by model as 16: 2 times
Examples labeled as 18 classified by model as 18: 375 times
Examples labeled as 18 classified by model as 24: 1 times
Warning: 7 classes were never predicted by the model and were excluded from average recall
Classes excluded from average recall: [19, 20, 21, 22, 23, 24, 25]
==========================Scores========================================
 # of classes:    26
 Accuracy:        0.8708
 Precision:       0.6489
 Recall:          0.8726	(7 classes excluded from average)
 F1 Score:        0.8798	(7 classes excluded from average)
Precision, recall & F1: macro-averaged (equally weighted avg. of 26 classes)
========================================================================
```
