```mermaid
---
title: Información Banca Múltiple
---
erDiagram
    %% Entidades Principales
    Serie_Historica_Banca_Multiple {
        string id_registro PK "Clave primaria para esta tabla"
        string sector
        string idconcepto FK "Referencia al Catalogo_Conceptos"
        string entidad FK "Referencia al Catalogo_Instituciones"
        numeric periodo
        numeric saldo
        numeric valor
    }

    Numero_Tarjetas_Credito_Institucion {
        string id_registro PK "Clave primaria para esta tabla"
        string cve_institucion FK "Referencia al Catalogo_Instituciones"
        numeric cve_periodo
        numeric total
    }

    Distribucion_Tarjetas_Perdida_Esperada {
        string id_registro PK "Clave primaria para esta tabla"
        string cve_institucion FK "Referencia al Catalogo_Instituciones"
        numeric cve_periodo
        int rango_perdida_esperada
        numeric total
    }

    Distribucion_Tarjetas_Pago_Realizado {
        string id_registro PK "Clave primaria para esta tabla"
        string cve_institucion FK "Referencia al Catalogo_Instituciones"
        numeric cve_periodo
        int rango_pago_realizado_vs_no_intereses
        numeric total
    }

    %% Entidades de Catálogo
    Catalogo_Instituciones {
        string cve_institucion PK "Clave única de la institución"
        string nombre_institucion
    }

    Catalogo_Conceptos {
        string idconcepto PK "Clave única del concepto financiero"
        string nombre_concepto
        string descripcion_concepto
    }

    %% Relaciones
    Serie_Historica_Banca_Multiple ||--o{ Catalogo_Instituciones : "entidad se refiere a"
    Serie_Historica_Banca_Multiple ||--o{ Catalogo_Conceptos : "idconcepto usa"

    Numero_Tarjetas_Credito_Institucion ||--o{ Catalogo_Instituciones : "cve_institucion pertenece a"
    Distribucion_Tarjetas_Perdida_Esperada ||--o{ Catalogo_Instituciones : "cve_institucion pertenece a"
    Distribucion_Tarjetas_Pago_Realizado ||--o{ Catalogo_Instituciones : "cve_institucion pertenece a"
