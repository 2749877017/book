# mybatis的常用标签

## 一.if标签

当需要动态生成where条件时，可以使用if标签：

```xml
<select id="findActiveBlogWithTitleLike"
     resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = ‘ACTIVE’
  <if test="title != null and title ！= ''">
    AND title like #{title}
  </if>
</select>
```

当if中的条件成立时，语句块中的内容将会被加到sql中，多个查询调价可以叠加多个 if标签

## 二.choose, when, otherwise标签

当需要从多个条件中选择一项时，可以使用choose元素：

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’ 
 <choose>
    <when test="title != null">
      AND title like #{title}  
  </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}   
 </when>
    <otherwise>
      AND featured = 1    
</otherwise>
  </choose>
</select>se>
</select>
```

choose标签只会将第一个成立的<when>标签中的语句加到sql中，如果所有的<when>都不成立，则取otherwise中的语句

## 三.where标签

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
         state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

<where>会自动去除多余的AND 或者OR

## 四.set标签

当update语句中set更新相关字段时，也会遇到类似的问题，生成的SQL语句可能会多或者少一个逗号

```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```

<set>会自动去除多余的逗号

## 五.trim标签

trim标签使用
1、trim 有四个属性
2、prefix，suffix 表示在trim标签包裹的部分的前面或者后面添加内容（注意：是没有        prefixOverrides，suffixOverrides的情况下）
3、如果有prefixOverrides，suffixOverrides 表示覆盖Overrides中的内容。
4、如果只有prefixOverrides，suffixOverrides 表示删除。

其实，<where>和<set>，都是 通过<trim>实现的：

```xml
<update id="testTrim" parameterType="com.mybatis.pojo.User">
        update user
        <trim prefix="set" suffixOverrides=",">
            <if test="cash!=null and cash!=''">cash= #{cash},</if>
            <if test="address!=null and address!=''">address= #{address},</if>
        </trim>
        <where>id = #{id}</where>
    </update>
```

在被trim包裹的前面放上set ，去掉后面多余的逗号

```xml
  <update id="testTrim" parameterType="com.mybatis.pojo.User">
        update user
        set
        <trim suffixOverrides="," suffix="where id = #{id}">
            <if test="cash!=null and cash!=''">cash= #{cash},</if>
            <if test="address!=null and address!=''">address= #{address},</if>
        </trim>
    </update>
```

在被trim包裹的后面放入where的条件，去掉后面多余的逗号

```xml
<select id="selectByid" parameterType="HashMap" resultType="com.neuedu.Student">
select * from student
<trim prefix="where" prefixOverrides="and | or">
    <if test="sex!=null and sex!=''">sex=#{sex}</if>
    <if test="class!=null and class!=''">and class=#{class}</if>
</trim>
</select>
```

## 六.foreach标签

动态 SQL 的另外一个常用的操作需求是对一个集合进行遍历，通常是在构建 IN 条件语句的时候。比如：

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```

*foreach* 元素的功能非常强大，它允许你指定一个集合，声明可以在元素体内使用的集合项（item）和索引（index）变量。它也允许你指定开头与结尾的字符串以及在迭代结果之间放置分隔符。这个元素是很智能的，因此它不会偶然地附加多余的分隔符。  

注意 你可以将任何可迭代对象（如 List、Set 等）、Map 对象或者数组对象传递给 *foreach* 作为集合参数。当使用可迭代对象或者数组时，index 是当前迭代的次数，item 的值是本次迭代获取的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。
