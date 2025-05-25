# Desripción de base de datos seleccionada

Se selecciono la base de datos **Serie Historica Banca múltiple** proporcionada por la Comisión Nacional Bancaria y de Valores (CNBV) y actualizada a marzo de 2025. Esta base de datos es fundamental porque contiene la situación financiera y el estado de resultados de las entidades reguladas por la CNBV.

Desde la perspectiva de un banco de banca múltiple, esta información es sumamente valiosa. Ya que le permite evaluar su desempeño en relación con el mercado general y entender su posición frente a la competencia. Contar con estos datos les facilita la toma de decisiones estratégicas informadas.

La base principal que se usara contiene las siguientes **variables**:

|Variable|Tipo de dato
 --- | :---: 
sector|Texto
idconcepto|Texto
entidad|Texto
periodo|Númerico
saldo|Númerico
valor|Númerico

Además de la base de datos principal, se utilizaran dos bases de datos adicionales que funcionarán como catálogos. Estas son el catálogo de conceptos y el catálogo de instituciones.

El catálogo de conceptos será muy útil para identificar claramente el nombre de cada elemento dentro de los estados financieros. Por ejemplo, en lugar de ver solo un código, se podrá saber que ese código se refiere a "Ingresos por intereses" o "Gastos operativos".

Por su parte, el catálogo de instituciones nos permitirá asociar los códigos de las entidades con sus nombres comerciales, por ejemplo, sabiendo que "040002" corresponde a "Banamex". Ambos catálogos son esenciales para poder entender la información de la base de datos principal de forma clara y directa.

Con los valores del estado financiero se puede revisar diferentes indicadores y su evolución en el tiempo, por mencionar algunos:

- Índice de Morosidad (o Cartera Vencida/Etapa 3): Mide el porcentaje de la cartera que no están siendo pagada a tiempo.
- $Cartera total: Saldo total (activo) de las entidades en créditos comerciales, consumo e hipotecarios.
- %Crecimiento anual de las carteras
- $Utilidad neta
- Índice de cobertura: Mide qué tan bien está "cubierta" la entidad con reservas para posibles créditos incobrables.


# Investigación SGBD



# Bibliografía
**Portafolio de Información**. (s/f). Gob.mx. Recuperado el 25 de mayo de 2025, de https://portafolioinfo.cnbv.gob.mx/Paginas/Inicio.aspx

**Power BI report**. (s/f). Powerbi.com. Recuperado el 25 de mayo de 2025, de https://app.powerbi.com/view?r=eyJrIjoiMGI0YWE0NzktYWQyYi00ZWFmLTllMWUtNDllZmY1OTc2ZGU0IiwidCI6IjVlMmM0OTc3LTEwN2QtNDBhMy04YWY3LTcwMDc0ODFhNjBkNCIsImMiOjR9

**DISPOSICIONES DE CARÁCTER GENERAL APLICABLES A LAS INSTITUCIONES DE CRÉDITO**. Gob.mx. Recuperado el 25 de mayo de 2025, de https://www.cnbv.gob.mx/Normatividad/Disposiciones%20de%20car%C3%A1cter%20general%20aplicables%20a%20las%20instituciones%20de%20cr%C3%A9dito.pdf

**Pérez, S. D.** (2021, septiembre 8). Gestor de Base de datos: Qué es, Funcionalidades y Ejemplos. Intelequia. https://intelequia.com/es/blog/post/gestor-de-base-de-datos-qu%C3%A9-es-funcionalidades-y-ejemplos