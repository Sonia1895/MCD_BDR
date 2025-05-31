# Diagrama entidad-relación
## En el diagrama se incluye los niveles entidad, atributos, dominio y relación


```mermaid
---
title: Información Banca Múltiple
---
graph TD
    %% Definición de Estilos Personalizados
    classDef entidad fill:#add8e6,stroke:#333,stroke-width:2px,rx:5px,ry:5px;
    classDef relacion fill:#ffb3ba,stroke:#333,stroke-width:2px,shape:rhombus;
    classDef atributo fill:#c2e0c6,stroke:#333,stroke-width:2px,shape:hexagon;

    %% Entidades (Rectángulos)
    Serie_Historica_Banca_Multiple[Serie Histórica Banca Múltiple]:::entidad
    Numero_Tarjetas_Credito_Institucion[Número Tarjetas Crédito Institución]:::entidad
    Distribucion_Tarjetas_Perdida_Esperada[Distribución Tarjetas Pérdida Esperada]:::entidad
    Distribucion_Tarjetas_Pago_Realizado[Distribución Tarjetas Pago Realizado]:::entidad
    Catalogo_Instituciones[Catálogo Instituciones]:::entidad
    Catalogo_Conceptos[Catálogo Conceptos]:::entidad

    %% Atributos (Hexágonos) y sus Conexiones a Entidades
    %% Serie_Historica_Banca_Multiple Atributos
    SHBM_sector{sector: Texto}:::atributo --- Serie_Historica_Banca_Multiple
    SHBM_idconcepto{idconcepto: Texto}:::atributo --- Serie_Historica_Banca_Multiple
    SHBM_entidad{entidad: Texto}:::atributo --- Serie_Historica_Banca_Multiple
    SHBM_periodo{periodo: Numérico}:::atributo --- Serie_Historica_Banca_Multiple
    SHBM_saldo{saldo: Numérico}:::atributo --- Serie_Historica_Banca_Multiple
    SHBM_valor{valor: Numérico}:::atributo --- Serie_Historica_Banca_Multiple

    %% Numero_Tarjetas_Credito_Institucion Atributos
    NTCI_cve_institucion{cve_institucion: Texto}:::atributo --- Numero_Tarjetas_Credito_Institucion
    NTCI_cve_periodo{cve_periodo: Numérico}:::atributo --- Numero_Tarjetas_Credito_Institucion
    NTCI_total{total: Numérico}:::atributo --- Numero_Tarjetas_Credito_Institucion

    %% Distribucion_Tarjetas_Perdida_Esperada Atributos
    DTPE_cve_institucion{cve_institucion: Texto}:::atributo --- Distribucion_Tarjetas_Perdida_Esperada
    DTPE_cve_periodo{cve_periodo: Numérico}:::atributo --- Distribucion_Tarjetas_Perdida_Esperada
    DTPE_rango_perdida_esperada{rango_perdida_esperada: int}:::atributo --- Distribucion_Tarjetas_Perdida_Esperada
    DTPE_total{total: Numérico}:::atributo --- Distribucion_Tarjetas_Perdida_Esperada

    %% Distribucion_Tarjetas_Pago_Realizado Atributos
    DTPR_cve_institucion{cve_institucion: Texto}:::atributo --- Distribucion_Tarjetas_Pago_Realizado
    DTPR_cve_periodo{cve_periodo: Numérico}:::atributo --- Distribucion_Tarjetas_Pago_Realizado
    DTPR_rango_pago_realizado_vs_no_intereses{rango_pago_realizado_vs_no_intereses: int}:::atributo --- Distribucion_Tarjetas_Pago_Realizado
    DTPR_total{total: Numérico}:::atributo --- Distribucion_Tarjetas_Pago_Realizado

    %% Catalogo_Instituciones Atributos
    CI_cve_institucion{cve_institucion: Texto}:::atributo --- Catalogo_Instituciones
    CI_nombre_institucion{nombre_institucion: Texto}:::atributo --- Catalogo_Instituciones

    %% Catalogo_Conceptos Atributos
    CC_idconcepto{idconcepto: Texto}:::atributo --- Catalogo_Conceptos
    CC_nombre_concepto{nombre_concepto: Texto}:::atributo --- Catalogo_Conceptos
    CC_descripcion_concepto{descripcion_concepto: Texto}:::atributo --- Catalogo_Conceptos

    %% Relaciones (Rombos) y sus Conexiones
    Se_Refiere_a_Institucion{se refiere a}:::relacion
    Usa_Concepto{usa}:::relacion
    Pertenece_a_Institucion_NTCI{pertenece a}:::relacion
    Pertenece_a_Institucion_DTPE{pertenece a}:::relacion
    Pertenece_a_Institucion_DTPR{pertenece a}:::relacion

    %% Conexiones de Entidades a Relaciones y luego a otras Entidades
    Serie_Historica_Banca_Multiple --> Se_Refiere_a_Institucion --> Catalogo_Instituciones
    Serie_Historica_Banca_Multiple --> Usa_Concepto --> Catalogo_Conceptos

    Numero_Tarjetas_Credito_Institucion --> Pertenece_a_Institucion_NTCI --> Catalogo_Instituciones
    Distribucion_Tarjetas_Perdida_Esperada --> Pertenece_a_Institucion_DTPE --> Catalogo_Instituciones
    Distribucion_Tarjetas_Pago_Realizado --> Pertenece_a_Institucion_DTPR --> Catalogo_Instituciones