# Diagrama entidad-relación

## En el diagrama se incluye los niveles entidad, atributos, dominio y relación

```mermaid
---
title: Información CNBV Banca Múltiple 
---
graph TD
    %% DIAGRAMA DE ENTIDAD-RELACIÓN ADAPTADO
    %% Entidades: Rectángulos
    %% Relaciones: Rombos
    %% Atributos: Óvalos (solo el nombre del atributo)
    %% Dominios: Hexágonos (el tipo de dato asociado a un atributo)

    %% Definición de Estilos Personalizados
    classDef entidad fill:#add8e6,stroke:#333,stroke-width:2px,rx:5px,ry:5px;
    classDef relacion fill:#ffb3ba,stroke:#333,stroke-width:2px,shape:rhombus;
    classDef atributo fill:#c2e0c6,stroke:#333,stroke-width:2px,shape:ellipse; 
    classDef dominio fill:#d8c2e6,stroke:#333,stroke-width:2px,shape:hexagon; 

    %% Entidades (Rectángulos)
    Serie_Historica_Banca_Multiple[Serie Histórica Banca Múltiple]:::entidad
    Numero_Tarjetas_Credito_Institucion[Número Tarjetas Crédito Institución]:::entidad
    Distribucion_Tarjetas_Perdida_Esperada[Distribución Tarjetas Pérdida Esperada]:::entidad
    Distribucion_Tarjetas_Pago_Realizado[Distribucion Tarjetas Pago Realizado]:::entidad
    Catalogo_Instituciones[Catálogo Instituciones]:::entidad
    Catalogo_Conceptos[Catálogo Conceptos]:::entidad

    %% Atributos (Óvalos) y sus Conexiones a Entidades
    %% Serie_Historica_Banca_Multiple Atributos
    Serie_Historica_Banca_Multiple --- SHBM_sector_attr((sector)):::atributo
    Serie_Historica_Banca_Multiple --- SHBM_idconcepto_attr((idconcepto)):::atributo
    Serie_Historica_Banca_Multiple --- SHBM_entidad_attr((entidad)):::atributo
    Serie_Historica_Banca_Multiple --- SHBM_periodo_attr((periodo)):::atributo
    Serie_Historica_Banca_Multiple --- SHBM_saldo_attr((saldo)):::atributo
    Serie_Historica_Banca_Multiple --- SHBM_valor_attr((valor)):::atributo

    %% Numero_Tarjetas_Credito_Institucion Atributos
    Numero_Tarjetas_Credito_Institucion --- NTCI_cve_institucion_attr((cve_institucion)):::atributo
    Numero_Tarjetas_Credito_Institucion --- NTCI_cve_periodo_attr((cve_periodo)):::atributo
    Numero_Tarjetas_Credito_Institucion --- NTCI_total_attr((total)):::atributo

    %% Distribucion_Tarjetas_Perdida_Esperada Atributos
    Distribucion_Tarjetas_Perdida_Esperada --- DTPE_cve_institucion_attr((cve_institucion)):::atributo
    Distribucion_Tarjetas_Perdida_Esperada --- DTPE_cve_periodo_attr((cve_periodo)):::atributo
    Distribucion_Tarjetas_Perdida_Esperada --- DTPE_rango_perdida_esperada_attr((rango_perdida_esperada)):::atributo
    Distribucion_Tarjetas_Perdida_Esperada --- DTPE_total_attr((total)):::atributo

    %% Distribucion_Tarjetas_Pago_Realizado Atributos
    Distribucion_Tarjetas_Pago_Realizado --- DTPR_cve_institucion_attr((cve_institucion)):::atributo
    Distribucion_Tarjetas_Pago_Realizado --- DTPR_cve_periodo_attr((cve_periodo)):::atributo
    Distribucion_Tarjetas_Pago_Realizado --- DTPR_rango_pago_realizado_vs_no_intereses_attr((rango_pago_realizado_vs_no_intereses)):::atributo
    Distribucion_Tarjetas_Pago_Realizado --- DTPR_total_attr((total)):::atributo

    %% Catalogo_Instituciones Atributos
    Catalogo_Instituciones --- CI_cve_institucion_attr((cve_institucion)):::atributo
    Catalogo_Instituciones --- CI_nombre_institucion_attr((nombre_institucion)):::atributo

    %% Catalogo_Conceptos Atributos
    Catalogo_Conceptos --- CC_idconcepto_attr((idconcepto)):::atributo
    Catalogo_Conceptos --- CC_nombre_concepto_attr((nombre_concepto)):::atributo
    Catalogo_Conceptos --- CC_descripcion_concepto_attr((descripcion_concepto)):::atributo

    %% Dominios (Hexágonos con Tipo de Dato) y sus Conexiones a Atributos
    %% Los dominios se conectan a los atributos a los que "dan su tipo"

    %% DECLARACIÓN EXPLICITA DE LOS NODOS DOMINIO (para asegurar que están definidos antes de usarse)
    DOM_string{string}:::dominio
    DOM_Texto{Texto}:::dominio
    DOM_Numerico{Numérico}:::dominio
    DOM_int{int}:::dominio


    SHBM_sector_attr --- DOM_Texto
    SHBM_idconcepto_attr --- DOM_Texto
    SHBM_entidad_attr --- DOM_Texto
    SHBM_periodo_attr --- DOM_Numerico
    SHBM_saldo_attr --- DOM_Numerico
    SHBM_valor_attr --- DOM_Numerico

    NTCI_cve_institucion_attr --- DOM_Texto
    NTCI_cve_periodo_attr --- DOM_Numerico
    NTCI_total_attr --- DOM_Numerico

    DTPE_cve_institucion_attr --- DOM_Texto
    DTPE_cve_periodo_attr --- DOM_Numerico
    DTPE_rango_perdida_esperada_attr --- DOM_int
    DTPE_total_attr --- DOM_Numerico

    DTPR_cve_institucion_attr --- DOM_Texto
    DTPR_cve_periodo_attr --- DOM_Numerico
    DTPR_rango_pago_realizado_vs_no_intereses_attr --- DOM_int
    DTPR_total_attr --- DOM_Numerico

    CI_cve_institucion_attr --- DOM_Texto
    CI_nombre_institucion_attr --- DOM_Texto

    CC_idconcepto_attr --- DOM_Texto
    CC_nombre_concepto_attr --- DOM_Texto
    CC_descripcion_concepto_attr --- DOM_Texto


    %% Relaciones (Rombos)
    Se_Refiere_a_Institucion{se refiere a}:::relacion
    Usa_Concepto{usa}:::relacion
    Union_Institucion_Periodo{unido por institución y período}:::relacion


    %% Conexiones de Entidades a Relaciones y luego a otras Entidades (Catálogos)
    Serie_Historica_Banca_Multiple --> Se_Refiere_a_Institucion --> Catalogo_Instituciones
    Serie_Historica_Banca_Multiple --> Usa_Concepto --> Catalogo_Conceptos

    %% Conexiones de todas las tablas de datos a la relación central de Institución/Período
    Serie_Historica_Banca_Multiple --> Union_Institucion_Periodo
    Numero_Tarjetas_Credito_Institucion --> Union_Institucion_Periodo
    Distribucion_Tarjetas_Perdida_Esperada --> Union_Institucion_Periodo
    Distribucion_Tarjetas_Pago_Realizado --> Union_Institucion_Periodo

    %% La relación central de Institución/Período se conecta a los atributos relevantes
    Union_Institucion_Periodo --> Catalogo_Instituciones
    Union_Institucion_Periodo --> SHBM_periodo_attr
    Union_Institucion_Periodo --> NTCI_cve_periodo_attr
    Union_Institucion_Periodo --> DTPE_cve_periodo_attr
    Union_Institucion_Periodo --> DTPR_cve_periodo_attr
