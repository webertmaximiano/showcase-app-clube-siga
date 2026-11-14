# 🏢 Case Study Arquitetural — Ecossistema Clube Siga (CRM AI-First, PDV Fiscal & Afiliados)

![Status](https://img.shields.io/badge/Status-Em_Produção-10B981?style=for-the-badge)
[![CRM Clube Siga](https://img.shields.io/badge/CRM_AI--First-crm.clubesiga.com.br-4F46E5?style=for-the-badge)](https://crm.clubesiga.com.br)
[![App Clube Siga](https://img.shields.io/badge/Plataforma-app.clubesiga.com.br-0EA5E9?style=for-the-badge)](https://app.clubesiga.com.br)
![Arquitetura](https://img.shields.io/badge/Arquitetura-Multi--Tenant_SaaS-F59E0B?style=for-the-badge)

> **Nota de Governança:** Este repositório público apresenta a arquitetura, decisões de engenharia e especificações técnicas da suíte comercial **Clube Siga**. Os repositórios de produção permanecem privados por conformidade de segurança e propriedade intelectual.

---

## 🎯 Visão Geral do Ecossistema

O **Ecossistema Clube Siga** integra três pilares de operação comercial e relacionamento em produção:

1. **💬 [CRM Clube Siga (`crm.clubesiga.com.br`)](https://crm.clubesiga.com.br):** Plataforma Multiempresa *AI-First* para gestão de funil de vendas, atendimento omnichannel via WhatsApp e automação comercial assistida por agentes de IA nativos (pronta para **Model Context Protocol — MCP**).
2. **🧾 PDV Clube Siga:** Sistema de Gestão Comercial, Controle de Estoque e Frente de Caixa com motor fiscal desacoplado e assíncrono (`NF-e`, `NFC-e` e `NFS-e` via webhooks idempotentes e certificados digitais A1).
3. **🤝 App Clube Siga (`app.clubesiga.com.br`):** Plataforma SaaS de fidelidade, cashback, árvore unilevel e matriz forçada com ledger financeiro auditável e proteção contra condições de corrida (*atomic row-level locking*).

---

## 🏗️ Arquitetura Técnica & Decisões de Engenharia

```mermaid
flowchart LR
    subgraph Channels["Canais & Pontos de Venda"]
        WA["WhatsApp Business / Chat"]
        WEB["Portais React / TypeScript"]
        POS["Terminais PDV & Caixa"]
    end

    subgraph Core["Núcleo SaaS Clube Siga"]
        CRM["CRM Clube Siga (Funil + IA)"]
        ERP["PDV & Gestão de Estoque"]
        AFF["Motor de Fidelidade & Matriz"]
    end

    subgraph Async["Processamento Assíncrono & Integrações"]
        MCP["Agentes de IA (Beto / MCP)"]
        FISC["Gateway Fiscal Assíncrono (NF-e/NFC-e/NFS-e)"]
        PAY["Split & Gateways de Pagamento"]
    end

    WA & WEB --> CRM
    POS & WEB --> ERP
    WEB --> AFF
    CRM --> MCP
    ERP --> FISC
    AFF & ERP --> PAY
```

### Destaques de Senioridade Aplicados
- **Idempotência Determinística por Entidade:** Todas as operações financeiras, emissões fiscais e bonificações utilizam chaves de idempotência derivadas de identificadores de domínio, eliminando duplicidades em retentativas de fila.
- **Segurança Purple-Team & LGPD:** Isolamento estrito por `tenant_id`, validação contra *BOLA/IDOR*, segredos injetados exclusivamente em runtime via *Docker Swarm Secrets* e trilha de auditoria imutável.
- **Observabilidade Full-Stack (LGTM):** Monitoramento contínuo com **Grafana**, **Prometheus**, **Loki**, **Alloy** e **cAdvisor** integrados ao esquadrão de resposta a incidentes da plataforma de agentes.

---

## 🛠️ Stack Tecnológica

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![PHP](https://img.shields.io/badge/PHP_8.4-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Docker Swarm](https://img.shields.io/badge/Docker_Swarm-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Traefik](https://img.shields.io/badge/Traefik_v3-24A1C1?style=for-the-badge&logo=traefikproxy&logoColor=white)
