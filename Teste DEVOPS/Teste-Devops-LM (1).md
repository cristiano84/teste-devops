Criar uma imagem Docker rodando um Hello World em NojeJS com arquivos estáticos (index.html)

A estrutura de arquivos da aplicação deverá ser algo próximo à esta abaixo:

.
├── app.js
├── public
   ├── index.html
   ├── script.js
    └── style.js

Após criar a imagem, configure um minikube (https://minikube.sigs.k8s.io/docs/) e faça o deploy deste no kubernetes.

Crie um Git Publico com o código usado e um repositório também público para a imagem, de forma que consigamos validar a atividade.
https://github.com/cristiano84/teste-devops
https://hub.docker.com/repository/docker/cris84/teste-devops/general
Evidêncie executando em linha de comando:

- Rode com o curl e exiba o resultado
Foi apresentado a construção do arquivo index.html como o texto hello world
![image](https://github.com/cristiano84/teste-devops/assets/43679779/da4b873c-47fc-45ff-8eb0-3ca163fd1255)
- Como foi executado o build da imagem
  Dentro do diretorio raiz do Dockerfile, utilizando os comandos:"docker build -t <nome da imagem> ." e o comando para iniciar "docker run -it <nome da imagem>"
- Como validar que a imagem funcionou no Docker
  Podemos validar os logs, acessar o container ou realizar uma requisição
- Como fez o deploy no Kubernetes
  Configurado o minikube para realizar o processo, utilizando o deployment generico do minikube para realizar o deploy
- Como obter que os recursos da aplicação no kubernetes foram aplicados
  Podemos utilizar o comando "kubectl get deployments", onde será mostrado os dados da execução.
- Como obter os recursos de CPU e memória dos Pods e do Node
 Utilizar o comando "kubectl top pods" e "kubectl top nodes"

Questões

- Como gerar um certificado SSL no Letsencrypt?
  R: Se já tiver o certbot instalado, é necessário definir o modo como obter certificado (Standalone, webroot ou manual)
     Exemplo: sudo certbot certonly --standalone -d cristianoaquino.com
     Após isso será necessário informar alguns campos como email e aceitar termos de uso
- Como criar um serviço com IP Publico no Kubernetes e como obter o endereço deste serviço?
  Definir um arquivo yaml de deployment, contendo os pods que serão exposto
  Criar um arquivo de serviço.yaml(por exemplo) contendo um loadbalancer ou Nodeport configurado
  Após isso e rodando podemos utilizar o comando kubectl describe services/(nome do serviço), onde será expecificado o external IP do serviço.
- Como criar um serviço com IP Privado no Kubernetes e como obter o endereço deste serviço?
  Para um serviço com IP Privado, seguimos o mesmo padrão acima, porem no arquivo de serviço, teremos um Cluster IP configurado, que será usado um IP interno na rede do Cluster
  kubectl get services *nome do serviço*, será mostrado o campo CLUSTER-IP contendo o ip interno do kubernetes
- Como criar um serviço acessível apenas dentro do Cluster do Kubernetes?
  Podemos configurar um yaml de serviço contendo um CLUSTER-IP onde o serviço será acessivel apenas da rede interna dos cluster
- Como configurar um certificado SSL no Kubernetes?
  O certificado pode ser gerado pelo letsencrypt e após isso criar uma secret no kubernetes contendo os valores
  Para expor um serviço utilizando o certificado para controle, podemos criar um arquivo de configuração contendo um ingress controller, apóos isso realizar o apply kubectlapply -f arquivo-do-ingress.yaml
- Qual a diferença do Deployment e Statefullset?
  Deployment seria um arquivo para implantação do pod e seu ciclo de vida e o StatefulSet é para aplicações que requerem a persistencia de estado do pod
- Explique quando devemos usar um DNS do tipo CNAME e um do tipo A?
  Um dns do tipo A é usado para associar um nome de domínio por exemplo a um ip especifico  CNAME é utilizado para criar alias para um nome de dominio ou sejá um redirecionamento para um DNS especifico.
- Em um cenário em que o Desenvolvedor não consegue acessar uma base RDS retornando Connection Timeout, como é feito o troubleshoot e qual a possível causa?
  Podemos ir eliminando possiveis causas como, verificar conectividade de rede ver se está na mesma rede do RDS, verificar porta de conexão, firewall, verificar configurações do rds, verificar permissões IAM, ver monitoramento do RDS para identificar algo.
- E se o erro no cenário acima for Connection Refused?
  Verificar grupos de seguranças associados, porta de conexão
- Explique a diferença entre Jenkins e GihubActions. Qual sua preferência para fluxo de CI/CD e por quê?
  Jenkins usa fases para executar uma coleção de etapas e o github actions usa trabalhos para agrupar etapas, github actions utiliza os jobs na infra da nuvem do git, jenkins tem uma infraestrutura propria necesitando manutenção e gerenciamento
  Preferência pelo github actions devido a simplicidade e a integração com o github
- Já usou ArgoCD? Em que parte do Fluxo de CI/CD ele é utilizado e qual seu papel?
  Nunca usei, mas ele é utilizado para implementação do passo de Deployment, onde pode ser especificado o estado desejado da infraestrutura
