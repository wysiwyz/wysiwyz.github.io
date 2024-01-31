---
title: "Spring JPA 與 Hibernete"
date: 2023-12-26T14:13:59+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

##### ORM 物件關係的映射

Object-Relational Mapping的概念是可以將資料表內容做提取，並映射為物件的形式。

缺點：對於複雜的操作（e.g. 多表格的join或查詢），相較於直接下SQL語句，更為綁手綁腳

優點：不用操作繁瑣的SQL，也可以避免惡意注入攻擊

##### JPA (Java Persistence API)

持久化：資料「儲存」與「讀取」的過程，意即將資料從<u>**瞬時狀態**</u>改為<u>**持久狀態**</u>的過程（將資料存進資料庫的過程）

JPA：透過Java將資料儲存到資料庫的API

使用JPA優點：

1. 可以隨時切換想要使用的 ORM 框架

2. 可以使用JPQL (Java Persistence Query Language) 向資料庫下命令：

   JPQL v.s. SQL:

   - SQL在不同資料庫，有不同SQL命令語句
   - JPQL操作對象不著重於資料庫，而是JPA的Entity Object下類似SQL的命令
     （切換不同資料庫，不用特別修改SQL語句）

##### 常見的 ORM 框架：

1. Hibernate - 

   一般使用 Spring Data JPA 就會使用 Hibernate

   Hibernate 可以自動生成SQL 語句，自動執行

   優勢：

   1. 簡化開發_簡化了數據訪問層的開發流程
   2. 自動化查詢_只要方法命名好，就能生成相應的查詢語句
   3. 強大的ORM功能_便於物件和關係數據庫之間的轉換
   4. 數據庫管理_Spring Data JPA 透過Spring框架的數據源管理功能，可方便切換和配置不同數據源

   劣勢：

   1. 性能問題_因為是以ORM的方式訪問數據，如果在大量數據或複雜查詢的應用情境下，可能會有性能上的影響
   2. 靈活性限制_在比較需要高級查詢的場景下有些侷限，需要編寫自定義JPQL

   舉例：

   ```java
   public interface UserRepository extends JpaRepository<User, Long> {
       List<User> findByLastName(String lastName);
   }
   ```

   

2. [MyBatis](https://blog.mybatis.org/) - 

   支援客製化SQL、存儲過程、以及 Advanced Mapping

   可以使用簡單的 xml 或者註解，配置

   優勢：

   1. 靈活性高
   2. 性能優化
   3. 易於集成

   劣勢：

   1. 需要手動編寫SQL語句，開發者必須具備一定的SQL語法以及數據庫知識
   2. 缺發對象關係映射，MyBatis需要開發者手動處理對象和關係數據庫的轉換

   舉例：

   ```java
   public interface UserMapper {
       @Select("SELECT * FROM users WHERE last_name = #{lastName}")
       List<User> findByLastName(@Param("lastName") String lastName);
   }
   ```

   

> Spring Data JPA 更方便，自動化更強，適合用於：
>
> 1. **簡單的CRUD操作**
>    Spring Data JPA 提供自動生成查詢方法的功能，減少手動編寫SQL的工作量
>
> 2. **需要快速集成Spring框架**
>
>    如果框架已經是 Spring，用 Spring Data JPA 比較便於與其它Spring組件協作
>    （e.g. 集成數據訪問層、事務管理、數據來源配置）
>
> 3. **對象關係映射需求較高**
>    業務邏輯涉及複雜的對象關係映射（e.g. 繼承、關聯關係）時，Spring Data JPA可以更好的處理其複雜性
>
> MyBatis 撰寫 sql 語句上更加靈活自由，適用於：
>
> 1. **複雜的查詢需求**
>
>    像是多表格關聯查詢、動態查詢條件的需求，MyBatis提供更好的靈活性
>
> 2. **性能優化要求比較高**
>
>    處理大量數據或者高併發訪問時，MyBatis提供更多的性能優化機會（例如：手動編寫SQL、優化SQL語句、使用緩存等技術手段）
>
> 3. **對SQL語法和數據庫的掌握較好**
>
>    可以更加靈活地控制SQL語句的執行過程，更好的發揮數據庫的特性



參考資料：

[什麼是Spring Data JPA](https://ithelp.ithome.com.tw/articles/10194906)

[Spring Data JPA_spring.io](https://docs.spring.io/spring-data/jpa/reference/#repositories)

[認識Spring Data JPA, JPA, Hibernate](https://ithelp.ithome.com.tw/articles/10217348)

[JPA v.s. Hibernate](https://www.interviewbit.com/blog/jpa-vs-hibernate/)





