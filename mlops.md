flowchart LR
    subgraph Azure_Prod_Tenant["Azure Prod Tenant (Terraform Managed)"]
        SA[Azure Storage Account]
        KV[Azure Key Vault]
        UC[Unity Catalog Metastore]
        DC[Databricks UC Connector]
    end

    subgraph DB_MLPS["Databricks Workspace: ***-mlps (DS)"]
        DS[Data Scientists]
        DSCAT["ds_catalog.ai_features"]
    end

    subgraph DB_PROD["Databricks Workspace: ***-prod (Production)"]
        PRODCAT["curated_catalog.ai_features"]
        PRODJOB[Prod Jobs / Model Serving]
    end

    subgraph CI_CD["GitHub"]
        GH[GitHub Repo]
        GA[GitHub Actions]
    end

    %% Infra connections
    UC --- DB_MLPS
    UC --- DB_PROD
    DC --- SA
    UC --- DC

    %% DS workflow
    DS --> DB_MLPS
    DB_MLPS --> DSCAT

    %% CI/CD flow
    GH --> GA
    GA --> DB_PROD

    %% Prod usage
    DB_PROD --> PRODCAT
    PRODCAT --> PRODJOB
