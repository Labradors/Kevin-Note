---

Java后端学习问题记录

---

- Spring3+Mybatis3集成

```xml
org.springframework.beans.factory.BeanDefinitionStoreException: Failed to read candidate component class: file [/Users/kevin/Project/Eclipse/MybatisRelativeSpring/bin/tech/jiangtao/mybatis/mapper/UserMapper.class]; nested exception is java.lang.IllegalArgumentException
```

卧槽，答案竟然是不支持Java8,这他妈的是啥版本。