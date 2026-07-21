#  API de Pedidos - Automação e Deploy na Magalu Cloud

##  Sobre o Projeto
Este repositório contém a evolução de uma API estática para um ambiente totalmente automatizado e escalável. O foco principal foi a implementação de uma esteira completa de Integração e Entrega Contínuas (CI/CD) e o provisionamento de infraestrutura em nuvem, garantindo segurança e alta disponibilidade.

## 🛠️Arquitetura e Tecnologias
* **Cloud Provider:** Magalu Cloud
* **Orquestração de Contêineres:** Kubernetes (K3s)
* **CI/CD:** GitHub Actions
* **Containerização:** Docker & Magalu Container Registry
* **Rede e Roteamento:** Traefik Ingress Controller
* **Segurança:** Gestão de Secrets nativos do Kubernetes e configuração rigorosa de Security Groups (Firewall).

## A Esteira de Automação (GitHub Actions)
O arquivo `.github/workflows/deploy.yml` orquestra o seguinte fluxo:
1. Validação da estrutura de código a cada novo `push`.
2. Autenticação segura no Container Registry da Magalu Cloud via Secrets.
3. Build da imagem Docker da API.
4. Tagueamento automático e envio (Push) da imagem pronta para o cofre na nuvem.

##  Troubleshooting & Desafios Solucionados
Durante o ciclo de vida da implantação, cenários reais de infraestrutura foram diagnosticados e resolvidos:
* **Resolução de `ImagePullBackOff`:** Diagnóstico de falha de autenticação do Kubernetes. O problema foi corrigido aplicando o conceito de isolamento de segurança, injetando o Secret de credenciais do repositório de imagens diretamente no namespace correto (`move-tech-api`), permitindo que os Pods baixassem a imagem.
* **Resolução de `ERR_CONNECTION_TIMED_OUT`:** Diagnóstico de bloqueio de rede na camada de nuvem. O tráfego foi restabelecido criando uma regra de entrada (Inbound Rule) no Security Group da Magalu Cloud, liberando o protocolo TCP na porta 80 para o Ingress Controller.

## Acesso
A aplicação está conteinerizada, rodando com réplicas para alta disponibilidade, e sua documentação interativa (Swagger) pode ser acessada publicamente via IP da Máquina Virtual, validando a integração de ponta a ponta.
