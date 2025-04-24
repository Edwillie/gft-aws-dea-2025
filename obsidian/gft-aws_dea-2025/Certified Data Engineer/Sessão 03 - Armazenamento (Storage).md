# S3 - Simple Storage Service
# EBS - Elastic Block Storage
### ✅ **Visão Geral**
#### 📌 O que é o EBS?
- EBS (Elastic Block Store) é um serviço de armazenamento de blocos da AWS usado com instâncias EC2.    
- Permite persistência de dados mesmo após encerramento da instância.    
- Pode ser desconectado de uma instância EC2 e conectado a outra.    
- Funciona via rede (não é armazenamento local), sujeito a latência.    

#### 📌 Características principais:
- **Persistente**: os dados não são perdidos quando a instância é encerrada.    
- **Escopo de AZ**: volumes são vinculados a uma _Availability Zone_ (AZ) específica.    
- **Delete on Termination**:    
    - Volumes **raiz** (root) têm essa opção ativada por padrão.        
    - Volumes **adicionais** não são excluídos por padrão.

- Pode haver múltiplos volumes EBS anexados a uma única instância.    
- Um volume só pode ser anexado a **uma instância por vez**.    

#### 📌 Tipos de Volume (exemplos citados):
- **gp2** e **gp3** (General Purpose SSD): usados no exemplo.    
- O material usa **30 GB gratuitos por mês**, conforme elegibilidade do Free Tier.    

#### 📌 Elastic Volumes (🚨 foco para a prova):
- Permite **modificar tamanho, tipo e performance** (IOPS e throughput) do volume **sem downtime**.    
- Mudanças como aumento de tamanho ou troca de gp2 para gp3 podem ser feitas em tempo real.    
- **Não é possível reduzir** o tamanho de um volume.    

#### 📌 Comandos no Console:
- Criar volume e anexar manualmente a uma instância.    
- Necessário que a **AZ do volume e da instância coincidam**.    
- Exemplo prático: criação de volumes de 2 GB e 8 GB, anexados a EC2.    
- Demonstração do comportamento do **Delete on Termination** após encerrar a instância.    

---

### 🔍 **Lacunas e Complementos Importantes para a Prova**
#### 🏷️ Tipos adicionais de volume EBS:
- **io1 / io2**: SSDs com alta performance de IOPS, usados em bancos de dados críticos.    
- **st1 (Throughput Optimized HDD)**: ótimo para grandes volumes com acesso sequencial.    
- **sc1 (Cold HDD)**: custo reduzido, usado para dados acessados com pouca frequência.    

#### 💸 Cobrança:
- Baseada em:    
    - **Tamanho provisionado (GB/mês)**,        
    - **IOPS provisionado (se aplicável)**,        
    - **Snapshots armazenados no S3**,        
    - **Taxas de transferência de dados (em alguns casos)**.        

#### 🛠️ Snapshots:
- São backups ponto-a-ponto armazenados no S3.    
- Permitem **restaurar volumes** e **mover dados entre AZs e regiões**.    
- São **incrementais** após o primeiro snapshot completo.    

#### 🔄 Backup e Restauração:
- O AWS Backup pode automatizar backups de volumes EBS.    
- Restauração pode ser feita em outra AZ/região com base no snapshot.    

#### 📥 Performance:
- **gp3** oferece IOPS e throughput _independentes_ do tamanho do volume.    
- **gp2** vincula performance ao tamanho (3 IOPS/GB até 16.000 IOPS).    

#### 📦 BOOT e DATA:
- Volume **root** (boot) contém o sistema operacional.    
- Volumes **adicionais** contêm dados de aplicações ou arquivos persistentes.
# EFS - Elastic File System

### ✅ **Visão Geral**
#### 📌 **Conceito**
- EFS é um sistema de arquivos de rede gerenciado (NFS) na AWS.
- Permite que múltiplas instâncias EC2, inclusive em diferentes **Zonas de Disponibilidade (AZs)**, acessem o mesmo sistema de arquivos simultaneamente.
- Escalável, durável, altamente disponível, mas **mais caro que o EBS** (até 3x mais).

#### 📌 **Características Técnicas**
- **Compatível apenas com AMIs Linux**, devido ao uso do sistema de arquivos **POSIX**.    
- Usa o protocolo **NFSv4.1 ou NFSv4.0**.    
- **Criptografia em repouso** pode ser ativada via **AWS KMS**.    
- Não requer provisão de capacidade: é **escalável automaticamente**.

