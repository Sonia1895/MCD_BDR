# Creación de vistas

```sql
/* Esta vista incluye el histórico de estados financieros, detallando los nombres de los conceptos y sus respectivos niveles, 
así como los nombres de las entidades. Todo esto para identificar los datos en la tabla con mayor claridad.*/

CREATE VIEW HISTORICO_ESTADOS_FINANCIEROS_CATALOGOS as
SELECT a.*, b.nombre_entidad, c.descripcion, c.idtema, c.nivel, c.indicador, c.orden
	FROM  PUBLIC.HISTORICO_ESTADOS_FINANCIEROS AS a
	LEFT JOIN PUBLIC.CATALOGO_ENTIDADES AS b
	ON a.entidad = b.entidad
	LEFT JOIN PUBLIC.CATALOGO_CONCEPTOS_FINANCIEROS AS c
	ON a.idconcepto = c.idconcepto;

-- Validar la cantidad de registros: los conteos deben ser iguales. De lo contrario, algo estaría mal en la información.
select
(select count (*) from PUBLIC.HISTORICO_ESTADOS_FINANCIEROS) conteo_tabla_original,
(select count (*) from HISTORICO_ESTADOS_FINANCIEROS_CATALOGOS) conteo_vista,
(select count (nombre_entidad) from HISTORICO_ESTADOS_FINANCIEROS_CATALOGOS) conteo_nombre_entidad_vw,
(select count (descripcion) from HISTORICO_ESTADOS_FINANCIEROS_CATALOGOS) conteo_descripcion_vw;
```


```sql
/* Esta vista te permitirá obtener tanto la cartera total como el total de tarjetas de crédito (TDC) por entidad.
Consideraciones importantes:
- Esta consulta solo incluirá la información que esté presente en ambas tablas.
- Dado que el reporte de TDC es bimestral, los datos obtenidos se limitarán a los bimestres disponibles en ambas fuentes.
- Con esta información, podrás identificar qué institución tiene el mayor número de TDC colocadas y cuál posee el mayor volumen de saldo en su cartera. */

drop view if exists PORTAFOLIO_CASOS_SALDO_TOTAL_TDC;
CREATE VIEW PORTAFOLIO_CASOS_SALDO_TOTAL_TDC as
select a.*, b.total as total_tarjetas 
	from  (select periodo, entidad, nombre_entidad,  valor 
	from HISTORICO_ESTADOS_FINANCIEROS_CATALOGOS
	where descripcion = 'Tarjeta de crédito'  and saldo = 133 -- Consolidado
	and  idtema = 1 and nivel = 4 and  orden = 40) as A
	JOIN PUBLIC.TDC_CANTIDAD_TOTAL as B
	ON a.entidad = b.cve_institucion and a.periodo = b.cve_periodo
	where periodo >= 201901; -- 5 años de informacion
```

