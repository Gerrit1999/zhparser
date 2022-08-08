# 编译方法

1. 下载已经用vs改进的工程：https://github.com/Gerrit1999/zhparser

2. 使用vs2010或以上的版本打开`zhparser\scws\win32\scws.sln`

![](https://ciirtuuew6.feishu.cn/space/api/box/stream/download/asynccode/?code=OGE1NDVlOGY1YWFlYWIyNTkyYWM4ZDQ2OTE1MjY1MDFfQjZvT29JM1pQOTdyWWlFV3dndGdwQVFtb3lFQkVHbmRfVG9rZW46Ym94Y25sbnNpcXh2NmhrbENCN04zWWhtajliXzE2NTk5NTI2NDc6MTY1OTk1NjI0N19WNA)

3. 打开`zhparser\zhparser\zhparser.sln`

![](https://ciirtuuew6.feishu.cn/space/api/box/stream/download/asynccode/?code=MTM4YTMyMmFkNzgzODlkN2E4MTE4NWY3ODllNWU4NTVfUHJDdTlvcnRUb3Exendqb29Da2M4WHFpaUhPZWZjUmJfVG9rZW46Ym94Y25nUHp2blNnMWVCRDJiVXhlVUhtdnJoXzE2NTk5NTI2NDc6MTY1OTk1NjI0N19WNA)

4. 此时可以看到vs也会把scws的工程加载进来

![](https://ciirtuuew6.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDY2YThhNTExZDMyNDdkNTgwZGM5Mzc4NzgwNWQ1OGNfSGk2c25zRlphQldrUkFXSExMeXpMazBiS1ZZZ1NmTUNfVG9rZW46Ym94Y25wR1FYUkJIU3RSOWhWZDlBbUd5S2xiXzE2NTk5NTI2NDc6MTY1OTk1NjI0N19WNA)

5. 修改编译配置支持x64

![](https://ciirtuuew6.feishu.cn/space/api/box/stream/download/asynccode/?code=NTFjOTFkNmUzODRmZTMwZDFjNTUxMDdlOTU4ZmRhYzJfWjhYb3NyVzc4bUNJQmd4eFJ6SVZqTkR4bUZ2dGxLUVNfVG9rZW46Ym94Y255cWs2cmxyQTFNbEhreG5YYW5TcG9oXzE2NTk5NTI2NDc6MTY1OTk1NjI0N19WNA)

6. 先编译scws，生成`libscws.lib`静态库。
   
   1. 编译前，在libscws工程 `属性 --> C/C++ --> 常规 --> 附加包含目录`输入（路径要根据依赖软件的位置相应调整）：
      
      E:\zhparser\scws\libscws;D:\PostgreSQL\10\include;D:\PostgreSQL\10\include\server;D:\PostgreSQL\10\include\server\utils;D:\PostgreSQL\10\include\server\port;%(AdditionalIncludeDirectories)

7. 右键libscws工程，点击生成
   
   ![](https://ciirtuuew6.feishu.cn/space/api/box/stream/download/asynccode/?code=YTNlNmZlOWI3ZDhkNTI1NjU1YzFmOGVjM2Y5MTZkNGFfMVRDWGVpYTNKeTIzWmhHSzVvV3ZzQWNzMmExaUJ5T05fVG9rZW46Ym94Y25kVjZyeWp5Z21EVktDQkRZbFl1aE81XzE2NTk5NTI2NDc6MTY1OTk1NjI0N19WNA)

8. 编译zhparser
   
   1. 将刚才编译完成的`zhparser\scws\Releas\libscws.lib`文件放到`zhparser\zhparser`根目录下
   
   2. 编译前，在zhparser工程 `属性 --> C/C++ --> 常规 --> 附加包含目录`输入（路径要根据依赖软件的位置相应调整）：
      
      E:\zhparser\scws\libscws;D:\PostgreSQL\10\include;D:\PostgreSQL\10\include\server;D:\PostgreSQL\10\include\server\utils;D:\PostgreSQL\10\include\server\port;D:\PostgreSQL\10\include\server\port\win32;%(AdditionalIncludeDirectories)

9. 在`属性 --> 链接器 --> 常规 --> 附加库目录`输入（路径要根据依赖软件的位置相应调整）：
   
      D:\PostgreSQL\10\lib;E:\zhparser\zhparser;%(AdditionalLibraryDirectories)

10. 右键zhparser工程，点击生成

11. 将编译完成的`zhparser\zhparser\x64\Release\zhparser.dll`文件复制到`D:\PostgreSQL\10\lib`中

12. 将`zhparser/zhparser.control`、`zhparser/*.sql`复制到`D:\PostgreSQL\10\share\extension`

![](https://ciirtuuew6.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDg5ZTE4ZjgyNTNlNmY0ZDU2YzVlZDJiY2RjNWI3MDZfSjU4WHhxWEhsWWdyZUZONU8zQTE5S01ySkZQdmpUSUdfVG9rZW46Ym94Y24xc01oU1NKUXJyTkhibkpXSVNTQWNlXzE2NTk5NTI2NDc6MTY1OTk1NjI0N19WNA)

10. 将`dict.utf8.xdb`、`rules.utf8.ini`复制到`D:\PostgreSQL\10\share\tsearch_data`

# 使用

1. 进入数据库，创建extension

```sql
CREATE EXTENSION zhparser
```

2. 添加配置

```sql
CREATE TEXT SEARCH CONFIGURATION zh (PARSER = zhparser);ALTER TEXT SEARCH CONFIGURATION zh ADD MAPPING FOR n,v,a,i,e,l WITH simple;

#查询已有的解析器
demo-# \dFp         
List of text search parsers   Schema   |   Name   |     Description     
------------+----------+--------------------- 
pg_catalog | default  | default word parser 
public     | zhparser | 
(2 rows)
```
