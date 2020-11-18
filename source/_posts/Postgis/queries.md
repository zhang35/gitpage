```sql
SELECT
	object_id,
	ST_AsEWKT ( gps_line ),
	ST_M(ST_EndPoint(gps_line)),
	to_char(to_timestamp(ST_M(ST_EndPoint(gps_line))), 'YYYY-MM-DD HH24:MI:SS')
FROM
	PUBLIC.objecttrajactory 
```

![image-20201019104748370](https://cdn.jsdelivr.net/gh/zhang35/Image@master/img/image-20201019104748370.png)