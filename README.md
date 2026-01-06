# 📋 Infraestrutura Provisionada - Terraform Migration

**Data de Criação:** 04/01/2026  
**Subscription:** Azure subscription 1 (6dd1ec24-1445-4b8a-a0c6-2fdc0f6f8964)  
**Estado:** ✅ Aplicado com Sucesso  
**Total de Recursos:** 81 recursos gerenciados pelo Terraform

---

## 📊 Visão Geral

Este documento descreve toda a infraestrutura Azure provisionada via Terraform na conta de destino, refletindo a estrutura do inventário original. A infraestrutura foi criada em duas regiões principais: **Brazil South** e **East US 2**.

```mermaid
graph TB
    subgraph "Azure Subscription"
        subgraph "Brazil South"
            BR_RG[Resource Groups BR]
            BR_VNET[Virtual Networks]
            BR_VM[VMs]
            BR_AKS[AKS Cluster]
            BR_STORAGE[Storage Accounts]
            BR_SB[Service Bus]
            BR_FUNC[Function Apps]
        end
        
        subgraph "East US 2"
            US_RG[Resource Groups US]
            US_VNET[Virtual Networks]
            US_VM[VMs]
            US_AKS[AKS Cluster]
            US_STORAGE[Storage Accounts]
            US_SQL[SQL Server]
        end
    end
    
    BR_RG --> BR_VNET
    BR_VNET --> BR_VM
    BR_VNET --> BR_AKS
    BR_RG --> BR_STORAGE
    BR_RG --> BR_SB
    BR_RG --> BR_FUNC
    
    US_RG --> US_VNET
    US_VNET --> US_VM
    US_VNET --> US_AKS
    US_RG --> US_STORAGE
    US_RG --> US_SQL
```

---

## 🗂️ Resource Groups

### Brazil South

| Resource Group | Localização | Propósito |
|----------------|-------------|-----------|
| **RG-VIAVAREJO-BR** | brazilsouth | Infraestrutura Via Varejo Brasil |
| **RG-AKS-BR** | brazilsouth | Cluster AKS Brasil |
| **RG-MESSAGERIA** | brazilsouth | Service Bus e mensageria |
| **RG-Alertas** | brazilsouth | Alertas e monitoramento |
| **RG-BRASIL** | brazilsouth | Recursos gerais Brasil |

### East US 2

| Resource Group | Localização | Propósito |
|----------------|-------------|-----------|
| **RGDATABASES** | eastus2 | SQL Server e databases |
| **RGWEB** | eastus2 | Aplicações web e VMs web |
| **RGSTORAGE** | eastus2 | Storage accounts |
| **RGBACKUP** | eastus2 | Backup e storage de backup |
| **RGNETWORK** | eastus2 | Recursos de rede |
| **RG-AKS-US** | eastus2 | Cluster AKS Estados Unidos |
| **FUSION-WCF_group** | eastus2 | VM Fusion WCF |
| **TRUX-RG** | eastus2 | Infraestrutura TRUX |

### Outras Regiões

| Resource Group | Localização | Propósito |
|----------------|-------------|-----------|
| **RGSTORAGE-EASTUS** | eastus | Storage accounts East US |
| **RGWEB-WESTUS2** | westus2 | Service Bus West US 2 |

---

## 🌐 Virtual Networks e Subnets

### Diagrama de Rede - Brazil South

```mermaid
graph TB
    subgraph "Brazil South - RG-VIAVAREJO-BR"
        VNET_VIA[RG-VIAVAREJO-BR-vnet<br/>10.1.0.0/24]
        SUBNET_VIA[snet-web<br/>10.1.0.0/28]
        VNET_VIA --> SUBNET_VIA
        SUBNET_VIA --> VM_SRVVIA[SRVVIA001]
    end
    
    subgraph "Brazil South - RG-AKS-BR"
        VNET_AKS_BR[RG-AKS-BR-vnet<br/>10.0.0.0/8]
        SUBNET_AKS_BR[snet-web<br/>10.10.0.0/16]
        VNET_AKS_BR --> SUBNET_AKS_BR
        SUBNET_AKS_BR --> AKS_BR[AKS-UX-BR]
    end
    
    subgraph "Brazil South - RG-MESSAGERIA"
        VNET_MSG[rg-messageria-vnet<br/>10.2.0.0/24]
        SUBNET_MSG[snet-web<br/>10.2.0.0/28]
        VNET_MSG --> SUBNET_MSG
    end
```

