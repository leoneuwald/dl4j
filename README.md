# Deep Learning 4J

Repositório para armazenamento dos notebooks do Zeppelin utilizados no TCC da Especialização de BigData & DataScience da UFRGS.

## GCloud

Foram criados vários clusters diferentes conforme cada um dos experimentos do trabalho. Abaixo seguem os scripts de criação utilizados:

### Modelos conhecidos

```
gcloud dataproc clusters create models-cluster --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-b --master-machine-type n1-standard-4 --master-boot-disk-size 25 --num-workers 4 --worker-machine-type c2-standard-4 --worker-boot-disk-size 25 --image-version 1.1
```

### Script para mudar arquivo de hosts

Os dados do DL4J 0.9.1 não estão mais disponíveis no endereço antigo. Para usar os exemplos antigos é necessário mudar o arquivo de hosts.

```
sudo su && echo "52.229.32.188 benchmark.deeplearn.online" >> /etc/hosts && exit
```

## Ajustar memória do Zeppelin
```
cd /etc/zeppelin/conf
sudo su

-Xms5g -Xmx5g -XX:MaxPermSize=4g

vi zeppelin-env.sh
service zeppelin restart
```

### Diferentes configurações de Cluster


gcloud dataproc clusters create tcc2 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --master-machine-type n1-standard-8 --master-boot-disk-size 25 --num-workers 2 --worker-machine-type n1-standard-8 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone southamerica-east1-b --master-machine-type n1-standard-8 --master-boot-disk-size 25 --num-workers 2 --worker-machine-type n1-standard-8 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tccn2 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-2 --master-boot-disk-size 25 --num-workers 6 --worker-machine-type n2-standard-2 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc6 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-2 --master-boot-disk-size 25 --num-workers 6 --worker-machine-type n1-standard-2 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc6h --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-4 --master-boot-disk-size 25 --num-workers 6 --worker-machine-type n1-standard-4 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc12 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-4 --master-boot-disk-size 25 --num-workers 12 --worker-machine-type n1-standard-2 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc6 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-4 --master-boot-disk-size 25 --num-workers 12 --worker-machine-type n1-standard-2 --worker-boot-disk-size 25 --image-version 1.1

## Delete Cluster

## Mudar Hosts no Master

## VPN
```
gcloud compute ssh tcc-m \
  --project=hidden-analyzer-249419 \
  --zone=southamerica-east1-b -- -D 1080 -N
```

```
gcloud compute ssh tcc6w-m \
  --project=hidden-analyzer-249419 \
  --zone=us-central1-a -- -D 1080 -N
```

## Google Chrome
```
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --proxy-server="socks5://localhost:1080" \
  --user-data-dir="/tmp/tcc-m" http://tcc-m:8088
  ```

```
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --proxy-server="socks5://localhost:1080" \
  --user-data-dir="/tmp/tcc6w-m" http://tcc6w-m:8088
```





