# Diagrama de relación

## Este sería el diagrama relacional

```mermaid
---
title: Diagrama de relación
---
erDiagram

    %% Entidades
    %% Atributos con KEY indican PK, y atributos con FK indican Foreign Key

    %% Entidad Serie_Historica_Banca_Multiple
    Serie_Historica_Banca_Multiple {
        string sector PK
        string idconcepto PK, FK "idconcepto"
        string entidad PK, FK "cve_institucion"
        int periodo PK, FK "cve_periodo"
        int saldo PK
        numeric valor
    }

    %% Entidad Numero_Tarjetas_Credito_Institucion
    Numero_Tarjetas_Credito_Institucion {
        string cve_institucion PK, FK "cve_institucion"
        int cve_periodo PK, FK "cve_periodo"
        numeric total
    }

    %% Entidad Distribucion_Tarjetas_Perdida_Esperada
    Distribucion_Tarjetas_Perdida_Esperada {
        string cve_institucion PK, FK "cve_institucion"
        int cve_periodo PK, FK "cve_periodo"
        int rango_perdida_esperada PK
        numeric total
    }

    %% Entidad Distribucion_Tarjetas_Pago_Realizado
    Distribucion_Tarjetas_Pago_Realizado {
        string cve_institucion PK, FK "cve_institucion"
        int cve_periodo PK, FK "cve_periodo"
        int rango_pago_realizado_vs_no_intereses PK
        numeric total
    }

    %% Entidad Catalogo_Instituciones
    Catalogo_Instituciones {
        string cve_institucion PK
        string nombre_institucion
    }

    %% Entidad Catalogo_Conceptos
    Catalogo_Conceptos {
        string idconcepto PK, FK "idconcepto"
        string nombre_concepto
        string descripcion_concepto
    }

    %% Relaciones
    %% La notación es: Tabla1 --|{o-- Tabla2 : "Tipo de Relación"
    %% --|: uno-a-muchos (one-to-many) con obligatoriedad en el lado "uno"
    %% }o--: muchos-a-uno (many-to-one)
    %% --o|: uno-a-uno (one-to-one)

    Serie_Historica_Banca_Multiple }o--|| Catalogo_Conceptos : "usa (idconcepto)"
    Serie_Historica_Banca_Multiple }o--|| Catalogo_Instituciones : "se refiere a (entidad)"

    Numero_Tarjetas_Credito_Institucion }o--|| Catalogo_Instituciones : "pertenece a"
    Distribucion_Tarjetas_Perdida_Esperada }o--|| Catalogo_Instituciones : "asociada a"
    Distribucion_Tarjetas_Pago_Realizado }o--|| Catalogo_Instituciones : "distribuida por"

    %% Relación implícita de unión por institución y periodo entre las tablas de datos.
    %% No se dibuja una relación directa con un rombo en erDiagram,
    %% sino que se infiere por las FKs y PKs compuestas que comparten.
    %% Solo indicaré las relaciones explícitas de FK.
    Serie_Historica_Banca_Multiple ||--o{ Numero_Tarjetas_Credito_Institucion : "relaciona por Institución y Periodo"
    Serie_Historica_Banca_Multiple ||--o{ Distribucion_Tarjetas_Perdida_Esperada : "relaciona por Institución y Periodo"
    Serie_Historica_Banca_Multiple ||--o{ Distribucion_Tarjetas_Pago_Realizado : "relaciona por Institución y Periodo"