### Diagrama de Rede - East US 2

```mermaid
graph TB
    subgraph "East US 2 - RGDATABASES"
        VNET_DB[AZVNETUS<br/>10.0.0.0/16]
        SUBNET_DB[subnet-db<br/>10.0.1.0/24]
        VNET_DB --> SUBNET_DB
        SUBNET_DB --> SQL_SERVER[SQL Server]
    end
    
    subgraph "East US 2 - RGWEB"
        VNET_WEB[rgweb-vnet<br/>10.5.0.0/16]
        SUBNET_WEB[snet-web<br/>10.5.10.0/28]
        VNET_WEB --> SUBNET_WEB
        SUBNET_WEB --> VM_AZWEB[AZWEB01]
    end
    
    subgraph "East US 2 - RG-AKS-US"
        VNET_AKS_US[RG-AKS-US-vnet<br/>10.224.0.0/12]
        SUBNET_AKS_US[snet-web<br/>10.240.0.0/16]
        VNET_AKS_US --> SUBNET_AKS_US
        SUBNET_AKS_US --> AKS_US[AKS-UX-US]
    end
    
    subgraph "East US 2 - FUSION-WCF_group"
        VNET_FUSION[FUSION-WCF-vnet<br/>172.17.0.0/16]
        SUBNET_FUSION[snet-web<br/>172.17.10.0/24]
        VNET_FUSION --> SUBNET_FUSION
        SUBNET_FUSION --> VM_FUSION[FUSION-WCF]
    end
    
    subgraph "East US 2 - TRUX-RG"
        VNET_TRUX[TRUX-RG-vnet<br/>172.16.0.0/16]
        SUBNET_TRUX[snet-web<br/>172.16.10.0/24]
        VNET_TRUX --> SUBNET_TRUX
        SUBNET_TRUX --> VM_ROUTER[ROUTER-UX-01]
    end
```

---

## 💻 Virtual Machines

### VMs Provisionadas

```mermaid
graph LR
    subgraph "Brazil South"
        VM1[SRVVIA001<br/>Windows<br/>Standard_B4ms<br/>10.1.0.4]
    end
    
    subgraph "East US 2"
        VM2[AZWEB01<br/>Windows<br/>Standard_B2s<br/>10.5.10.4]
        VM3[FUSION-WCF<br/>Windows<br/>Standard_D2s_v3<br/>172.17.10.4]
        VM4[ROUTER-UX-01<br/>Linux<br/>Standard_DS1_v2<br/>172.16.10.4]
    end
    
    VM1 --> NSG1[SRVVIA001-nsg]
    VM2 --> NSG2[Sem NSG]
    VM3 --> NSG3[FUSION-WCF-nsg]
    VM4 --> NSG4[ROUTER-UX-01-nsg]
```

| VM | Resource Group | Localização | Size | OS | IP Privado | Status |
|----|----------------|-------------|------|----|-----------|--------|
| **SRVVIA001** | RG-VIAVAREJO-BR | brazilsouth | Standard_B4ms | Windows | 10.1.0.4 | ✅ Criada |
| **AZWEB01** | RGWEB | eastus2 | Standard_B2s | Windows | 10.5.10.4 | ✅ Criada |
| **FUSION-WCF** | FUSION-WCF_group | eastus2 | Standard_D2s_v3 | Windows | 172.17.10.4 | ✅ Criada |
| **ROUTER-UX-01** | TRUX-RG | eastus2 | Standard_DS1_v2 | Linux | 172.16.10.4 | ✅ Criada |
| **AZDB01** | RGDATABASES | eastus2 | Standard_D2s_v3 | Windows | - | ⚠️ Comentada (quota) |

