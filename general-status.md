# 📊 Status Geral da Migração DevOps

**Última atualização:** 28/01/2026  
**Data de retomada:** 29/01/2026  
**Status atual:** ⚠️ Aguardando configuração de paralelismo

---

## 🎯 Objetivo Geral

Migrar **17 repositórios** e seus pipelines do Azure DevOps origem (`uxsolutions/Projetos`) para o destino (`ux-solutions/life-cycle`), atualizando domínios de `*.uxsolutions.com.br` e `*.ondetah.com.br` para `*.dynabit.com.br`.

---

## ✅ O Que Já Foi Feito

### 1. Cluster OndeTah - Migração de Código (COMPLETO)

**Repositório migrado:**
- ✅ `OndeTah` - Push realizado para `https://dev.azure.com/ux-solutions/life-cycle/_git/OndeTah`

**Domínios atualizados no código:**
- ✅ `portal.ondetah.com.br` → `ondetah-portal.dynabit.com.br`
- ✅ `cliente.ondetah.com.br` → `ondetah-cliente.dynabit.com.br`
- ✅ `ondetah.dev.uxsolutions.com.br` → `ondetah.dev.dynabit.com.br`
- ✅ `carbon.uxsolutions.com.br` → `carbon.dynabit.com.br`
- ✅ `tracking.dev.uxsolutions.com.br` → `tracking.dev.dynabit.com.br`
- ✅ `ondetah.com.br` → `ondetah.dynabit.com.br`

**Arquivos modificados:**
- Backend: `appsettings.json` (4 arquivos)
- Frontend: `environment.ts`, `environment.prod.ts`, `config.json` (6 arquivos)
- Mobile: `initPushNotification.ts`
- SQL: Procedures e scripts de migração (4 arquivos)
- Bot: `index.js`

**Regras aplicadas:**
- ✅ E-mails mantidos sem alteração (`suporte@OndeTah.com.br`, `contato@ondetah.com.br`, `sistema@ondetah.com.br`)
- ✅ Connection strings de banco de dados mantidas apontando para origem
- ✅ Apenas URLs HTTP/HTTPS foram alteradas

### 2. Pipeline OndeTah.CI - Configuração (PARCIAL)

**Pipeline criado:**
- ✅ Nome: `OndeTah.CI`
- ✅ ID: 18
- ✅ Projeto: `life-cycle`
- ✅ Repositório vinculado: `OndeTah`
- ✅ YAML: `azure-pipelines.yml` (branch `master`)

**Correções aplicadas:**
- ✅ `file-creator@6` substituído por script bash nativo (depois removido)
- ✅ GitVersion removido temporariamente após 5 tentativas de sintaxe falharem
- ✅ Usando `Build.BuildId` como tag em vez de GitVersion
- ✅ Pool configurado para usar `vmImage: ubuntu-latest`
- ✅ Código atualizado e push realizado (commit `abccb176`)

**Service Connection:**
- ✅ `UXREGISTRY_102025` configurada
- ✅ Aponta para `uxregistry2026.azurecr.io`

**Extensões:**
- ✅ GitTools (`gittools.gittools`) instalada na organização `ux-solutions`
- ⚠️ Tasks GitVersion não funcionam (problema não resolvido, removido temporariamente)

---

## ⚠️ Onde Estamos Agora

### Bloqueio Atual

**Problema:** Organização `ux-solutions` não tem paralelismo configurado para usar agentes hospedados da Microsoft.

**Erro:**
```
No hosted parallelism has been purchased or granted. 
To request a free parallelism grant, please fill out the following form 
https://aka.ms/azpipelines-parallelism-request
```

**Status do Pipeline:**
- ✅ Pipeline criado e configurado corretamente
- ✅ YAML corrigido e atualizado no repositório remoto
- ⚠️ **Não pode executar** até paralelismo ser configurado

### Ação Necessária (URGENTE)

**Solicitar paralelismo gratuito:**
1. Acesse: https://aka.ms/azpipelines-parallelism-request
2. Preencha formulário:
   - Organização: `ux-solutions`
   - Projeto: `life-cycle`
   - Justificativa: Migração de pipelines DevOps
3. Tempo de resposta: 2-3 dias úteis (pode variar)

**Alternativas:**
- Configurar agente auto-hospedado (não requer paralelismo)
- Comprar jobs paralelos (inclui trial gratuito)