#### 📌 **Casos de uso comuns**
- Web servers e CMS (ex: WordPress)    
- Compartilhamento de dados entre aplicações    
- Ambientes multi-AZ de alta disponibilidade

---
### ⚙️ **Modos de Desempenho (Performance Modes)**
Escolhido na criação do sistema de arquivos:
- **General Purpose (default)**: baixa latência; ideal para web servers, CMS etc.    
- **Max I/O**: maior throughput, tolera latência; ideal para big data e processamento de mídia.    

---
### 📈 **Modos de Taxa de Transferência (Throughput Modes)**
- **Bursting** (explosivo): automático, escala com o armazenamento usado.    
- **Provisioned**: throughput fixo independente do armazenamento.    
- **Elastic (recomendado)**: ajusta dinamicamente a E/S conforme a carga.    

---
### 💾 **Classes e Camadas de Armazenamento**
- **Standard**: dados acessados com frequência.    
- **EFS-IA (Infrequent Access)**: menor custo, mas cobra por recuperação.    
- **One Zone-IA**: mais barato, mas com menor disponibilidade.    
- **Lifecycle Management**: política para mover arquivos automaticamente entre camadas com base no tempo sem acesso (ex: 30 dias → EFS-IA, 90 dias → arquivamento).    

---
### 🌐 **Configuração de Rede e Segurança**
- Necessário definir:    
    - **VPC/Sub-rede**        
    - **Grupo de segurança com porta 2049 liberada (NFS)** entre EC2 e EFS.        
- O console do EC2 já configura automaticamente as dependências se montar o EFS durante a criação da instância.    

---

### 🆚 **Comparação: EFS vs. EBS**

|Característica|EFS|EBS|
|---|---|---|
|Tipo|Sistema de arquivos de rede (NFS)|Volume de bloco|
|Multi-AZ|Sim|Não (snapshot para migrar)|
|Multi-attach|Sim (até milhares de instâncias)|Limitado a 1 (exceto io1/io2 com multi-attach)|
|Performance|Escalável automaticamente|IOPS pode ser provisionado|
|Custo|Mais caro (~3x GP2)|Mais barato|
|Compatibilidade|Somente Linux (POSIX)|Linux e Windows|
|Elasticidade|Totalmente elástico|Capacidade provisionada|
|Casos de uso|Web, CMS, compartilhamento|DB, aplicações de alto I/O|

---
### ✅ **Boas práticas para certificação**
- **Use EFS Regional** em produção (multi-AZ).    
- **One Zone-IA** pode ser usado para desenvolvimento/teste com menor custo.    
- **EFS-IA com Lifecycle** é recomendado para reduzir custos com dados pouco acessados.    
- **Elastic throughput** é a melhor escolha para cargas imprevisíveis.    
- **Crie regras de segurança adequadas**, permitindo porta **2049 TCP** para NFS.    

---
### 🔍 **Lacunas e Complementos Importantes para a Prova**

1. **Backup**:    
    - Backups automáticos podem ser ativados.        
    - Use o **AWS Backup** para políticas centralizadas.        

2. **Monitoramento**:    
    - Use **CloudWatch** para métricas como throughput, latência e uso de storage.    

3. **Limites e Cotas**:    
    - Um único sistema de arquivos EFS pode crescer até **petabytes**.        
    - Latência pode ser maior que EBS (Max I/O tem latência mais alta).

4. **Montagem manual**:    
    - Em sistemas que não usam a automação do console, é necessário instalar o pacote `amazon-efs-utils` e montar manualmente com `mount -t efs`.

5. **Persistência e Integridade**:    
    - EFS é **altamente durável**, com replicação em várias AZs.        
    - Mas lembre-se: **não é um substituto para backup**.

6. **Custo-benefício**:    
    - Analisar uso real: EFS é caro para cargas pequenas que não exigem múltiplos acessos simultâneos.        
    - Combine com **EBS** ou **S3** para workloads híbridos.        

---

### 📚 Dica final de flashcard para memorização

**📌 Amazon EFS:**

