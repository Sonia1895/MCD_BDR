# Código usado para hacer modificaciones en las diferentes tablas de la base de datos

```sql

-- En la tabla  de rangos de perdida esperada, se agrega una columna para convertir a texto el número de rango

alter table PUBLIC.TDC_RANGO_PERDIDA_ESPERADA add rango_perdida_esperado_t varchar(10);

-- Modifcar la columna previamente agregada
alter table PUBLIC.TDC_RANGO_PERDIDA_ESPERADA alter column  rango_perdida_esperado_t type varchar(20);

update PUBLIC.TDC_RANGO_PERDIDA_ESPERADA
set rango_perdida_esperado_t = case
when rango_perdida_esperada = 1 then '0%'
when rango_perdida_esperada = 2 then '(0 - 1%]'
when rango_perdida_esperada = 3 then '(1 - 2%]'
when rango_perdida_esperada = 4 then '(2 - 3%]'
when rango_perdida_esperada = 5 then '(3 - 4%]'
when rango_perdida_esperada = 6 then '(4 - 5%]'
when rango_perdida_esperada = 7 then '(5 - 6%]'
when rango_perdida_esperada = 8 then '(6 - 7%]'
when rango_perdida_esperada = 9 then '(7 - 8%]'
when rango_perdida_esperada = 10 then '(8 - 9%]'
when rango_perdida_esperada = 11 then '(9 - 10%]'
when rango_perdida_esperada = 12 then '(10 - 11%]'
when rango_perdida_esperada = 13 then '(11 - 12%]'
when rango_perdida_esperada = 14 then '(12 -13%]'
when rango_perdida_esperada = 15 then '(13 - 14%]'
when rango_perdida_esperada = 16 then '(14 -15%]'
when rango_perdida_esperada = 17 then '(15 -16%]'
when rango_perdida_esperada = 18 then '(16 - 17%]'
when rango_perdida_esperada = 19 then '(17 - 18%]'
when rango_perdida_esperada = 20 then '(18 - 19%]'
when rango_perdida_esperada = 21 then '(19 -20%]'
when rango_perdida_esperada = 22 then '(20 - 21%]'
when rango_perdida_esperada = 23 then '(21 - 22%]'
when rango_perdida_esperada = 24 then '(22 - 23%]'
when rango_perdida_esperada = 25 then '(23 - 24%]'
when rango_perdida_esperada = 26 then '(24 - 25%]'
when rango_perdida_esperada = 27 then '(25 - 26%]'
when rango_perdida_esperada = 28 then '(26 - 27%]'
when rango_perdida_esperada = 29 then '(27 - 28%]'
when rango_perdida_esperada = 30 then '(28 - 29%]'
when rango_perdida_esperada = 31 then '(29 - 30%]'
when rango_perdida_esperada = 32 then '(30 - 35%]'
when rango_perdida_esperada = 33 then '(35 - 40%]'
when rango_perdida_esperada = 34 then '(40 - 45%]'
when rango_perdida_esperada = 35 then '(45 - 50%]'
when rango_perdida_esperada = 36 then '(50 - 55%]'
when rango_perdida_esperada = 37 then '(55 -60%]'
when rango_perdida_esperada = 38 then '(65 - 70%]'
when rango_perdida_esperada = 39 then '(70 -75%]'
when rango_perdida_esperada = 40 then '(75 - 80%]'
when rango_perdida_esperada = 41 then '(80 - 85%]'
when rango_perdida_esperada = 42 then '(85 - 90%]'
when rango_perdida_esperada = 43 then '(90 -95%]'
when rango_perdida_esperada = 44 then '(95 - 100%]'
when rango_perdida_esperada = 45 then '+ de 100%'
end;

-- Validar lógica de update
select distinct rango_perdida_esperada,rango_perdida_esperado_t
	from PUBLIC.TDC_RANGO_PERDIDA_ESPERADA
	order by 1,2;

/* En la tabla de distribución de tarjetas por porcentaje de pago realizado contra pago para no generar intereses, 
agregaremos una columna con texto para los rangos */

alter table PUBLIC.TDC_RANGO_PAGOS add rango_pago_realizado_vs_no_intereses_t varchar(10);

-- Eliminar columna 
alter table PUBLIC.TDC_RANGO_PAGOS drop column  rango_pago_realizado_vs_no_intereses_t;

--Agregar columna con largo de varchar correcto 
alter table PUBLIC.TDC_RANGO_PAGOS add rango_pago_realizado_vs_no_intereses_t varchar(20);

update PUBLIC.TDC_RANGO_PAGOS
set rango_pago_realizado_vs_no_intereses_t = case
when rango_pago_realizado_vs_no_intereses = 1 then '0%'
when rango_pago_realizado_vs_no_intereses = 2 then '(0 - 1%]'
when rango_pago_realizado_vs_no_intereses = 3 then '(1 - 2%]'
when rango_pago_realizado_vs_no_intereses = 4 then '(2 - 3%]'
when rango_pago_realizado_vs_no_intereses = 5 then '(3 - 4%]'
when rango_pago_realizado_vs_no_intereses = 6 then '(4 - 5%]'
when rango_pago_realizado_vs_no_intereses = 7 then '(5 -6%]'
when rango_pago_realizado_vs_no_intereses = 8 then '(6 - 7%]'
when rango_pago_realizado_vs_no_intereses = 9 then '(7 - 8%]'
when rango_pago_realizado_vs_no_intereses = 10 then '(8 - 9%]'
when rango_pago_realizado_vs_no_intereses = 11 then '(9 - 10%]'
when rango_pago_realizado_vs_no_intereses = 12 then '(10 - 11%]'
when rango_pago_realizado_vs_no_intereses = 13 then '(11 - 12%]'
when rango_pago_realizado_vs_no_intereses = 14 then '(12 -13%]'
when rango_pago_realizado_vs_no_intereses = 15 then '(13 - 14%]'
when rango_pago_realizado_vs_no_intereses = 16 then '(14 -15%]'
when rango_pago_realizado_vs_no_intereses = 17 then '(15 - 20%]'
when rango_pago_realizado_vs_no_intereses = 18 then '(20 - 25%]'
when rango_pago_realizado_vs_no_intereses = 19 then '(25 - 30%]'
when rango_pago_realizado_vs_no_intereses = 20 then '(30 - 35%]'
when rango_pago_realizado_vs_no_intereses = 21 then '(35 - 40%]'
when rango_pago_realizado_vs_no_intereses = 22 then '(40 - 45%]'
when rango_pago_realizado_vs_no_intereses = 23 then '(45 - 50%]'
when rango_pago_realizado_vs_no_intereses = 24 then '(50 - 60%]'
when rango_pago_realizado_vs_no_intereses = 25 then '(60 - 70%]'
when rango_pago_realizado_vs_no_intereses = 26 then '(70 - 80%]'
when rango_pago_realizado_vs_no_intereses = 27 then '(80 - 90%]'
when rango_pago_realizado_vs_no_intereses = 28 then '(90 - 100%]'
when rango_pago_realizado_vs_no_intereses = 29 then '+ de 100%'
when rango_pago_realizado_vs_no_intereses = 999 then 'Otros'
end;

-- Validar lógica de update
select distinct rango_pago_realizado_vs_no_intereses,rango_pago_realizado_vs_no_intereses_t
	from PUBLIC.TDC_RANGO_PAGOS
	order by 1,2;
```

