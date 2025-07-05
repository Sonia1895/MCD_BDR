# Ejemplos de consultas con funciones de agregación

## Ejemplo de consulta para obtener el valor promedio de tarjetas de crédito por institución de acuerdo a sus diferentes períodos
<br>

```sql
select cve_institucion,
        count(distinct cve_periodo) cantidad_periodos, 
        avg(TOTAL) promedio
        from TDC_CANTIDAD_TOTAL 
group by 1 
order by count(distinct cve_periodo) desc,cve_institucion;
```

## Ejemplo de consulta para obtener el mínimo y máximo de tarjetas de crédito por institución de acuerdo a sus diferentes períodos
<br>

```sql
select cve_institucion,
        min(TOTAL) minimo_tarjetas, 
        max(TOTAL) maximo_tarjetas 
    from TDC_CANTIDAD_TOTAL 
group by 1 
order by max(TOTAL) desc;
```

## Ejemplo de consulta para obtener el cuantil del 70% de tarjetas de crédito totales por institución de acuerdo a sus diferentes períodos
<br>

```sql
select cve_institucion,
    PERCENTILE_CONT(0.70) WITHIN GROUP (ORDER BY TOTAL) AS percentile_75_cont 
from TDC_CANTIDAD_TOTAL 
    group by 1 order by 1;
```

## Ejemplos de consultas para obtener la moda (considerando solo una) y otra para las múltiples modas según los períodos
<br>

**Moda única**
```sql
SELECT cve_periodo,
    MODE() WITHIN GROUP (ORDER BY rango_perdida_esperada) AS rango_mas_comun
FROM
    PUBLIC.TDC_RANGO_PERDIDA_ESPERADA
	group by 1
	order by 1;
```

**Múltiples modas**
```sql
WITH frecuencias_por_periodo AS (
    SELECT cve_periodo, 
        rango_perdida_esperada,
        COUNT(rango_perdida_esperada) AS frecuencia
    FROM
           PUBLIC.TDC_RANGO_PERDIDA_ESPERADA
    GROUP BY cve_periodo, 
        rango_perdida_esperada
)
SELECT cve_periodo, 
    rango_perdida_esperada,
    frecuencia 
FROM
    frecuencias_por_periodo
WHERE
    cve_periodo::varchar||frecuencia::varchar in (SELECT cve_periodo::varchar||MAX(frecuencia)::varchar FROM frecuencias_por_periodo group by cve_periodo::varchar)
	order by cve_periodo,rango_perdida_esperada;
```

# Reporte de hallazgos
Hacer consultas SQL con funciones de agregación puede presentar algunas dificultades comunes. El reto principal a menudo radica en el uso correcto de la cláusula GROUP BY, ya que debes incluir en ella todas las columnas que no estén dentro de una función de agregación. Además, calcular la moda directamente en SQL puede ser un poco más complejo de lo esperado, ya que no existe una función de agregación estándar como SUM() o AVG(), requiriendo combinaciones de GROUP BY y ORDER BY con LIMIT o el uso de funciones de ventana como MODE() WITHIN GROUP. 
