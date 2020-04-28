# Sqoop

컬럼 두개로 나눠서 export

```
sqoop export -connect jdbc:oracle:thin:@70.12.115.56:1521:xe -username shop -password shop -export-di/bigtest/test4/part-r-00000 -table comment_result -columns "word,cnt" --fields-terminated-by '\t'
```

오라클 DB에서 Hadoop으로 import

```
sqoop import -connect jdbc:oracle:thin:@70.12.115.56:1521:xe -username shop -password shop -query "select prd_no,prd_nm from tb_product" -target-dir /sqoop_where -m 1
```

```
sqoop import -connect jdbc:oracle:thin:@70.12.115.56:1521:xe -username shop -password shop -table tb_product -target-dir /mywork/sqoop2/ -as-textfile -columns "prd_no,prd_nm"-split-by prd_no -m 2
```