---

## 📋 O Que Temos Que Fazer

### Fase 1: Resolver Bloqueio Atual (PRIORIDADE)

- [ ] Solicitar paralelismo gratuito via formulário
- [ ] Aguardar aprovação (2-3 dias úteis)
- [ ] OU configurar agente auto-hospedado
- [ ] Testar pipeline OndeTah.CI após paralelismo liberado

### Fase 2: Finalizar Cluster OndeTah

- [ ] Executar pipeline OndeTah.CI com sucesso
- [ ] Validar que imagens Docker foram buildadas e publicadas
- [ ] Migrar `OndeTah.Bot` (aplicar mudanças de domínios)
- [ ] Fazer push para `life-cycle/OndeTah.Bot`
- [ ] Criar/configurar pipeline `OndeTah.Bot.CI`

### Fase 3: Migrar Outros Clusters (16 repositórios restantes)

**Ordem de migração (por dependências):**
1. Foundations (bases)
2. SSO (autenticação)
3. OndeTah (em andamento - 1/2 completo)
4. Fusion
5. TMS
6. Trux/Last Mile
7. Analytics

**Processo para cada repositório:**
- [ ] Aplicar mudanças de domínios no código
- [ ] Fazer push para `life-cycle/[NOME_REPO]`
- [ ] Criar/configurar pipeline `[NOME].CI`
- [ ] Executar pipeline com sucesso

### Fase 4: Revisar GitVersion (Opcional - Depois)

- [ ] Investigar por que extensão GitTools não funciona
- [ ] Resolver problema com tasks GitVersion
- [ ] Adicionar GitVersion de volta aos pipelines
- [ ] Restaurar versionamento semântico

---

## 🔄 O Que Vamos Fazer Após Paralelismo Ser Liberado

### Passo 1: Testar Pipeline OndeTah.CI

1. Executar pipeline manualmente
2. Verificar logs em tempo real
3. Confirmar que todas as tasks Docker executam
4. Validar que imagens foram buildadas e publicadas no registry

**Comandos úteis:**
```bash
# Verificar pipeline
az pipelines show --id 18 --organization https://dev.azure.com/ux-solutions --project life-cycle

# Executar pipeline
az pipelines run --id 18 --organization https://dev.azure.com/ux-solutions --project life-cycle --branch master
```

### Passo 2: Validar Artefatos

1. Verificar imagens Docker no registry:
   - `uxregistry2026.azurecr.io/OndeTah.Api`
   - `uxregistry2026.azurecr.io/OndeTah.WebHook.Api`
   - `uxregistry2026.azurecr.io/OndeTah.Api.Mobile`
   - `uxregistry2026.azurecr.io/OndeTah.Service`
   - `uxregistry2026.azurecr.io/OndeTah.Web.Cliente`
   - `uxregistry2026.azurecr.io/OndeTah.Web.Portal`
   - `uxregistry2026.azurecr.io/Ondetah.Seller.Fusion`

2. Verificar artefatos publicados:
   - Database artifacts em `OndeTahArtifacts`

### Passo 3: Continuar Migração

1. Migrar `OndeTah.Bot`
2. Prosseguir com próximo cluster (Foundations ou SSO)
3. Aplicar mesmo processo: domínios → push → pipeline

---

## 📊 Progresso Geral

### Repositórios Migrados: 1/17 (6%)

- ✅ **OndeTah** - Código migrado, pipeline criado, aguardando paralelismo

### Repositórios Pendentes: 16/17 (94%)

- ⏳ OndeTah.Bot
- ⏳ Fusion
- ⏳ Fretter
- ⏳ Trux (vários repositórios)
- ⏳ UXCarbon
- ⏳ UxSignOn
- ⏳ TMS (vários repositórios)
- ⏳ Outros (ver `pipelines/repositories.md`)

### Pipelines Criados: 1/17 (6%)

- ✅ OndeTah.CI (ID 18) - Configurado, aguardando paralelismo

---

## 🚨 Regras Críticas de Migração

### 1. Connection Strings de Banco de Dados
- ❌ **NÃO alterar** connection strings durante migração de domínios
- ✅ Manter apontando para bancos originais
- ⚠️ Migração de banco é fase separada

### 2. E-mails
- ❌ **NÃO alterar** endereços de e-mail (`@ondetah.com.br`, `@uxsolutions.com.br`)
- ✅ Manter como estão

