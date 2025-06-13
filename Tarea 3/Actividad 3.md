# Diagrama de relación

## Este sería el diagrama relacional

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

# Ejemplos de operadores de álgebra relacional


1. **Selección (σ)**

    La operación de Selección (σ) se usa para filtrar filas (registros) de una tabla basándose en una condición específica.<br>
    Ejemplo: se quiere encontrar todos los estados financieros del periodo de 202412.

    σ<small>periodo=202412</small>(Serie_Historica_Banca_Multiple)

    Esta operación devolverá todas las filas y columnas de Serie_Historica_Banca_Multiple donde el valor de la columna periodo es 202412.

2. **Proyección (Π)**

    La operación proyección se utiliza para seleccionar columnas específicas de una tabla, eliminando las columnas que no te interesan. A diferencia de la selección que filtra filas, la proyección filtra columnas. <br>
    Ejemplo: solo se quiere ver la cve_institucion de la tabla Numero_Tarjetas_Credito_Institucion.

    Π<small>cve_institucion</small>(Numero_Tarjetas_Credito_Institucion)

    
    Esta operación regresara solo lo que contiene la columna cve_institucion de Numero_Tarjetas_Credito_Institucion.

3. **Unión natural con condición <small>⋈</small>**

    La operación de Unión Natural (⋈) se usa para combinar filas de dos o más tablas basándose en columnas que comparten nombres y dominios comunes (es decir, columnas con el mismo significado que actúan como claves). <br>
    Ejemplo: unir la información de los nombres de institución de la tabla  Catalogo_Instituciones con los datos de la tabla Numero_Tarjetas_Credito_Institucion
    
    Catalogo_Instituciones <small>**⋈Catalogo_Instituciones.cve_institucion=Numero_Tarjetas_Credito_Institucion.cve_institucion**</small> Numero_Tarjetas_Credito_Institucion

    El resultado será una nueva tabla que combina las columnas de ambas tablas, emparejando las filas donde Catalogo_Instituciones.cve_institucion coincide con Numero_Tarjetas_Credito_Institucion.cve_institucion.

4. **Agregación (G)**

    La operación de Agregación (G) se utiliza para calcular valores resumidos (como sumas, promedios, conteos, máximos o mínimos) sobre grupos de filas.<br>
    Ejemplo: se quiere calcular el total de Total de tarjetas de crédito por cada institución en la tabla Numero_Tarjetas_Credito_Institucion:

     G <small>**cve_institucion,cve_periodo SUM(total)**</small>(Numero_Tarjetas_Credito_Institucion)
    
    Esta operación agrupará las filas por cve_institucion y cve_periodo y luego sumará el valor de la columna total para cada grupo, dando el total de tarjetas por cada institución.