> 🔁 Multi-AZ, 🔒 POSIX/Linux, 💸 Mais caro que EBS, 📂 Compartilhamento via NFS, 🧠 Ideal para CMS/Web, 📊 Escala automática, ⚙️ Modos: General Purpose / Max I/O, 📈 Throughput: Bursting / Provisioned / Elastic, 📦 Storage: Standard / IA / One Zone-IA.

# BACKUP

### ✅ **Visão Geral**
- **AWS Backup** é um serviço **gerenciado e centralizado** para automatizar backups em diversos serviços da AWS.    
- Ideal para **evitar scripts manuais** e obter **visibilidade unificada** da estratégia de backup.    

### 🧩 **Serviços Compatíveis**
- **Compute & Storage**: EC2, EBS, EFS, FSx (Windows, Lustre), S3    
- **Bancos de Dados**: RDS, Aurora, DynamoDB, DocumentDB, Neptune    
- **Híbrido**: AWS Storage Gateway (Volume Gateway)    

### 🛡️ **Recursos Importantes**
- **Cross-Region Backup**: backup pode ser replicado entre regiões (Disaster Recovery).    
- **Cross-Account Backup**: pode replicar entre contas AWS (multi-conta).    
- **Recuperação Pontual**: suportada para serviços como Aurora.    
- **Vault Lock (WORM)**: Write Once Read Many – impede exclusão de backups, até mesmo pelo usuário root.    
- **Cold Storage**: possível mover dados para armazenamento mais barato com tempo de retenção definido.    

### ⚙️ **Planos e Políticas**
- **Planos de Backup (Backup Plans)**:    
    - Permite definir **frequência** (diária, semanal, mensal, cron).        
    - **Janela de backup** (ex.: iniciar às 05:00 UTC).        
    - **Retenção** (dias, semanas, anos).        
    - **Transição para Cold Storage** após determinado período.

- **Backup Vault**:    
    - Cofre onde os backups são armazenados.        
    - Pode ser o padrão da AWS ou customizado.

- **Backup com Tags**:    
    - Aplicação automática com **políticas baseadas em tags** (ex.: `Environment=Production`).        

### 🛠️ **Funcionalidades Operacionais**
- **Backup sob demanda** ou **agendado**.    
- **Criação de planos via console, modelos prontos ou JSON.**    
- **Criação de atribuições (Assignments)**:    
    - Atribuem recursos ao plano.        
    - Permite usar **IAM Role padrão ou personalizada**.        
    - **Seleção por recurso específico ou por tag**.

- **Jobs de Backup/Restore/Copy** são listados na interface.    
- **Monitoramento e auditoria** são possíveis via CloudWatch, CloudTrail e AWS Config.    

---

### 🔍 **Lacunas e Complementos Importantes para a Prova**

#### 🔐 **Vault Lock (WORM) – Detalhe Crítico para a Prova**
- Ao ativar o **Vault Lock**, nem o root da conta pode excluir backups.    
- Para fins regulatórios e de compliance, é frequentemente **perguntado em provas**.    

#### 📊 **Cobrança (Billing)**
- A cobrança ocorre por:    
    - **Armazenamento de backup ativo e cold.**        
    - **Transferência entre regiões (se aplicável).**        
    - **Backups sob demanda ou programados.**

- **Cold storage é mais barato**, mas com maior tempo de recuperação.    

#### 📘 **Integração com Outros Serviços**
- **CloudWatch**: para monitoramento de jobs.    
- **CloudTrail**: para auditoria de chamadas e operações.    
- **AWS Organizations**: possível integrar estratégia multi-conta via políticas de backup organizacionais.    

#### 🚫 **Limitações ou Restrições**
- Nem todos os serviços suportam **recuperação pontual** (ex.: RDS sim, EBS não).    
- Nem todos os serviços suportam **cold storage**.    
- Algumas funcionalidades exigem **permissões IAM específicas** – cuidado ao configurar roles customizadas.    

#### 📥 **Importante Saber para a Prova**
- Diferença entre **Backup Plan**, **Backup Vault**, **Assignment**.    
- Como aplicar **políticas baseadas em tags**.    
- Como usar **Cross-Region** e **Cross-Account** backup corretamente.    
- Significado e implicações de **Vault Lock/WORM**.    

---
### 📚 Dica final de flashcard para memorização

