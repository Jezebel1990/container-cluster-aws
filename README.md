# Arquitetura de Ambiente na AWS com Amazon ECS

> Este projeto apresenta uma arquitetura moderna e escalável na AWS, utilizando containers com ECS (Fargate), além de recursos como VPC, ALB, ECR, WAF e S3. O diagrama foi desenvolvido com o draw.io e conta com elementos animados para melhor visualização da comunicação entre os serviços.

## Diagrama da Arquitetura
![Diagrama AWS ECS](./assets/aws.gif)

## Ferramentas

- [Draw.io](https://app.diagrams.net/): Para criação e animação do diagrama
- [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/): Ícones oficiais utilizados no diagrama

---

##  Descrição da Arquitetura
### 🔹 FrontEnd — Entrega Estática e Proteção na Borda

- **Usuários → Internet**  
  A aplicação é acessada por usuários externos via navegador.

- **Route 53 (DNS Zone)**  
  Gerencia o roteamento de domínios personalizados para os endpoints da aplicação.

- **AWS WAF (Web Application Firewall)**  
  Fornece proteção contra ameaças na borda da rede, como injeções de SQL, XSS e outros ataques comuns.

- **Amazon CloudFront (CDN Distribution)**  
  Distribui o conteúdo estático com baixa latência, cacheando arquivos em edge locations globais.

- **Amazon S3 (Website Bucket)**  
  Hospeda os arquivos estáticos da aplicação, como HTML, CSS, JS e imagens — normalmente associados ao front-end de aplicações SPA ou JAMstack.


### 🔸 BackEnd — Orquestração com ECS Fargate

- **Amazon VPC (Virtual Private Cloud)**  
  Ambiente de rede isolado com controle de IPs, roteamento e firewalls. Inclui duas Zonas de Disponibilidade (`sa-east-1a` e `sa-east-1b`) para garantir alta disponibilidade.

- **Subnets Públicas e Privadas**  
  Recursos expostos ao público (como o ALB) são alocados em subnets públicas, enquanto containers e tarefas sensíveis rodam em subnets privadas com acesso controlado.

- **Application Load Balancer (ALB)**  
  Distribui o tráfego de entrada entre os serviços de back-end hospedados em containers ECS, garantindo balanceamento de carga inteligente e escalabilidade.

- **Amazon ECS com Fargate (Cluster de Containers)**  
  Permite execução de containers Docker sem provisionamento de servidores. As tarefas são:

  - **Tasks ECS em subnets privadas**: executadas com segurança, isoladas do tráfego externo.
  - **Auto Scaling Group**: escalonamento automático de tarefas com base em métricas como uso de CPU ou número de requisições.

- **Elastic Network Interfaces (ENIs)**  
  Interfaces de rede associadas às tasks do ECS para permitir comunicação privada e segura dentro da VPC.

- **Amazon S3 (Bucket “Images”)**  
  Armazena arquivos gerados ou utilizados pela aplicação — como imagens, documentos ou dados estáticos.

- **Amazon ECR (Elastic Container Registry)**  
  Armazena imagens Docker utilizadas pelas tarefas do ECS. Neste caso, duas imagens distintas (`Image 1` e `Image 2`) são mantidas no repositório privado da AWS para uso nos containers.

---

### ✅ Benefícios da Arquitetura

- **Escalabilidade** com ECS + Auto Scaling  
- **Alta disponibilidade** com múltiplas zonas (AZs)  
- **Segurança** com WAF, subnets privadas e ENIs  
- **Desempenho otimizado** com CloudFront e ALB  
- **Infraestrutura moderna baseada em containers (Docker)**

---

## Licença

Este projeto está licenciado sob a Licença MIT. Consulte o arquivo LICENSE para mais informações.

Feito com ♥ por [Jezebel Guedes](https://www.linkedin.com/in/jezebel-guedes/) 👋 Entre em contato!