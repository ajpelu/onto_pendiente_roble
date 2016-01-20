## Get raw data 

SQL query to get raw data (`iv` and `snow`)

> ndvi 

```sql 
SELECT
iv.ano,
iv.ndvi_i_invierno,
iv.ndvi_i_primavera,
iv.ndvi_i_verano,
iv.ndvi_i_otono,
iv.iv_malla_modi_id,
aux.nie_malla_modi_id, 
aux_pob.poblacion,
aux_pob.grupo_poblacion

FROM 
public.aux_ms_ontologias_iv_indices as iv, 
public.aux_ms_ontologias_iv_malla_modis_nie_malla_modis as aux,
public.aux_ms_ontologias_iv_malla_modis_completa as aux_pob

WHERE 
iv.iv_malla_modi_id = aux.iv_malla_modi_id AND
iv.iv_malla_modi_id = aux_pob.id;  
```

Export output file to `./data/raw_iv.csv`

> snow 

```sql 
SELECT
s.ano,
s.scd,
s.scod,
s.scmd,
s.nie_malla_modi_id,
s.iv_malla_modi_id,
aux_pob.poblacion,
aux_pob.grupo_poblacion

FROM 
public.aux_ms_ontologias_nie_indicadors as s, 
public.aux_ms_ontologias_iv_malla_modis_completa as aux_pob

WHERE 
s.iv_malla_modi_id = aux_pob.id;  
```

Export output file to `./data/raw_snow.csv`