|Termo|Definição|
|---|---|
|**Backup Plan**|Conjunto de regras para agendar, reter e copiar backups.|
|**Vault Lock (WORM)**|Proteção contra exclusão de backups, mesmo pelo root.|
|**Backup Assignment**|Atribuição de recursos (por ID ou tag) a um plano.|
|**Cold Storage**|Armazenamento de baixo custo com maior tempo de acesso.|
|**Cross-Account Backup**|Replicação de backups entre contas AWS.|
|**Cross-Region Backup**|Replicação de backups entre regiões AWS.|
|**Backup Vault**|Cofre de armazenamento para os backups criados.|
|**Tag-based Policy**|Políticas que automatizam backup com base em tags dos recursos.|
# FLASH CARDS
### **Flashcards: Amazon S3**

---

**🃏 Flashcard 1**

---

**🃏 Flashcard 2**

---

**🃏 Flashcard 3**

---

**🃏 Flashcard 4**

---

**🃏 Flashcard 5**

---

**🃏 Flashcard 6**

---

**🃏 Flashcard 7**

---

**🃏 Flashcard 8**

---

**🃏 Flashcard 9**

---

**🃏 Flashcard 10**

---

**🃏 Flashcard 11**

---

**🃏 Flashcard 12**

---

**🃏 Flashcard 13**

---

**🃏 Flashcard 14**

---

**🃏 Flashcard 15**

---

**🃏 Flashcard 16**

---

**🃏 Flashcard 17**

---

**🃏 Flashcard 18**

---

**🃏 Flashcard 19**


### **Flashcards: Amazon EBS**

---

**🃏 Flashcard 1**

**Q:** O que é o Amazon EBS?  
**A:** Um serviço de armazenamento de blocos persistente para EC2, que permite salvar dados mesmo após o encerramento da instância. 

---

**🃏 Flashcard 2**

**Q:** O que significa EBS?  
**A:** Elastic Block Store.

---

**🃏 Flashcard 3**

**Q:** Um volume EBS pode ser anexado a quantas instâncias EC2 simultaneamente (modo padrão)?  
**A:** Apenas uma instância por vez.

---

**🃏 Flashcard 4**

**Q:** O que acontece com o volume raiz EBS ao encerrar uma instância EC2, por padrão?  
**A:** Ele é excluído se a opção “Delete on Termination” estiver ativada (ativada por padrão).

---

**🃏 Flashcard 5**

**Q:** Volumes adicionais EBS são excluídos ao encerrar a instância?  
**A:** Não, a opção “Delete on Termination” está desativada por padrão nesses volumes.

---

**🃏 Flashcard 6**

**Q:** O que define a performance de um volume EBS?  
**A:** O tamanho do volume e os IOPS provisionados.

---

**🃏 Flashcard 7**

**Q:** Qual a relação entre volumes EBS e zonas de disponibilidade (AZs)?  
**A:** Volumes EBS são vinculados a uma única AZ e só podem ser anexados a instâncias na mesma AZ.

---

**🃏 Flashcard 8**

**Q:** Como mover um volume EBS entre AZs?  
**A:** Criando um snapshot e restaurando em outra AZ.

---

**🃏 Flashcard 9**

**Q:** O que são Elastic Volumes no contexto do EBS?  
**A:** Recurso que permite modificar o tamanho, tipo e performance de um volume sem desconectar ou parar a instância.

---

**🃏 Flashcard 10**

**Q:** É possível reduzir o tamanho de um volume EBS?  
**A:** Não, apenas aumentar.

---

**🃏 Flashcard 11**

**Q:** O que são IOPS no EBS?  
**A:** Input/Output Operations Per Second, indicador de performance do volume.

---

**🃏 Flashcard 12**

**Q:** Qual tipo de volume EBS permite definir IOPS e throughput separadamente?  
**A:** gp3.

---

**🃏 Flashcard 13**

**Q:** Qual a performance padrão do volume gp2?  
**A:** 3 IOPS por GB, até 16.000 IOPS.

---

**🃏 Flashcard 14**

**Q:** O que é um snapshot EBS?  
**A:** Backup incremental de um volume EBS, armazenado no S3.

---

**🃏 Flashcard 15**

**Q:** É possível anexar múltiplos volumes EBS a uma única instância EC2?  
**A:** Sim.

---

**🃏 Flashcard 16**

**Q:** Qual volume EBS é ideal para cargas de leitura sequencial, como Big Data?  
**A:** st1 (Throughput Optimized HDD).

---

**🃏 Flashcard 17**