**Nota:** A VM AZDB01 foi comentada temporariamente devido à limitação de quota de cores (10 cores disponíveis, 9 em uso).

---

## ☸️ AKS Clusters

### Clusters Kubernetes Provisionados

```mermaid
graph TB
    subgraph "AKS-UX-BR"
        AKS_BR[AKS Cluster BR<br/>brazilsouth]
        AKS_BR --> NODES_BR[Node Pool<br/>Standard_D2s_v3]
        AKS_BR --> VNET_BR[RG-AKS-BR-vnet]
        AKS_BR --> LAW_BR[Log Analytics<br/>workspacergaksbra93e]
    end
    
    subgraph "AKS-UX-US"
        AKS_US[AKS Cluster US<br/>eastus2]
        AKS_US --> NODES_US[Node Pool<br/>Standard_D2s_v3]
        AKS_US --> VNET_US[RG-AKS-US-vnet]
        AKS_US --> LAW_US[Log Analytics<br/>DefaultWorkspace-EUS2]
    end
```

| Cluster | Resource Group | Localização | FQDN | Node Pool | Status |
|---------|----------------|-------------|------|-----------|--------|
| **AKS-UX-BR** | RG-AKS-BR | brazilsouth | aks-ux-br-04xusv4z.hcp.brazilsouth.azmk8s.io | Standard_D2s_v3 (1 node) | ✅ Criado |
| **AKS-UX-US** | RG-AKS-US | eastus2 | aks-ux-us-pjsrjz90.hcp.eastus2.azmk8s.io | Standard_D2s_v3 (1 node) | ✅ Criado |

---

## 🗄️ SQL Server e Databases

### SQL Server Provisionado

```mermaid
graph TB
    SQL[SQL Server<br/>sql-server-rgdatabases<br/>eastus2]
    SQL --> DB1[Db_Fusion<br/>Basic SKU]
    SQL --> DB2[Db_Fusion_Hml<br/>Basic SKU]
    SQL --> DB3[Db_Ondetah<br/>Basic SKU]
    SQL --> VNET[AZVNETUS<br/>subnet-db]
```

| Recurso | Resource Group | Localização | Detalhes |
|---------|----------------|-------------|----------|
| **SQL Server** | RGDATABASES | eastus2 | Nome: `sql-server-rgdatabases` |
| **Db_Fusion** | RGDATABASES | eastus2 | SKU: Basic, Max Size: 2GB |
| **Db_Fusion_Hml** | RGDATABASES | eastus2 | SKU: Basic, Max Size: 2GB |
| **Db_Ondetah** | RGDATABASES | eastus2 | SKU: Basic, Max Size: 2GB |

---

## 💾 Storage Accounts

### Storage Accounts Criados

```mermaid
pie title Distribuição de Storage Accounts por Região
    "Brazil South" : 3
    "East US 2" : 5
    "East US" : 1
```

#### Brazil South

| Storage Account | Resource Group | Localização | Kind | Status |
|-----------------|----------------|-------------|------|--------|
| **menufrrgfo6ld53b07f1** | RG-VIAVAREJO-BR | brazilsouth | StorageV2 | ✅ Criado |
| **rgaksbr84b5d53b07f1** | RG-AKS-BR | brazilsouth | Storage | ✅ Criado |
| **rgviadiagd53b07f1** | RG-VIAVAREJO-BR | brazilsouth | Storage | ✅ Criado |

#### East US 2

| Storage Account | Resource Group | Localização | Kind | Status |
|-----------------|----------------|-------------|------|--------|
| **reenvioocorrenciasfuncti** | RG-AKS-US | eastus2 | StorageV2 | ✅ Criado (Function App) |
| **uxcarbonstg** | RG-AKS-BR | brazilsouth | StorageV2 | ✅ Criado (Function App) |

**Nota:** Alguns storage accounts do inventário original já existiam na conta e foram preservados (não recriados):
- `dbfusionbck`
- `csfaturaazure`
- `storagefusion`
- `truxstorageaccount`
- `rgdatabasesdiag745`
- `rgwebdiag964`
- `rgwebperfdiag438`
- `sqlvaez2w5q6adbsrk`

