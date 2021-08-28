# Shipay DevOps Challenge 


## Observações: 

O teste foi desenvolvido para entender como o candidato pensa em diferentes situações. Não é um teste objetivo e existem várias soluções corretas para os problemas propostos. Queremos saber como é o seu modo de trabalho.


## Introdução

Nesse repositório está presente o codigo fonte de duas aplicações, ambas em python e utilizando poetry como gerenciador de dependências. O serviço `api_rest` prove uma API que responde a um GET na porta 8000 e o serviço `async_worker` envia e processa eventos em um canal do Kafka(o Kafka É uma dependencia externa dessa aplicação).


## Dicas:
* use mensagens de commit consistentes.
* aplique boas praticas que já trabalhou ou leu em documentações.
* faça as mudanças que achar necessário na estrutura de repositório. 
* não se atenha a esse repositório, caso prefira, separe em dois ou mais.
* mantenha a calma que vai dar bom. 


## O desafio: 

1. Containerização: crie uma imagem de docker base, e extenda sua imagem para cada aplicação. Instale uma cadeia de certificados publica qualquer na base padrão ex: Comodo SSL, DigiCert, Entrust Datacard, GoDaddy, etc.. Nas imagens extendidas, instale as dependencias, e configure a imagem para utilizar o comando `ENTRYPOINT` para iniciar as aplicações. Documente o processo de build e run nos devidos arquivos, e.g. `README.md`.

    Bonus: Dependências externas da aplicação podem subir a parte com um `docker-compose.yaml`.

2. Empacotamento: baseado no repositório apresentado, crie um Helm chart contendo todos os componentes necessários para essa aplicação rodar em um cluster de Kubernetes local (kind, minikube, Kubernetes Docker Desktop). Você pode utilizar Kustomize caso se sentir a vontade. A aplicação `api_rest` precisa estar acessível por um `Service` do Kubernetes (ClusterIP) na porta 443. A aplicação `async_worker` não precisa de um `Service` do Kubernetes. 

    Bonus: A instancia da aplicação `api_rest` performa bem em ec2 com a label `shipay.com.br/capacity-type = spot` enquanto a aplicação `async_worker` só pode rodar em ec2 com a label `shipay.com.br/capacity-type = ondemand`. Configure Affinity's para garantir melhor distribuição dos workloads.

3. Pipeline: o candidato pode optar por uma entre duas opções para o desafio de pipeline. Uma pipeline de build ou uma pipeline de deploy. Nessa parte do desafio, não é importante se ater a tecnologia. Utilize o que se sentir confortável: GitlabCI, Jenkis, TravisCI, CircleCI, Github Actions, Tekton, DroneCI, JenkinsX, Spinnaker, Codefresh, ArgoCD, etc.. É preferível utilizar ferramentas aderentes ao fluxo de versionamento gitflow, mas não é mandatário. É imprescindível a necessidade de versionamento da pipeline como código. Para builds, escreva uma pipeline para buildar as imagens de docker e enviar para o dockerhub público. Apesar de o repositório ter a estrutura de `monorepo`, não é necessário que os builds das aplicações sejam trigados individualmentes e.g. cada trigger handler pode buildar as 3 imagens e enviar para as 3 imagens o repositório de imagens. Para deploy, utilize a estratégia de empacotamento utilizada no passo anterior para implantar as aplicações e suas dependencias externas.  

    Bonus: Implemente ambas as pipelines. 