**Q:** O que acontece se um volume EBS não estiver anexado a uma instância?  
**A:** Ele permanece disponível, pronto para ser anexado sob demanda.

---

**🃏 Flashcard 18**

**Q:** É possível alterar o tipo de volume (ex: gp2 para gp3) com a instância em execução?  
**A:** Sim, com Elastic Volumes.

---

**🃏 Flashcard 19**

**Q:** Quais operações podem ser feitas com Elastic Volumes sem downtime?  
**A:** Alterar tipo, aumentar tamanho, alterar IOPS e throughput.

### **Flashcards: Amazon EFS**

---

**🃏 Flashcard 1**  
**Q:** O que é o Amazon EFS?  
**A:** É um sistema de arquivos em rede (NFS) gerenciado, escalável e altamente disponível, usado por múltiplas instâncias EC2 simultaneamente, inclusive em diferentes zonas de disponibilidade.

---

**🃏 Flashcard 2**  
**Q:** EFS é compatível com qual sistema operacional?  
**A:** Apenas sistemas **Linux**, devido ao uso do padrão **POSIX**. (não compatível com Windows).

---

**🃏 Flashcard 3**  
**Q:** Como é feita a cobrança no EFS?  
**A:** Por **uso (GB/mês)**, sem necessidade de provisionamento de capacidade antecipado.

---

**🃏 Flashcard 4**  
**Q:** Quais são os dois modos de desempenho do EFS?  
**A:**
1. **General Purpose** – baixa latência (padrão).
2. **Max I/O** – maior throughput, porém com latência mais alta.

---

**🃏 Flashcard 5**  
**Q:** Quais são os modos de throughput disponíveis no EFS?  
**A:**
- **Bursting** (**padrão** - ajusta com base no tamanho do storage)
- **Provisioned** (valor fixo)
- **Elastic throughput** (automático baseado em carga de trabalho) -  **(recomendado)**

---

**🃏 Flashcard 6**  
**Q:** Quais são as classes (ou camadas) de armazenamento do EFS?  
**A:**
- **Standard** (acesso frequente)
- **EFS-IA** (Infrequent Access)
- **One Zone**
- **One Zone-IA** (baixo custo, menor disponibilidade)

---

**🃏 Flashcard 7**  
**Q:** O que é o recurso de políticas de ciclo de vida (_Lifecycle Management_) no EFS?  
**A:** Permite mover automaticamente arquivos para classes de armazenamento mais econômicas após um período de inatividade (ex: 30 ou 90 dias).

---

**🃏 Flashcard 8**  
**Q:** Qual protocolo de rede é utilizado no EFS?  
**A:** **NFS v4.1/v4.0**

---

**🃏 Flashcard 9**  
**Q:** É possível criptografar os dados no EFS?  
**A:** Sim, com **AWS KMS** para criptografia em repouso.

---

**🃏 Flashcard 10**  
**Q:** Como o EFS garante alta disponibilidade e resiliência?  
**A:** Armazenando dados de forma redundante em **múltiplas zonas de disponibilidade** (exceto nas classes One Zone).

---

**🃏 Flashcard 11**  
**Q:** Qual diferença entre EFS e EBS?  
**A:**
- **EFS:** acessado por múltiplas instâncias EC2 via rede.
- **EBS:** vinculado a uma única instância EC2 por vez.

---

**🃏 Flashcard 12**  
**Q:** Para qual uso o modo Max I/O é ideal?  
**A:** **Big Data** e **processamento de mídia**, que exigem alta taxa de transferência e paralelismo.

---

**🃏 Flashcard 13**  
**Q:** O que são Access Points no EFS?  
**A:** São **pontos de entrada com permissões específicas**, úteis para multi-tenant ou aplicações com múltiplos usuários.

---

**🃏 Flashcard 14**

**Q:** Qual porta deve ser liberada no grupo de segurança para permitir acesso ao EFS?  
**A:** Porta TCP 2049 (protocolo NFSv4).

---

**🃏 Flashcard 15**

**Q:** Quais os principais casos de uso do EFS?  
**A:**
- WordPress
- Web servers
- Compartilhamento de conteúdo e dados

---

**🃏 Flashcard 16**

**Q:** Quais as vantagens do EFS em relação ao EBS?  
**A:**
- Multi-AZ
- Acesso simultâneo por várias instâncias
- Escalabilidade automática
- Sem necessidade de provisionamento antecipado