```sql
/* Se detectaron inconsistencias: hay claves de instituciones en los catálogos de la CNBV que no están presentes en la tabla 
de Tarjetas de Crédito (TDC), y viceversa, claves de institución en la tabla de TDC que no aparecen en los estados de resultados.*/ 
SELECT distinct entidad,cve_institucion ,validacion_nombre 
	FROM PORTAFOLIO_CASOS_SALDO_TOTAL_TDC_VALIDA WHERE ENTIDAD IS NULL and cve_periodo >= 201901;

-- Para CI banco se tiene informacion de tarjetas en el 2012, pero no en estados de resultados
select * from PORTAFOLIO_CASOS_SALDO_TOTAL_TDC_VALIDA where validacion_nombre is not null and entidad is null;

-- Validar si hay información de instituciones con saldo en tarjeta de credito pero sin información en numeros de plasticos
-- Solo si es bimestre se puede considerar como incorrecto

SELECT periodo,count(distinct entidad) FROM PORTAFOLIO_CASOS_SALDO_TOTAL_TDC_VALIDA WHERE total_tarjetas IS NULL and valor > 0
and periodo >= 201901
and periodo <= (select max(cve_periodo) from TDC_CANTIDAD_TOTAL)
and nombre_entidad not in ('G-7','Comercial Pequeño','Total Banca Múltiple','Inversión y Servicios Financieros','Comercial Mediano','Créditos a los Hogares')
and EXTRACT(MONTH FROM TO_DATE(periodo::varchar, 'YYYYMMDD')::DATE) not in (1,3,5,7,9,11)
group by 1order by 1;

-- Inbursa,  Santander, Banamex, presentan incositencias entre el reporte de estados financieros y el de tarjetas totales
select distinct  nombre_entidad FROM PORTAFOLIO_CASOS_SALDO_TOTAL_TDC_VALIDA 
WHERE total_tarjetas IS NULL and valor > 0  and nombre_entidad not in ('G-7','Comercial Pequeño','Total Banca Múltiple','Inversión y Servicios Financieros','Comercial Mediano','Créditos a los Hogares')
and EXTRACT(MONTH FROM TO_DATE(periodo::varchar, 'YYYYMMDD')::DATE) not in (1,3,5,7,9,11) 
and periodo >= 201901
and periodo <= (select max(cve_periodo) from TDC_CANTIDAD_TOTAL)
order by periodo desc;
```


```sql
/*Crear una vista para validar si existen nombres de entidades en el catálogo de entidades  que no se encuentren en la tabla de estados financieros.*/
drop view if exists VALIDACION_NOMBRES_ENTIDADES;
CREATE VIEW VALIDACION_NOMBRES_ENTIDADES as
select  distinct a.entidad,a.nombre_entidad , c.entidad validacion_entidad,c.nombre_entidad validacion_nombre 
from  (select periodo, entidad, nombre_entidad,  valor 
	from HISTORICO_ESTADOS_FINANCIEROS_CATALOGOS
	where descripcion = 'Tarjeta de crédito'  and saldo = 133 -- Consolidado
	and  idtema = 1 and nivel = 4 and  orden = 40) as A
	RIGHT JOIN PUBLIC.CATALOGO_ENTIDADES AS C
	ON a.entidad = c.entidad; 
	
select * from VALIDACION_NOMBRES_ENTIDADES where entidad is null;
```


```sql
/*Crea una función trigger que impida la inserción de una entidad si no está presente en el catálogo de entidades.*/

-- Función que se ejecutará como parte del trigger para validar la CVE_INSTITUCION
CREATE OR REPLACE FUNCTION validar_cve_institucion_existente()
RETURNS TRIGGER AS $$
BEGIN
    -- Verifica si la CVE_INSTITUCION del nuevo registro existe en CATALOGO_ENTIDADES
    IF NOT EXISTS (SELECT 1 FROM PUBLIC.CATALOGO_ENTIDADES WHERE ENTIDAD = NEW.CVE_INSTITUCION) THEN
        -- Si no existe, lanza un error y evita la inserción
        RAISE EXCEPTION 'Error: La CVE_INSTITUCION "%" no existe en el CATALOGO_ENTIDADES.', NEW.CVE_INSTITUCION;
    END IF;
    -- Si la CVE_INSTITUCION existe, permite la inserción
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Crear el trigger que llama a la función/procedimiento antes de cada inserción en TDC_CANTIDAD_TOTAL
CREATE TRIGGER trg_validar_cve_institucion
BEFORE INSERT ON PUBLIC.TDC_CANTIDAD_TOTAL
FOR EACH ROW
EXECUTE PROCEDURE validar_cve_institucion_existente(); 

-- Ejemplo de Insert No Válido
INSERT INTO PUBLIC.TDC_CANTIDAD_TOTAL (CVE_INSTITUCION, CVE_PERIODO, TOTAL) VALUES ('INST004', 202301, 200.75);
-- Esto debería fallar y mostrar el mensaje de error:
-- ERROR: La CVE_INSTITUCION "INST004" no existe en el CATALOGO_ENTIDADES.
```