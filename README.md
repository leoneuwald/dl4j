# Deep Learning 4J

Repositório para armazenamento dos notebooks do Zeppelin utilizados no TCC da Especialização de BigData & DataScience da UFRGS.

## GCloud

Foram criados vários clusters diferentes conforme cada um dos experimentos do trabalho. Abaixo seguem os scripts de criação utilizados:

### Modelos conhecidos

Comando do gcloud para criação dos clustes usados no capítulo de datasets e modelos conhecidos.

```
gcloud dataproc clusters create neuwaldcluster --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-b --master-machine-type n1-standard-8 --master-boot-disk-size 25 --num-workers 6 --worker-machine-type c2-standard-4 --worker-boot-disk-size 25 --image-version 1.1
```

Comandos do gcloud para criação dos clustes usados no capítulo de avaliação de performance em diferentes clusters.

```
gcloud dataproc clusters create tcc2 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --master-machine-type n1-standard-8 --master-boot-disk-size 25 --num-workers 2 --worker-machine-type n1-standard-8 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone southamerica-east1-b --master-machine-type n1-standard-8 --master-boot-disk-size 25 --num-workers 2 --worker-machine-type n1-standard-8 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tccn2 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-2 --master-boot-disk-size 25 --num-workers 6 --worker-machine-type n2-standard-2 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc6 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-2 --master-boot-disk-size 25 --num-workers 6 --worker-machine-type n1-standard-2 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc6h --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-4 --master-boot-disk-size 25 --num-workers 6 --worker-machine-type n1-standard-4 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc12 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-4 --master-boot-disk-size 25 --num-workers 12 --worker-machine-type n1-standard-2 --worker-boot-disk-size 25 --image-version 1.1

gcloud dataproc clusters create tcc6 --initialization-actions gs://ufrgsneuwald/zeppelin/zeppelin.sh --zone us-central1-a --master-machine-type n1-standard-4 --master-boot-disk-size 25 --num-workers 12 --worker-machine-type n1-standard-2 --worker-boot-disk-size 25 --image-version 1.1
```

### Script para mudar arquivo de hosts

Os dados do DL4J 0.9.1 não estão mais disponíveis no endereço antigo. Para usar os exemplos antigos é necessário mudar o arquivo de hosts.

```
sudo su
echo "52.229.32.188 benchmark.deeplearn.online" >> /etc/hosts
exit
```

## Ajustar memória do Zeppelin
```
sudo su
cd /etc/zeppelin/conf
vi zeppelin-env.sh
-Xms10g -Xmx10g -XX:MaxPermSize=6g
service zeppelin restart
```

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
