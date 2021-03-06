# NGINX-HELM

## Pre-Req

- Ter um cluster Kubernetes já configurado.
- Ter o conhecimento básico em Kubernetes e Linux

## O que é o Helm

O Helm é um gerenciador de pacotes criado para facilitar a instalação de aplicações e suas dependências no Kubernetes.
Podemos comparar o Helm com o APT do Debian, pois com apenas um comando você consegue instalar aplicações e suas dependencias no Kubernetes e ainda, fazer o gerenciamento de suas versões, podendo fazer o upgrade ou downgrade sem maiores problemas e rapidamente.
O Helm não é somente utilizado para fazer a instalação de aplicativos de terceiros, você consegue criar `charts`, que são os pacotes que o Helm utiliza para a instalação e configuração do aplicativo no Kubernetes.
O chart é composto por arquivos que definem como e qual deve ser o comportamento da aplicação dentro do cluster. É no chart que você define o seu deployment, o service, ingress e qualquer outra coisa necessária para a instalação e configuração da app desejada, e para isso, utilizamos os templates, que serão abordados mais para frente.
Acho que já sabemos o que é o Helm, evidente, iremos entende-lo melhor e sua aplicação dentro da nossa realidade conforme avançamos no treinamento.

## Instalação

O Helm é um bínario Go, e sua instalação é bastante simples.
Hoje o Helm é suportado por diversos sistemas operacionais, como Linux, MacOS, BSD e Windows.

Para realizar a instalação do nosso Helm, siga os passos abaixo:

### Linux

```bash
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

### MacOS

```bash
brew install helm
```

### Primeiros passos com o Helm

Para verificar a versão do Helm instalada:

```bash
helm version

version.BuildInfo{Version:"v3.5.3", GitCommit:"041ce5a2c17a58be0fcd5f5e16fb3e7e95fea622", GitTreeState:"dirty", GoVersion:"go1.15.8"}
```

Acessar o help e assim, todos os parametros que podemos utilizar com o Helm:

```bash
helm help
```

Adicionando um repositório de charts:

```bash
helm repo add stable https://charts.helm.sh/stable
```

Após adicionar um repo, você precisa executar um update para pegar as informações mais recentes do repositório:

```bash
helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "ingress-nginx" chart repository
...Successfully got an update from the "jetstack" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
```

Para pesquisar os charts disponíveis em seu repositório `stable`:

```bash
helm search repo stable
```

Para procurar por determinado chart no https://artifacthub.io/

```bash
helm search hub nginx
```

Visualizar os detalhes de determinado chart:

```bash
helm show chart stable/mysql
```

Criando a nossa estrutura de arquivos e diretórios para o nosso chart:

```bash
helm create giropops
```

Instalando o comando tree para melhor visualização da estrutura de arquivos:

```bash
sudo apt-get install tree -y
```

Estrutura do chart:

```
└── giropops
    ├── charts
    ├── Chart.yaml
    ├── templates
    │   ├── deployment.yaml
    │   ├── _helpers.tpl
    │   ├── hpa.yaml
    │   ├── ingress.yaml
    │   ├── NOTES.txt
    │   ├── serviceaccount.yaml
    │   ├── service.yaml
    │   └── tests
    │       └── test-connection.yaml
    └── values.yaml
```

Realizando o deploy do nosso primeiro chart:

```bash
helm install giropops giropops/ --values giropops/values.yaml
```

Listando os charts:

```bash
helm list
```

Realizando o upgrade do nosso chart:

```bash
helm upgrade giropops giropops/ --values giropops/values.yaml
```

Realizando o rollback para a `revision 1`:

```bash
helm rollback giropops 1
```

Visualizando o histórico de ações de determinado chart deployado:

```bash
helm history giropops
```