---

**🃏 Flashcard 17**

**Q:** Qual a principal desvantagem do EFS?  
**A:** Custo elevado comparado ao EBS (até 3x mais caro que EBS GP2).

---

**🃏 Flashcard 18**

**Q:** O que é o One Zone EFS?  
**A:** É uma versão do EFS em apenas uma zona de disponibilidade, com custo menor e menor disponibilidade – ideal para ambientes de desenvolvimento e testes.

---

**🃏 Flashcard 19**

**Q:** É possível ativar backup automático no EFS?  
**A:** Sim, e é recomendável ativar. Também é possível usar o AWS Backup para gerenciamento centralizado.

### **Flashcards: Amazon Backup**

---

**🃏 Flashcard 1**  

**Q:** O que é o AWS Backup?  
**A:** Um serviço gerenciado da AWS que centraliza e automatiza backups de diversos serviços.

---

**🃏 Flashcard 2**  

**Q:** Qual a vantagem de usar o AWS Backup em vez de scripts manuais?  
**A:** Centralização, automação, e visibilidade unificada da estratégia de backup.

---

**🃏 Flashcard 3**  

**Q:** Quais são os três métodos para criar um plano de backup?  
**A:** A partir de um modelo, manualmente no console, ou via JSON.

---

**🃏 Flashcard 4**  

**Q:** Cite três serviços compatíveis com o AWS Backup.  
**A:** EC2, EBS, RDS (também: Aurora, DynamoDB, EFS, S3, etc.).

---

**🃏 Flashcard 5**  

**Q:** O AWS Backup é compatível com backups de bancos de dados?  
**A:** Sim, incluindo RDS, Aurora, DynamoDB, DocumentDB e Neptune.

---

**🃏 Flashcard 6**  

**Q:** O que é um Plano de Backup?  
**A:** Conjunto de regras que define frequência, janela, retenção e transição para cold storage.

---

**🃏 Flashcard 7**  

**Q:** Qual a finalidade da Janela de Backup?  
**A:** Define quando os backups devem iniciar.

---

**🃏 Flashcard 8**  

**Q:** O que significa Cold Storage no contexto do AWS Backup?  
**A:** Armazenamento de baixo custo, com maior latência de acesso, usado após tempo configurado.

---

**🃏 Flashcard 9**  

**Q:** Para que servem as Tags em um plano de backup?  
**A:** Automatizar o backup apenas de recursos com tags específicas, como `Environment=Production`.

---

**🃏 Flashcard 10**  

**Q:** O que é um Assignment (atribuição) no AWS Backup?  
**A:** Mapeamento de recursos a um plano de backup, por tipo de recurso ou por tag.

---

**🃏 Flashcard 11**  

**Q:** O que é o Vault Lock (WORM)?  
**A:** Proteção contra exclusão: Write Once Read Many – até o root não consegue deletar backups.

---

**🃏 Flashcard 12**  

**Q:** Qual a principal vantagem do Vault Lock?  
**A:** Garante integridade dos backups mesmo contra exclusões acidentais ou maliciosas.

---

**🃏 Flashcard 13**  

**Q:** O que é Cross-**Region** Backup?  
**A:** Replicação automática de backups entre regiões da AWS para recuperação de desastres.

---

**🃏 Flashcard 14**  

**Q:** O que é Cross-**Account** Backup?  
**A:** Permite replicar backups entre diferentes contas AWS.

---

**🃏 Flashcard 15**  

**Q:** O AWS Backup suporta recuperação pontual? Para quais serviços?  
**A:** Sim, por exemplo, para Amazon Aurora.

---

**🃏 Flashcard 16**  

**Q:** Quais fatores influenciam o custo do AWS Backup?  
**A:** Armazenamento ativo, cold storage, transferências entre regiões e execuções sob demanda.

---

**🃏 Flashcard 17**  

**Q:** Como monitorar os jobs de backup?  
**A:** Via CloudWatch, CloudTrail e visualização dos Jobs no console do AWS Backup.

---

**🃏 Flashcard 18**  

**Q:** É possível agendar backups no AWS Backup?  
**A:** Sim, usando frequência (diária, semanal etc.) ou expressão cron.

---

**🃏 Flashcard 19**  

**Q:** Como funciona a retenção de backups?  
**A:** Pode ser definida em dias, semanas, meses ou anos.