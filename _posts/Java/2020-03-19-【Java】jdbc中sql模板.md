> 参考https://www.cnblogs.com/wang-meng/p/5525389.html

### 匹配多个参数

```java
public static List<Title> selectTitleList(String type, String author, String time)
{
	List<Title> list = new ArrayList<>();	//返回值
	List params = new ArrayList();			//参数列表
	StringBuilder sql = new StringBuilder("SELECT * FROM article WHERE 1=1");
	if (type!=null && !type.isEmpty())
	{
		sql.append(" AND Type LIKE ?");
		params.add("%"+type+"%");
	}
    if (author!=null && !author.isEmpty())
    {
        sql.append(" AND Author LIKE ?");
        params.add("%"+author+"%");
    }
    if (time!=null && !time.isEmpty())
    {
        sql.append(" AND Time LIKE ?");
        params.add("%"+time+"%");
    }
	sql.append(" ORDER BY PublishTime DESC");
	
	list = new QueryRunner().query(connection, sql.toString(), new BeanListHandler<>(Title.class), params.toArray());//找不到会返回null
	
	return list;
}
```

