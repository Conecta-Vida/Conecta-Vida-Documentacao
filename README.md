# 🌟 Documentação Oficial - Projeto ConectaVida

Bem-vindo ao repositório central de documentação técnica do ecossistema **ConectaVida**. Este espaço consolida os guias arquiteturais, especificações de backend e manuais do painel administrativo para alinhar toda a equipe de desenvolvimento.

## 📚 Índice de Documentos
* [📖 1. Visão Geral e Arquitetura Macro](#1-visão-geral-do-sistema)
* [⚙️ 2. Especificação Técnica da API (Backend)](API_BACKEND.md)
* [💻 3. Manual do Painel Administrador (Frontend)](ADMIN_FRONTEND.md)

---

## 1. Visão Geral do Sistema
O **ConectaVida** é uma plataforma distribuída voltada à gestão de saúde pública, campanhas sanitárias e comunicação com o cidadão. 

### Arquitetura Macro (Containers)
```mermaid
flowchart TB
    subgraph Clientes ["Camada de Clientes"]
        AdminWeb["💻 ConectaVida-Admin (Web SPA)\nReact + TypeScript + Tailwind CSS"]
        MobileApp["📱 App Cidadão (Mobile)\nConsumo de Feed e Push"]
    end

    subgraph Backend ["Servidor Backend (Spring Boot Multi-Module)"]
        AdminAPI["⚙️ Módulo: admin-api\nGestão, Relatórios, Auth e CRUDs"]
        MobileAPI["📲 Módulo: mobile-api\nFeed Público e Registro de Tokens"]
        Core["🧱 Módulo: core\nDomínio JPA, Repositórios, Mappers,\nSegurança, RabbitMQ e FCM"]
    end

    subgraph Infra ["Infraestrutura (Docker Compose)"]
        DB[("🐘 PostgreSQL\nSchema: public")]
        Broker[["🐇 RabbitMQ\nMensageria Assíncrona"]]
        Firebase(("🔥 Firebase (FCM)\nPush Notifications"))
    end

    AdminWeb -- "REST / JSON" --> AdminAPI
    MobileApp -- "REST / JSON" --> MobileAPI
    AdminAPI --> Core
    MobileAPI --> Core
    Core -- "Hibernate / JPA" --> DB
    Core -- "AMQP" --> Broker
    Core -- "HTTP / SDK" --> Firebase
