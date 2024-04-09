# MySql 命令

### 自定义排序

> 语法规则：FIELD(value,str1,str2,str3,str4)
>
> value分别与str1，str2，str3，str4比较

>示例：SELECT * FROM books WHERE `books`.`author` IN ('李雷','韩梅梅','安华') ORDER BY FIELD(author, '李雷','韩梅梅','安华'); 

