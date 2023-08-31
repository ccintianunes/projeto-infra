# Projeto-infra
Seram implementados os seguintes serviços em Kubernetes: 
1. Artifatory, tendo as métricas coletadas pelo Prometheus e vizualizadas em um Dashboard no Grafana
2. WikiJS, como ferramenta de documentação, utilizando banco de dados Postgresql
3. Pgadmin, ferramenta que pode auxiliar na consulta rápida as bases

### Instalando o Artifacotory 
Seguindo a documentação oficial, a instalação do Artifacory pode ser feita utilizando o Helm, então certifique-se de ter o Heml instalado.

**1. Instalando o Helm**

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

**2. Adcione o repositório**
```
helm repo add jfrog https://charts.jfrog.io
helm repo update
heml pull jfrog/artifactory
tar -zxvf artifactory-107.63.14.tgz
cd artifactory
```
**3. Instalando o Serviço**
Para instalar o serviço na versão **7.6.3** passe o seguinte comando:

```
helm install --name artifactory --set artifactory.image.repository=docker.bintray.io/jfrog/artifactory-oss:7.6.3 jfrog/artifactory 
```

Ele irá criar os seguintes pods: 

![image](https://github.com/ccintianunes/projeto-infra/assets/110416764/6cc4bef3-ba60-4f71-874b-c260caf174fe)

**4. Habilitando o Serviço**

Para que o serviço fique disponínel faça as seguintes alterações no documento *value.yaml*

```
ESCREVER ALTERAÇÕES 
```
### Instalando o Prometheus 

**1. **


