# 📋 Plano de Migração DevOps

**Organização Origem:** `https://dev.azure.com/uxsolutions/` / `Projetos`  
**Organização Destino:** `https://dev.azure.com/ux-solutions/` / `life-cycle`  
**Última atualização:** 27-01-2026 às 19:19:29

---

## 📊 Status das Fases

| Fase | Descrição | Status | Progresso |
|------|-----------|--------|-----------|
| **FASE 0** | Preparação e Análise | ✅ **CONCLUÍDA** | 100% |
| **FASE 1** | Migração de Repositórios | ⚠️ **PARCIAL** | 50% (criados, conteúdo pendente) |
| **FASE 2** | Service Connections | ✅ **CONCLUÍDA** | 100% (7/8 criadas) |
| **FASE 3** | Variable Groups | ✅ **CONCLUÍDA** | 100% (nenhuma encontrada) |
| **FASE 4** | Migração de Pipelines | ✅ **CONCLUÍDA** | 100% (17/17 criados) |
| **FASE 5** | Validação Final | ⏳ **PENDENTE** | 0% |

**Progresso Geral:** ~70% concluído

---

## 🎯 O Que Falta Fazer

### 1. **FASE 1: Completar Migração de Conteúdo dos Repositórios** ⚠️ CRÍTICO

**Status:** 17 repositórios criados, mas conteúdo (branches, tags, commits) não migrado.

**Ação:**
```bash
cd scripts
export AUTO_OVERWRITE='s'
export TARGET_ORG_URL='https://dev.azure.com/ux-solutions/'
export TARGET_PROJECT='life-cycle'
./migrate-repositories.sh
```

**Localização dos repositórios:** `https://dev.azure.com/ux-solutions/life-cycle/_git/{NomeRepo}`

---

### 2. **FASE 2: Criar Service Connections** ✅ CONCLUÍDA

**Status:** ✅ **7/8 Service Connections criadas com sucesso!**

**Resultado:**
- ✅ 7 Docker Registry Connections criadas → UXREGISTRY2026
- ⚠️ 1 Azure Subscription Connection pendente (requer Service Principal manual)

**Service Connections Criadas:**
1. ✅ UXREGISTRY_102025 (ID: 999f1dc1-06cf-4dec-98d6-e65de10b72aa)
2. ✅ UXREGISTRY_112025 (ID: 6ddf73f2-336f-45fc-962d-15d7eee83666)
3. ✅ UXREGISTRY_022025 (ID: 94e28ab8-04db-492f-9b8f-c19d8b002685)
4. ✅ UXREGISTRY_052024 (ID: 1e3791c1-923b-47c7-8b91-ef2209e23b29)
5. ✅ UXREGISTRY_082024 (ID: 61d814ae-651f-41c7-9c96-e59a7f485fb8)
6. ✅ 6bfe592c-5582-4125-88a3-9135264bd8a1 (ID: 13c3de8d-fe66-4cf8-b1a3-3e1624d39010)
7. ✅ d62e4ce8-724c-49c1-bbe9-100f4b7b2bcd (ID: 33b8ca92-c304-488c-a367-d72303b12091)

**Documentação:** Todas as credenciais criadas estão documentadas em `service-connections-created.md`

**Recursos do Destino:**
- Container Registry: `UXREGISTRY2026` (uxregistry2026.azurecr.io)
- Subscription: `Microsoft Azure - UX` (b30da310-60fe-4d2b-9ac0-ec4ce87df6a3)

---

### 3. **FASE 4: Migrar Pipelines** ✅ CONCLUÍDA

**Status:** ✅ **17/17 pipelines criados com sucesso!**

**Resultado:**
- Todos os 17 pipelines foram criados na conta destino
- Localização: `https://dev.azure.com/ux-solutions/life-cycle`
- Pipelines: 8-fretter-ci, 9-ondetah-ci, 17-fusion-fila-ci, 18-fusion-ci, 19-tms-logistica-ci, 20-tms-sorterapi-ci, 22-uxdelivery-api-lastmile, 23-uxdelivery-app-lastmile, 24-truxdiscovery, 25-tms-rastreiomotorista-ci, 27-trux-mobile-ci, 28-ondetah-bot, 29-uxcarbon-ci, 30-uxframework-ci, 32-uxtracking-ci, 33-fusionenvvias-ci, 36-uxsignon

**Observação:** Pipelines criados, mas precisam de Service Connections para funcionar completamente.

---

### 4. **FASE 5: Validação Final**

- Verificar conteúdo dos repositórios (branches, tags, commits)
- Testar execução de pipelines
- Validar Service Connections
- Documentar diferenças e problemas encontrados

---

## 📊 Resumo Executivo

