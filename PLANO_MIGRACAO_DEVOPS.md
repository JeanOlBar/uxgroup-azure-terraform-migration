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

- **Pipelines:** 17
- **Repositórios:** 17 (origem) / 18 (destino - inclui repositório padrão do projeto)
- **Service Connections:** 8 identificadas (serão criadas no destino)
- **Variable Groups:** 0 (nenhuma encontrada)

---

## ⚠️ Observações Importantes

1. **Repositórios:** Foram criados no destino, mas conteúdo não foi migrado. Executar migração forçada.

2. **Service Connections:** 
   - ⭐ **NOVA ABORDAGEM**: Criar novas credenciais no destino
   - Apontar para recursos do destino (UXREGISTRY2026, Subscription destino)
   - Todas as credenciais serão documentadas automaticamente

3. **Pipelines:** Dependem de Service Connections e repositórios com conteúdo. Executar na ordem correta.

4. **Limpeza:** Use `scripts/cleanup-devops.sh` com cuidado - apaga tudo sem confirmação fácil.

---

## 🔄 Ordem de Execução Recomendada

1. ✅ FASE 0: Preparação - **CONCLUÍDA**
2. ⚠️ FASE 1: Migrar conteúdo dos repositórios - **PENDENTE** (repositórios criados, conteúdo não migrado)
3. ✅ FASE 2: Criar Service Connections - **CONCLUÍDA** (7/8 criadas)
4. ✅ FASE 3: Variable Groups - **CONCLUÍDA** (nenhuma encontrada)
5. ✅ FASE 4: Migrar pipelines - **CONCLUÍDA** (17/17 criados)
6. ⏳ FASE 5: Validação - **PENDENTE**

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
