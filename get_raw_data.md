## Get raw data 

SQL query to get raw data (`iv` and `snow`)


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

Exportamos los datos al archivo `./data/raw_iv.csv`