| Fase | Status Atual | Ação Necessária | Prioridade | Tempo Estimado |
|------|--------------|-----------------|------------|----------------|
| **FASE 1** | 50% | Migrar conteúdo dos repositórios | 🔴 **CRÍTICA** | 30-60 min |
| **FASE 2** | 87.5% | Criar Azure Subscription Connection | 🟡 **MÉDIA** | 10-15 min |
| **FASE 5** | 0% | Validação completa | 🟢 **APÓS FASE 1** | 1-2 horas |

**Progresso Atual:** ~70% concluído  
**Progresso Após FASE 1:** ~85% concluído  
**Progresso Após FASE 2:** ~95% concluído  
**Progresso Final (com validação):** 100% concluído

---

## 🔄 Ordem Recomendada de Execução

### 1. **FASE 1: Migração de Conteúdo dos Repositórios** 🔴 CRÍTICO

**Por que primeiro:** É o bloqueador principal. Sem conteúdo nos repositórios, os pipelines não podem funcionar.

**Como executar:**
```bash
cd scripts
export AUTO_OVERWRITE='s'
export TARGET_ORG_URL='https://dev.azure.com/ux-solutions/'
export TARGET_PROJECT='life-cycle'
./migrate-repositories.sh
```

**O que faz:**
- Clona cada repositório da origem usando `git clone --mirror`
- Faz push de todas as branches e tags para o destino
- Usa `AUTO_OVERWRITE='s'` para sobrescrever repositórios vazios existentes

**Tempo estimado:** 30-60 minutos (depende do tamanho dos repositórios)

---

### 2. **FASE 2: Completar Service Connections** 🟡 RECOMENDADO

**Por que importante:** Alguns pipelines podem falhar sem a conexão Azure Resource Manager.

**Como executar (manual):**

1. **Criar Service Principal no Azure:**
   ```bash
   az ad sp create-for-rbac --name "AzureDevOps-ServiceConnection" \
     --role contributor \
     --scopes /subscriptions/b30da310-60fe-4d2b-9ac0-ec4ce87df6a3
   ```

2. **Criar Service Connection no Azure DevOps:**
   - Portal: `https://dev.azure.com/ux-solutions/life-cycle/_settings/adminservices`
   - Tipo: Azure Resource Manager
   - Subscription: `Microsoft Azure - UX` (b30da310-60fe-4d2b-9ac0-ec4ce87df6a3)
   - Service Principal: usar as credenciais criadas acima

**Tempo estimado:** 10-15 minutos

---

### 3. **FASE 5: Validação Final** 🟢 APÓS FASE 1

**Por que necessário:** Garantir que tudo funciona antes de considerar a migração concluída.

**Como executar:**

#### 3.1. Verificar Conteúdo dos Repositórios
```bash
# Para cada repositório, verificar:
az repos show --repository {NomeRepo} --org https://dev.azure.com/ux-solutions/ --project life-cycle
az repos ref list --repository {NomeRepo} --org https://dev.azure.com/ux-solutions/ --project life-cycle
```

#### 3.2. Testar Execução de Pipelines
- Executar manualmente 1-2 pipelines de teste
- Verificar se as Service Connections estão sendo encontradas
- Validar se os builds/deploys funcionam

#### 3.3. Validar Service Connections
- Verificar se todas as 8 estão ativas
- Testar conexão com o Container Registry
- Testar conexão com a Subscription Azure

#### 3.4. Documentar Diferenças
- Criar documento com problemas encontrados
- Listar pipelines que precisam ajustes
- Documentar configurações adicionais necessárias

**Tempo estimado:** 1-2 horas

---

## ⚠️ Observações Importantes

### 1. **FASE 1 é Bloqueadora**
- **Sem conteúdo nos repositórios, os pipelines não funcionam**
- Esta é a ação mais crítica para alcançar 100%
- Deve ser executada antes de qualquer validação

### 2. **FASE 2 Pode Ser Feita Depois**
- A Service Connection Azure RM é importante, mas não bloqueia completamente
- Alguns pipelines podem falhar sem ela, mas outros funcionarão
- Pode ser criada manualmente quando necessário

### 3. **FASE 5 Deve Ser Feita Após FASE 1**
- Não faz sentido validar pipelines sem código nos repositórios
- A validação completa só é possível após migração do conteúdo
- Use esta fase para identificar problemas antes do uso em produção

### 4. **Dependências Entre Fases**
- **Pipelines** dependem de **Service Connections** e **repositórios com conteúdo**
- Execute na ordem: FASE 1 → FASE 2 → FASE 5
- FASE 0, 3 e 4 já estão concluídas

### 5. **Limpeza**
- Use `scripts/cleanup-devops.sh` com cuidado - apaga tudo sem confirmação fácil
- Não execute limpeza até ter certeza de que a migração está completa