# Uso de subconsultas
```sql

--¿Hay registros en 0 o vacíos en las diferentes variables de la tabla de estados financieros?

select
(select count(*) from PUBLIC.HISTORICO_ESTADOS_FINANCIEROS where SECTOR <= 0) Casos_SECTOR_0,
(select count(*) from PUBLIC.HISTORICO_ESTADOS_FINANCIEROS where IDCONCEPTO <= 0) Casos_IDCONCEPTO_0,
(select count(*) from PUBLIC.HISTORICO_ESTADOS_FINANCIEROS where ENTIDAD = '') Casos_ENTIDAD_vacio,
(select count(*) from PUBLIC.HISTORICO_ESTADOS_FINANCIEROS where PERIODO <= 0) Casos_PERIODO_0,
(select count(*) from PUBLIC.HISTORICO_ESTADOS_FINANCIEROS where SALDO <= 0) Casos_SALDO_0,
(select count(*) from PUBLIC.HISTORICO_ESTADOS_FINANCIEROS where VALOR <= 0) Casos_VALOR_0;
```

# Reporte de hallazgos
Al utilizar las sentencias UPDATE y ALTER TABLE en SQL, encontramos hallazgos clave sobre la manipulación de datos y la estructura de las tablas, respectivamente. UPDATE es crucial para modificar registros existentes, permitiéndonos corregir datos, actualizar estados o realizar cálculos sobre valores ya almacenados sin afectar la estructura de la tabla. Por otro lado, ALTER TABLE es indispensable para cambiar el diseño de la tabla, ya sea añadiendo, eliminando o modificando columnas (como su tipo de dato o tamaño), ajustando restricciones o alterando el nombre de la tabla misma. La principal diferencia y, por ende, el hallazgo más importante, es que UPDATE opera a nivel de datos dentro de las filas existentes, mientras que ALTER TABLE impacta directamente la definición y composición de la tabla, con implicaciones en la forma en que se almacenan y se acceden a los datos de ahí en adelante.