---

## 📨 Service Bus Namespaces

### Service Bus Provisionados

```mermaid
graph LR
    SB1[uxgroup-d53b07f1<br/>brazilsouth<br/>Standard]
    SB2[uxsolutions-d53b07f1<br/>westus2<br/>Standard]
    
    SB1 --> RG1[RG-MESSAGERIA]
    SB2 --> RG2[RGWEB-WESTUS2]
```

| Namespace | Resource Group | Localização | SKU | Status |
|-----------|----------------|-------------|-----|--------|
| **uxgroup-d53b07f1** | RG-MESSAGERIA | brazilsouth | Standard | ✅ Criado |
| **uxsolutions-d53b07f1** | RGWEB-WESTUS2 | westus2 | Standard | ✅ Criado |

**Nota:** Os nomes originais (`uxgroup` e `uxsolutions`) já estavam em uso, então foram adicionados sufixos únicos baseados no tenant ID.

---

## ⚡ Function Apps

### Function Apps Provisionadas

```mermaid
graph TB
    FUNC1[uxcarbon-d53b07f1<br/>brazilsouth<br/>Linux]
    FUNC1 --> ASP1[App Service Plan<br/>Y1 Consumption]
    FUNC1 --> STG1[Storage Account<br/>uxcarbonstg]
    
    FUNC2[ReenvioOcorrenciasFunction2<br/>eastus2<br/>Comentada]
    FUNC2 -.-> QUOTA[Quota Insuficiente]
```

| Function App | Resource Group | Localização | Runtime | Status |
|--------------|----------------|-------------|---------|--------|
| **uxcarbon-d53b07f1** | RG-AKS-BR | brazilsouth | Linux (.NET 8.0) | ✅ Criada |
| **ReenvioOcorrenciasFunction2** | RG-AKS-US | eastus2 | - | ⚠️ Comentada (quota) |

**Nota:** A Function App `ReenvioOcorrenciasFunction2` foi comentada devido a limitações de quota de Dynamic VMs na conta.

---

## 🔐 Network Security Groups (NSGs)

### NSGs Criados

```mermaid
graph TB
    subgraph "Brazil South"
        NSG1[SRVVIA001-nsg<br/>RG-VIAVAREJO-BR]
        NSG1 --> RULE1[RDP: 3389]
    end
    
    subgraph "East US 2"
        NSG2[AZDB01-nsg<br/>RGDATABASES]
        NSG2 --> RULE2[RDP: 3389]
        NSG2 --> RULE3[SQL: 1433]
        
        NSG3[FUSION-WCF-nsg<br/>FUSION-WCF_group]
        NSG3 --> RULE4[RDP: 3389]
        
        NSG4[ROUTER-UX-01-nsg<br/>TRUX-RG]
        NSG4 --> RULE5[SSH: 22]
    end
```

| NSG | Resource Group | Localização | Regras | Associado a |
|-----|----------------|-------------|--------|-------------|
| **SRVVIA001-nsg** | RG-VIAVAREJO-BR | brazilsouth | RDP (3389) | SRVVIA001 |
| **AZDB01-nsg** | RGDATABASES | eastus2 | RDP (3389), SQL (1433) | AZDB01 |
| **FUSION-WCF-nsg** | FUSION-WCF_group | eastus2 | RDP (3389) | FUSION-WCF |
| **ROUTER-UX-01-nsg** | TRUX-RG | eastus2 | SSH (22) | ROUTER-UX-01 |

---

## 📊 Log Analytics Workspaces

### Workspaces Criados

```mermaid
graph TB
    LAW1[DefaultWorkspace-d53b07f1-EUS2<br/>RGWEB<br/>eastus2]
    LAW2[workspacergaksbra93e<br/>RG-AKS-BR<br/>brazilsouth]
    
    LAW1 --> INSIGHTS1[Container Insights]
    LAW2 --> INSIGHTS2[AKS Monitoring]
```