---

## ✅ Conclusão

Para alcançar **100% de conclusão** da migração DevOps:

1. ✅ **Executar FASE 1** (migração de conteúdo dos repositórios) - **CRÍTICO**
   - Esta é a ação mais importante e bloqueadora
   - Sem ela, os pipelines não podem funcionar
   - Após esta fase, o progresso geral será ~85%

2. ✅ **Completar FASE 2** (Service Connection Azure RM) - **RECOMENDADO**
   - Necessária para pipelines que fazem deploy no Azure
   - Pode ser feita manualmente quando necessário
   - Após esta fase, o progresso geral será ~95%

3. ✅ **Executar FASE 5** (validação final) - **APÓS FASE 1**
   - Garante que tudo funciona antes de considerar concluído
   - Identifica problemas antes do uso em produção
   - Após esta fase, o progresso geral será **100%**

**Próximo passo imediato:** Executar a FASE 1 (migração de conteúdo dos repositórios).

---

## 📁 Estrutura do Projeto

```
pipelines/
├── README.md                    # Documentação principal
├── PLANO_MIGRACAO_DEVOPS.md    # Este arquivo
├── repositories.md              # Lista de repositórios da origem
├── service-connections-created.md  # Documentação de credenciais criadas
├── dependencies-*.txt           # Listas de dependências
├── scripts/                     # Scripts de migração
│   ├── execute-migration.sh    # Orquestrador principal
│   ├── migrate-repositories.sh
│   ├── migrate-service-connections.sh
│   ├── create-service-connections-destino.sh  # NOVO: Cria SCs no destino
│   ├── migrate-variable-groups.sh
│   ├── prepare-migration.sh
│   ├── cleanup-devops.sh
│   └── ...
├── logs/                        # Logs de execução
├── definitions/                 # Definições dos pipelines (17)
├── migration-prep/              # Scripts gerados para migração
└── service-connections-prep/   # Scripts antigos (legado)
```

---

## 🛠️ Scripts Disponíveis

### Scripts de Migração (`scripts/`)

- **`execute-migration.sh`** - Orquestra execução de todas as fases
- **`migrate-repositories.sh`** - Migra repositórios Git
- **`create-service-connections-destino.sh`** - ⭐ **NOVO**: Cria Service Connections no destino
- **`migrate-service-connections.sh`** - Prepara migração de Service Connections (legado)
- **`migrate-variable-groups.sh`** - Prepara migração de Variable Groups
- **`prepare-migration.sh`** - Prepara scripts de migração de pipelines

### Scripts de Limpeza (`scripts/`)

- **`cleanup-devops.sh`** - ⚠️ **APAGA** todo ambiente DevOps criado (use com cuidado!)

### Scripts de Extração (`scripts/`)

- **`extract-pipelines.sh`** - Extrai definições de pipelines
- **`download-repo-files.sh`** - Baixa arquivos importantes dos repositórios

---

## 📊 Inventário

- **Pipelines:** 17 (17/17 criados no destino)
- **Repositórios:** 17 (origem) / 18 (destino - inclui repositório padrão do projeto)
- **Service Connections:** 7/8 criadas no destino (7 Docker Registry, 1 Azure RM pendente)
- **Variable Groups:** 0 (nenhuma encontrada)

---


## 🔐 Nova Abordagem: Service Connections

### Objetivo
Criar Service Connections no destino apontando para recursos do próprio destino, não da origem.

### Recursos do Destino Identificados
- **Container Registry:** `UXREGISTRY2026` (RG-AKS)
- **Subscription:** `Microsoft Azure - UX` (b30da310-60fe-4d2b-9ac0-ec4ce87df6a3)
- **Storage Accounts:** Múltiplos (com sufixo -2026)

### Service Connections a Criar
1. **Docker Registry Connections** (8):
   - UXREGISTRY_102025 → UXREGISTRY2026
   - UXREGISTRY_112025 → UXREGISTRY2026
   - UXREGISTRY_022025 → UXREGISTRY2026
   - UXREGISTRY_052024 → UXREGISTRY2026
   - UXREGISTRY_082024 → UXREGISTRY2026
   - 6bfe592c-5582-4125-88a3-9135264bd8a1 → UXREGISTRY2026
   - d62e4ce8-724c-49c1-bbe9-100f4b7b2bcd → UXREGISTRY2026

2. **Azure Subscription Connection** (1):
   - Azure-Subscription → Subscription destino

### Documentação
Todas as credenciais criadas serão automaticamente documentadas em:
- `service-connections-created.md` - Lista completa com IDs, tipos, status

---

**Última atualização:** 27-01-2026 às 19:19:29
