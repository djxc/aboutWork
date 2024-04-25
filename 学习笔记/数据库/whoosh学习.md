# whoosh学习
whoosh是用python开发的文本索引与查询工具，类似于ElasticSearch。
- 1、基本概念
  - 1.1 Index，索引，对应数据库表，可以创建以及打开（open_dir）
  - 1.2 schema，模式类似于数据库表结构，创建索引时仅需要创建schema一次。
  - 1.3 FileStorage，文件保存类，用来将索引数据保存在磁盘上。
  - 1.4 IndexWriter，索引写入类，通过Index类获取IndexWriter类，通过add_document方法写入文档。通过commit方法将文档写入索引。
  - 1.5 Searcher，当索引创建并写入文档之后就可以进行查询。searcher类也是通过index获取到的。