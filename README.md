# Projeto-infra
Seram implementados os seguintes serviços em Kubernetes: 
1. Artifatory, tendo as métricas coletadas pelo Prometheus e vizualizadas em um Dashboard no Grafana
2. WikiJS, como ferramenta de documentação, utilizando banco de dados Postgresql
3. Pgadmin, ferramenta que pode auxiliar na consulta rápida as bases

## Instalando o Artifactory 

**1. Primeiro Clone do projeto**

```
git clone https://github.com/ccintianunes/projeto-infra.git

```

**2. Subindo o serviço artifactory**

no diretório 'artifactory' passe os comando abaixo:
```
kubectl apply -f artifactory.yaml
kubectl apply -f artifactory-pv.yaml
kubectl apply -f artifactory-pvc.yaml
```
O comando irá criar o Deployment e o Serviço, também irá criar o PersistentVolumeClaim (PVC) e o PersistentVolume (PV)

## Instalando o Prometheus
Implemente o Prometheus passando os seguintes comandos:

 No diretório 'prometheus' passe:
```
kubectl apply -f prometheus-setup.yaml
```

- O serviço pode agora ser vizualizado no navegador através do caminho "http://localhost:30000/ (substitua 'localhost' pelo ip da máquina local)

  ![image](https://github.com/ccintianunes/projeto-infra/assets/110416764/613e722e-a062-4cd4-a042-e99bffc75e11)  

Para que o Prometheus possa analisar a aplicação o 'Exporter' precisa ser instalado.

- Nesse caso coletaremos as metricas do **artifactory-exporter**


**1. Configurando o artifactory-exporter**

```
cd exporter-artifactory
kubectl -f apply artifactory-exporter.yaml
```
Com o serviço  sendo executado agora o Prometheus pode coletar os dados da aplicação.


## Instalando o Grafana
No diretório **grafana** Passe o seguinte comando:

```
kubectl apply -f grafana.yaml
```
Para verificar a porta onde o serviço está disponível passe ``kubectl get services``, o seguinte retorno é esperado:

![image](https://github.com/ccintianunes/projeto-infra/assets/110416764/d7c91605-d8e6-4ebc-bec8-d321a5ca0204)

Nesse caso serviço pode ser vizualizado em: "http://localhost:31495"

## Instalando Wikijs

Seguindo a documentação oficial, podemos instalar o wikijs utilizando o HELM

 **1. Instale o Helm**
 
 Faça download do binário do Helm:
 
```
wget https://get.helm.sh/helm-v3.12.3-linux-amd64.tar.gz
tar -zxvf helm-v3.12.3-linux-amd64.tar.gz
mv linux-amd64/helm /usr/local/bin/helm
```
 **2. Adicionando o repositório Wikijs**
 
 ```
helm repo add requarks https://charts.js.wiki
helm repo update
helm pull requarks/wiki
tar -zxvf wiki-2.2.20.tgz
helm install wiki requarks/wiki
```
 **3. Atualizando as variáveis de ambiente**
 
As váriaveis do serviço ficam por padrão do arquivo *values.yaml*

A variavel 'service' precisa ser alterada pra ficar da seguinte forma:
![image](https://github.com/ccintianunes/projeto-infra/assets/110416764/fabf4f83-87e4-492d-b338-fde528dbeec3)

O Banco de Dados pre-configurado pelo template do helm é o PosgresSQL, que subirá altomaticamente junto com o serviço.
```
helm upgrade wiki .
```

## Instalando PgAdmin4

no diretorio pgadmin execute:

```
kubectl apply -f deployment.yaml
```
O banco de dados estará disponivel em:
http://localhost:31000/

### Resultado

As aplicações estão instaladas em pods separados

![image](https://github.com/ccintianunes/projeto-infra/assets/110416764/2fe0462a-a008-463b-92b0-952756fbdd49)

Serviços

![image](https://github.com/ccintianunes/projeto-infra/assets/110416764/6a6ebb24-543d-4761-a90d-5e6a2965d4c2)

- Dificuldades:
  
  Encontrei dificuldade pra fazer o pull da imagem do artifactory: ``bintray.io/jfrog/artifactory-oss:7.6.3``
  