| Workspace | Resource Group | Localização | Propósito |
|-----------|----------------|-------------|-----------|
| **DefaultWorkspace-d53b07f1-EUS2** | RGWEB | eastus2 | Monitoramento geral East US 2 |
| **workspacergaksbra93e** | RG-AKS-BR | brazilsouth | Monitoramento AKS Brasil |

---

## 🌐 Public IPs

### Public IPs Criados

| Public IP | Resource Group | Localização | Allocation | SKU | Associado a |
|-----------|----------------|-------------|------------|-----|-------------|
| **PIP-SRVVIA001** | RG-VIAVAREJO-BR | brazilsouth | Static | Standard | SRVVIA001 |
| **AZDB01-ip** | RGDATABASES | eastus2 | Static | Standard | AZDB01 |
| **AZWEB01** | RGWEB | eastus2 | Static | Standard | AZWEB01 |
| **FUSION-WCF-ip** | FUSION-WCF_group | eastus2 | Static | Standard | FUSION-WCF |
| **ROUTER-UX-01-ip** | TRUX-RG | eastus2 | Static | Standard | ROUTER-UX-01 |

---

## 📈 Arquitetura Completa

### Diagrama de Arquitetura Geral

```mermaid
graph TB
    subgraph "Brazil South Region"
        subgraph "RG-VIAVAREJO-BR"
            VNET1[RG-VIAVAREJO-BR-vnet]
            VM1[SRVVIA001]
            NSG1[SRVVIA001-nsg]
            STG1[Storage Accounts]
            VNET1 --> VM1
            NSG1 --> VM1
        end
        
        subgraph "RG-AKS-BR"
            VNET2[RG-AKS-BR-vnet]
            AKS1[AKS-UX-BR]
            FUNC1[uxcarbon Function App]
            LAW1[Log Analytics]
            VNET2 --> AKS1
            AKS1 --> LAW1
        end
        
        subgraph "RG-MESSAGERIA"
            SB1[Service Bus uxgroup]
        end
    end
    
    subgraph "East US 2 Region"
        subgraph "RGDATABASES"
            VNET3[AZVNETUS]
            SQL[SQL Server]
            DB1[Databases]
            VNET3 --> SQL
            SQL --> DB1
        end
        
        subgraph "RGWEB"
            VNET4[rgweb-vnet]
            VM2[AZWEB01]
            LAW2[Log Analytics]
            VNET4 --> VM2
        end
        
        subgraph "RG-AKS-US"
            VNET5[RG-AKS-US-vnet]
            AKS2[AKS-UX-US]
            VNET5 --> AKS2
        end
        
        subgraph "FUSION-WCF_group"
            VNET6[FUSION-WCF-vnet]
            VM3[FUSION-WCF]
            NSG2[FUSION-WCF-nsg]
            VNET6 --> VM3
            NSG2 --> VM3
        end
        
        subgraph "TRUX-RG"
            VNET7[TRUX-RG-vnet]
            VM4[ROUTER-UX-01]
            NSG3[ROUTER-UX-01-nsg]
            VNET7 --> VM4
            NSG3 --> VM4
        end
    end
    
    subgraph "West US 2 Region"
        SB2[Service Bus uxsolutions]
    end
```

---

## 📋 Resumo de Recursos por Tipo

### Distribuição de Recursos

```mermaid
pie title Distribuição de Recursos por Tipo
    "Resource Groups" : 14
    "Virtual Networks" : 12
    "Subnets" : 12
    "Virtual Machines" : 4
    "AKS Clusters" : 2
    "Storage Accounts" : 5
    "Service Bus" : 2
    "Function Apps" : 1
    "SQL Server" : 1
    "SQL Databases" : 3
    "NSGs" : 4
    "Public IPs" : 5
    "Log Analytics" : 2
    "Availability Sets" : 1
```

### Contagem Detalhada