### 3. Migrações de Banco via Pipeline
- ❌ **NÃO executar** DbUp, SqlScripts/Migrate, ou qualquer comando de criação/alteração de estrutura de banco
- ✅ Pipelines devem apenas **buildar e publicar artefatos**
- ⚠️ Execução de migrações só após migração completa de banco

### 4. Ordem de Migração
Seguir ordem por dependências:
1. Foundations → 2. SSO → 3. OndeTah → 4. Fusion → 5. TMS → 6. Trux → 7. Analytics

---

## 📁 Estrutura do Projeto

```
azure-terraform-migration/
├── repositories/              # 17 repositórios clonados
│   ├── OndeTah/              # ✅ Migrado
│   ├── OndeTah.Bot/           # ⏳ Pendente
│   └── ...
├── pipelines/
│   ├── definitions/           # Definições de pipelines
│   │   └── 9-ondetah-ci/      # ✅ Pipeline OndeTah configurado
│   ├── docs/                  # Documentação
│   │   ├── HANDOFF_CONTINUIDADE.md
│   │   ├── ONDETAH_MIGRACAO_DOMINIOS.md
│   │   └── ...
│   └── repositories.md       # Lista completa dos 17 repositórios
└── general-status.md          # Este arquivo
```

---

## 🔗 Links Importantes

### Azure DevOps
- **Pipeline OndeTah.CI:** https://dev.azure.com/ux-solutions/life-cycle/_build?definitionId=18
- **Repositório OndeTah:** https://dev.azure.com/ux-solutions/life-cycle/_git/OndeTah
- **Projeto life-cycle:** https://dev.azure.com/ux-solutions/life-cycle
- **Extensões:** https://dev.azure.com/ux-solutions/_settings/extensions
- **Paralelismo:** https://dev.azure.com/ux-solutions/_settings/parallelism

### Formulários e Solicitações
- **Solicitar Paralelismo:** https://aka.ms/azpipelines-parallelism-request

### Documentação
- **Handoff Continuidade:** `pipelines/docs/HANDOFF_CONTINUIDADE.md`
- **Migração OndeTah:** `pipelines/docs/ONDETAH_MIGRACAO_DOMINIOS.md`
- **Lista de Repositórios:** `pipelines/repositories.md`

---

## 📝 Notas Importantes

1. **Tokens:** Estão em `repositories/.env`
   - `UX_GIT_TOKEN`: Token para repositórios origem (`uxsolutions/Projetos`)
   - `VENUXX_GIT_TOKEN`: Token para repositórios destino (`ux-solutions/life-cycle`)

2. **Remote padrão:** Usar nome `venux` para o remote do destino

3. **Branch padrão:** `master` (alguns podem ter `main`)

4. **Azure CLI:** Configurado para organização `ux-solutions` e projeto `life-cycle`

5. **Commits locais:** Alguns commits podem estar apenas locais (push para GitHub pode falhar por permissões, mas não é crítico)

---

## 🎯 Próximas Ações Imediatas (Ao Retomar)

1. **Verificar status do paralelismo:**
   - Acessar https://dev.azure.com/ux-solutions/_settings/parallelism
   - Verificar se foi aprovado

2. **Se paralelismo aprovado:**
   - Executar pipeline OndeTah.CI
   - Validar execução bem-sucedida
   - Continuar com migração de OndeTah.Bot

3. **Se paralelismo não aprovado:**
   - Considerar configurar agente auto-hospedado
   - OU aguardar mais alguns dias
   - OU comprar jobs paralelos (trial gratuito)

4. **Continuar migração:**
   - Próximo cluster: Foundations ou SSO (verificar dependências)
   - Aplicar mesmo processo: domínios → push → pipeline

---

## 📊 Resumo Executivo

| Item | Status | Progresso |
|------|--------|-----------|
| Repositórios migrados | 1/17 | 6% |
| Pipelines criados | 1/17 | 6% |
| Cluster OndeTah | Parcial | 50% |
| Bloqueio atual | Paralelismo | ⚠️ |
| Próxima ação | Solicitar paralelismo | 🔄 |

---

**Última atualização:** 28/01/2026  
**Próxima revisão:** 29/01/2026  
**Status:** ⚠️ Aguardando configuração de paralelismo para continuar