startEvaluation: Long = 1566266256171
resultado: org.deeplearning4j.eval.Evaluation =
Examples labeled as 0 classified by model as 0: 5826 times
Examples labeled as 0 classified by model as 1: 2 times
Examples labeled as 0 classified by model as 2: 12 times
Examples labeled as 0 classified by model as 3: 2 times
Examples labeled as 0 classified by model as 4: 2 times
Examples labeled as 0 classified by model as 5: 4 times
Examples labeled as 0 classified by model as 6: 37 times
Examples labeled as 0 classified by model as 7: 1 times
Examples labeled as 0 classified by model as 8: 32 times
Examples labeled as 0 classified by model as 9: 5 times
Examples labeled as 1 classified by model as 0: 1 times
Examples labeled as 1 classified by model as 1: 6700 times
Examples labeled as 1 classified by model as 2: 16 times
Examples l...
Examples labeled as 0 classified by model as 0: 5826 times
Examples labeled as 0 classified by model as 1: 2 times
Examples labeled as 0 classified by model as 2: 12 times
Examples labeled as 0 classified by model as 3: 2 times
Examples labeled as 0 classified by model as 4: 2 times
Examples labeled as 0 classified by model as 5: 4 times
Examples labeled as 0 classified by model as 6: 37 times
Examples labeled as 0 classified by model as 7: 1 times
Examples labeled as 0 classified by model as 8: 32 times
Examples labeled as 0 classified by model as 9: 5 times
Examples labeled as 1 classified by model as 0: 1 times
Examples labeled as 1 classified by model as 1: 6700 times
Examples labeled as 1 classified by model as 2: 16 times
Examples labeled as 1 classified by model as 3: 1 times
Examples labeled as 1 classified by model as 4: 6 times
Examples labeled as 1 classified by model as 6: 1 times
Examples labeled as 1 classified by model as 7: 5 times
Examples labeled as 1 classified by model as 8: 11 times
Examples labeled as 1 classified by model as 9: 1 times
Examples labeled as 2 classified by model as 0: 8 times
Examples labeled as 2 classified by model as 1: 29 times
Examples labeled as 2 classified by model as 2: 5822 times
Examples labeled as 2 classified by model as 3: 17 times
Examples labeled as 2 classified by model as 4: 9 times
Examples labeled as 2 classified by model as 5: 1 times
Examples labeled as 2 classified by model as 6: 10 times
Examples labeled as 2 classified by model as 7: 18 times
Examples labeled as 2 classified by model as 8: 42 times
Examples labeled as 2 classified by model as 9: 2 times
Examples labeled as 3 classified by model as 0: 5 times
Examples labeled as 3 classified by model as 1: 20 times
Examples labeled as 3 classified by model as 2: 53 times
Examples labeled as 3 classified by model as 3: 5885 times
Examples labeled as 3 classified by model as 4: 1 times
Examples labeled as 3 classified by model as 5: 59 times
Examples labeled as 3 classified by model as 6: 2 times
Examples labeled as 3 classified by model as 7: 19 times
Examples labeled as 3 classified by model as 8: 78 times
Examples labeled as 3 classified by model as 9: 9 times
Examples labeled as 4 classified by model as 0: 5 times
Examples labeled as 4 classified by model as 1: 18 times
Examples labeled as 4 classified by model as 2: 10 times
Examples labeled as 4 classified by model as 3: 1 times
Examples labeled as 4 classified by model as 4: 5691 times
Examples labeled as 4 classified by model as 5: 4 times
Examples labeled as 4 classified by model as 6: 28 times
Examples labeled as 4 classified by model as 7: 16 times
Examples labeled as 4 classified by model as 8: 16 times
Examples labeled as 4 classified by model as 9: 53 times
Examples labeled as 5 classified by model as 0: 13 times
Examples labeled as 5 classified by model as 1: 13 times
Examples labeled as 5 classified by model as 2: 7 times
Examples labeled as 5 classified by model as 3: 23 times
Examples labeled as 5 classified by model as 4: 2 times
Examples labeled as 5 classified by model as 5: 5284 times
Examples labeled as 5 classified by model as 6: 38 times
Examples labeled as 5 classified by model as 7: 3 times
Examples labeled as 5 classified by model as 8: 32 times
Examples labeled as 5 classified by model as 9: 6 times
Examples labeled as 6 classified by model as 0: 12 times
Examples labeled as 6 classified by model as 1: 11 times
Examples labeled as 6 classified by model as 2: 2 times
Examples labeled as 6 classified by model as 4: 9 times
Examples labeled as 6 classified by model as 5: 9 times
Examples labeled as 6 classified by model as 6: 5861 times
Examples labeled as 6 classified by model as 8: 14 times
Examples labeled as 7 classified by model as 0: 12 times
Examples labeled as 7 classified by model as 1: 26 times
Examples labeled as 7 classified by model as 2: 50 times
Examples labeled as 7 classified by model as 3: 10 times
Examples labeled as 7 classified by model as 4: 13 times
Examples labeled as 7 classified by model as 5: 3 times
Examples labeled as 7 classified by model as 6: 2 times
Examples labeled as 7 classified by model as 7: 6106 times
Examples labeled as 7 classified by model as 8: 14 times
Examples labeled as 7 classified by model as 9: 29 times
Examples labeled as 8 classified by model as 0: 10 times
Examples labeled as 8 classified by model as 1: 34 times
Examples labeled as 8 classified by model as 2: 4 times
Examples labeled as 8 classified by model as 3: 20 times
Examples labeled as 8 classified by model as 4: 4 times
Examples labeled as 8 classified by model as 5: 12 times
Examples labeled as 8 classified by model as 6: 12 times
Examples labeled as 8 classified by model as 7: 1 times
Examples labeled as 8 classified by model as 8: 5746 times
Examples labeled as 8 classified by model as 9: 8 times
Examples labeled as 9 classified by model as 0: 16 times
Examples labeled as 9 classified by model as 1: 15 times
Examples labeled as 9 classified by model as 2: 1 times
Examples labeled as 9 classified by model as 3: 35 times
Examples labeled as 9 classified by model as 4: 52 times
Examples labeled as 9 classified by model as 5: 46 times
Examples labeled as 9 classified by model as 6: 4 times
Examples labeled as 9 classified by model as 7: 51 times
Examples labeled as 9 classified by model as 8: 51 times
Examples labeled as 9 classified by model as 9: 5678 times
#ERROR!
# of classes: 10
Accuracy: 0.9767
Precision: 0.9767
Recall: 0.9765
F1 Score: 0.9765
Precision, recall & F1: macro-averaged (equally weighted avg. of 10 classes)
#ERROR!
endEvaluation: Long = 1566266287856
res8: String = 31685 ms
