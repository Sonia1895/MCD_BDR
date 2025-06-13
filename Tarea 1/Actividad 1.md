# Desripción de base de datos seleccionada
Se seleccionó la base de datos basándose en la información proporcionada por la Comisión Nacional Bancaria y de Valores (CNBV) en su portafolio de información, actualizada a marzo de 2025. Esta base de datos es interesante porque contiene datos sobre la situación financiera y el estado de resultados de las entidades reguladas por la CNBV, así como la distribución de los productos de tarjeta de crédito.

Desde la perspectiva de un banco de banca múltiple, esta información es sumamente valiosa. Le permite evaluar su desempeño en relación con el mercado general y entender su posición frente a la competencia. Contar con estos datos facilita la toma de decisiones estratégicas informadas.

La base de datos contiene las siguientes entidades y atributos variables:

- **Serie Historica Banca múltiple**

|Variable|Tipo de dato|
| --- | :---: |
|sector|Texto|
|idconcepto|Texto|
|entidad|Texto|
|periodo|Númerico|
|saldo|Númerico|
|valor|Númerico|

- **Número de tarjetas de crédito por institución**

|Variable|Tipo de dato
 --- | :---: 
cve_institucion|Texto
cve_periodo|Númerico
total|Númerico

- **Distribución de tarjetas por pérdida esperada**

|Variable|Tipo de dato
 --- | :---: 
cve_institucion|Texto
cve_periodo|Númerico
rango_perdida_esperada|int
total|Númerico

- **Distribución de tarjetas por porcentaje de pago realizado contra pago para no generar intereses**

|Variable|Tipo de dato
 --- | :---: 
cve_institucion|Texto
cve_periodo|Númerico
rango_pago_realizado_vs_no_intereses|int
total|Númerico

Además de las tablas principales, se utilizarán dos bases de datos adicionales que funcionarán como catálogos: el catálogo de conceptos y el catálogo de instituciones.

El catálogo de conceptos será muy útil para identificar claramente el nombre de cada elemento dentro de los estados financieros. Por ejemplo, en lugar de ver solo un código, se podrá saber que ese código se refiere a "Ingresos por intereses" o "Gastos operativos".

Por su parte, el catálogo de instituciones permitirá asociar los códigos de las entidades con sus nombres comerciales; por ejemplo, saber que "040002" corresponde a "Banamex". Ambos catálogos son esenciales para entender la información de la base de datos principal de forma clara y directa.

Con los valores del estado financiero, se pueden revisar diferentes indicadores y su evolución en el tiempo. Por mencionar algunos:

- Índice de Morosidad (o Cartera Vencida/Etapa 3): Mide el porcentaje de la cartera que no se está pagando a tiempo.
- Cartera total: Saldo total (activo) de las entidades en créditos comerciales, de consumo e hipotecarios.
- % Crecimiento anual de las carteras
- Utilidad neta
- Índice de cobertura: Mide qué tan bien está "cubierta" la entidad con reservas para posibles créditos incobrables.

Y con la información relacionada con las tarjetas de crédito, se puede vincular a cada institución bancaria la cantidad de tarjetas de crédito, así como el comportamiento de estas en sus pagos exigibles y niveles de pérdida esperada (reservas).

# Investigación SGBD

Un sistema gestor de base de datos (SGBD) o Database Management System (DBMS) es un conjunto de programas invisibles para el usuario final con el que se administra y gestiona la información que incluye una base de datos.

Los gestores de datos o gestores de base de datos permiten administrar todo acceso a la base de datos, pues tienen el objetivo de servir de interfaz entre esta, el usuario y las aplicaciones.

En pocas palabras, el gestor de base de datos controla cualquier operación ejecutada por el usuario contra la BBDD. Para desarrollar esta función, es normal que se requieran herramientas específicas, como por ejemplo sistemas de búsqueda y de generación de informes, así como distintas aplicaciones. Los gestores de base de datos también permiten lo siguiente:
- Que las interacciones con cualquier base de datos gestionada puedan desarrollarse siempre separadamente a los programas o aplicaciones que los gestionan.
- La manipulación de bases de datos, garantizando su seguridad, integridad y consistencia.
- La definición de bases de datos a diferentes niveles de abstracción.

Un SGBD permite definir los datos, además de manipularlos, aplicar medidas de seguridad e integridad y recuperarlos o restaurarlos después de producirse algún tipo de fallo. Algunas de las funciones principales de los gestores de bases de datos son las siguientes:

- Contribuyen a la creación de bases de datos más eficaces y consistentes.
- Determinan las estructuras de almacenamiento del sistema.
- Facilitan las búsquedas de datos de cualquier tipo y procedencia a los usuarios de negocio.
- Ayudan a mantener la integridad de los activos informacionales de la empresa.
- Introducen cambios en la información, si es requerido.
- Simplifican los procesos de consulta.
- Controlan los movimientos que se observan en la base de datos.

Uno de los SGBD más usado es PostgreSQL. 
PostgreSQL, comúnmente pronunciado "Post-GRES", es una base de datos de código abierto que tiene una sólida reputación por su fiabilidad, flexibilidad y soporte de estándares técnicos abiertos. A diferencia de otros RDMBS (sistemas de gestión de bases de datos relacionales), PostgreSQL (enlace externo a ibm.com) soporta tipos de datos relacionales y no relacionales. Esto la convierte en una de las bases de datos relacionales más compatibles, estables y maduras disponibles actualmente.


# Bibliografía
**Portafolio de Información**. (s/f). Gob.mx. Recuperado el 25 de mayo de 2025, de https://portafolioinfo.cnbv.gob.mx/Paginas/Inicio.aspx

**Power BI report**. (s/f). Powerbi.com. Recuperado el 25 de mayo de 2025, de https://app.powerbi.com/view?r=eyJrIjoiMGI0YWE0NzktYWQyYi00ZWFmLTllMWUtNDllZmY1OTc2ZGU0IiwidCI6IjVlMmM0OTc3LTEwN2QtNDBhMy04YWY3LTcwMDc0ODFhNjBkNCIsImMiOjR9

**DISPOSICIONES DE CARÁCTER GENERAL APLICABLES A LAS INSTITUCIONES DE CRÉDITO**. Gob.mx. Recuperado el 25 de mayo de 2025, de https://www.cnbv.gob.mx/Normatividad/Disposiciones%20de%20car%C3%A1cter%20general%20aplicables%20a%20las%20instituciones%20de%20cr%C3%A9dito.pdf

**Pérez, S. D.** (2021, septiembre 8). Gestor de Base de datos: Qué es, Funcionalidades y Ejemplos. Intelequia. https://intelequia.com/es/blog/post/gestor-de-base-de-datos-qu%C3%A9-es-funcionalidades-y-ejemplos

**¿Qué es PostgreSQL?** (2023, octubre 2). Ibm.com. https://www.ibm.com/mx-es/topics/postgresql

**PostgreSQL**. (2025, mayo 25). PostgreSQL. https://www.postgresql.org/