| Tipo de Recurso | Quantidade | Status |
|-----------------|------------|--------|
| Resource Groups | 14 | ✅ Todos criados |
| Virtual Networks | 12 | ✅ Todos criados |
| Subnets | 12 | ✅ Todas criadas |
| Virtual Machines | 4 | ✅ 4 criadas, 1 comentada (quota) |
| AKS Clusters | 2 | ✅ Ambos criados |
| Storage Accounts | 5 | ✅ 5 criados, 8 preservados |
| Service Bus Namespaces | 2 | ✅ Ambos criados |
| Function Apps | 1 | ✅ 1 criada, 1 comentada (quota) |
| SQL Server | 1 | ✅ Criado |
| SQL Databases | 3 | ✅ Todos criados |
| Network Security Groups | 4 | ✅ Todos criados |
| Public IPs | 5 | ✅ Todos criados |
| Log Analytics Workspaces | 2 | ✅ Ambos criados |
| Availability Sets | 1 | ✅ Criado |

**Total:** 81 recursos gerenciados pelo Terraform

---

## ⚠️ Limitações e Ajustes

### Recursos Comentados/Temporariamente Indisponíveis

1. **VM AZDB01**
   - **Motivo:** Quota de cores excedida (10 cores disponíveis, 9 em uso, necessário 2 adicionais)
   - **Solução:** Solicitar aumento de quota ou reduzir tamanho de outras VMs

2. **Function App ReenvioOcorrenciasFunction2**
   - **Motivo:** Limitação de quota de Dynamic VMs (0 disponível)
   - **Solução:** Solicitar aumento de quota ou usar App Service Plan dedicado

3. **Storage Accounts Existentes**
   - **Motivo:** Alguns storage accounts já existiam na conta
   - **Solução:** Preservados, não recriados para evitar conflitos

### Ajustes de Nomenclatura

- **Service Bus:** Adicionados sufixos únicos (`-d53b07f1`) para evitar conflitos
- **Function Apps:** Adicionados sufixos únicos para evitar conflitos
- **Storage Accounts:** Alguns receberam sufixos únicos baseados no tenant ID

### Ajustes de SKU

- **VM AZWEB01:** Reduzida de `Standard_B4ms` para `Standard_B2s` (quota)
- **VM FUSION-WCF:** Reduzida de `Standard_D4s_v3` para `Standard_D2s_v3` (quota)

---

## 🔗 Conectividade e Endpoints

### Endpoints Públicos

| Recurso | Endpoint | Tipo |
|---------|----------|------|
| **AKS-UX-BR** | `aks-ux-br-04xusv4z.hcp.brazilsouth.azmk8s.io` | Kubernetes API |
| **AKS-UX-US** | `aks-ux-us-pjsrjz90.hcp.eastus2.azmk8s.io` | Kubernetes API |
| **SRVVIA001** | Public IP estático | RDP (3389) |
| **AZWEB01** | Public IP estático | RDP (3389) |
| **FUSION-WCF** | Public IP estático | RDP (3389) |
| **ROUTER-UX-01** | Public IP estático | SSH (22) |

### Conectividade Interna

- Todas as VMs estão em VNets privadas com subnets dedicadas
- AKS clusters têm integração com VNets (network plugin: azure)
- SQL Server está em subnet dedicada (`subnet-db`)

---

## 📝 Próximos Passos

### Recomendações

1. **Aumentar Quota de Cores**
   - Solicitar aumento para pelo menos 15 cores em East US 2
   - Isso permitirá criar a VM AZDB01

2. **Aumentar Quota de Dynamic VMs**
   - Solicitar quota para Function Apps se necessário
   - Ou migrar para App Service Plan dedicado

3. **Configurar Peering de VNets**
   - Se necessário comunicação entre VNets diferentes
   - Configurar peering entre VNets da mesma região

4. **Configurar Load Balancers**
   - Adicionar Load Balancers se necessário para alta disponibilidade
   - Configurar regras de balanceamento para VMs

5. **Backup e Disaster Recovery**
   - Configurar Azure Backup para VMs
   - Configurar backup automático para SQL Databases

6. **Monitoramento e Alertas**
   - Configurar alertas adicionais nos Log Analytics Workspaces
   - Configurar dashboards no Azure Monitor

---
