# 使用JAVA操作ES

## 创建工程

创建maven工程，在`pom.xml`中添加`ES`相关依赖

```xml
<dependencies>  
    <dependency>  
        <groupId>org.elasticsearch.client</groupId>  
        <artifactId>elasticsearch-rest-high-level-client</artifactId>  
        <version>7.10.0</version>  
    </dependency>  
</dependencies>
```

## 操作

### 连接ES

```java
public static void main(String[] args) throws Exception {  

	
	RestHighLevelClient esClient = new RestHighLevelClient(  
	        RestClient.builder(new HttpHost("192.168.219.130", 9200, "http")  
	        )  
	); 

	// 关闭连接
    esClient.close();  
  
}
```

### 索引操作

**创建索引**

```java
public void create(RestHighLevelClient client,String index) throws IOException {  
    //获取ES的索引对象  
    IndicesClient indices = client.indices();  
    CreateIndexRequest createIndexRequest = new CreateIndexRequest(index); 
     
    //设置别名  
    Alias alias = new Alias("alias_"+index);  
    createIndexRequest.alias(alias);  
  
    //设置settings  
    createIndexRequest.settings(Settings.builder()  
            .put("index.number_of_shards",3)  
            .put("index.number_of_replicas",1)  
        );  
        
    //设置mapping  
    String source = "" +  
            "{\n" +  
            "    \"properties\": {\n" +  
            "      \"iid\":{\n" +  
            "        \"type\": \"text\"\n" +  
            "      },\n" +  
            "      \"name\":{\n" +  
            "        \"type\": \"text\"\n" +  
            "      },\n" +  
            "      \"addr\":{\n" +  
            "        \"type\": \"text\"\n" +  
            "      }\n" +  
            "    }\n" +  
            "  }\n" +  
            "}"; 
 
    createIndexRequest.mapping("mapping", source, XContentType.JSON);  

    CreateIndexResponse response = indices
	    .create(createIndexRequest, RequestOptions.DEFAULT);  
    System.out.println(
	    response.isAcknowledged() ? index + "创建成功" : index + "创建失败"
	);  
}
```

在kibana中查看

![](../../markdown_img/Pasted%20image%2020221111164237.png)

**删除索引**

```java
public void delete(RestHighLevelClient client, String index) throws IOException {  
    IndicesClient indices = client.indices();  
    DeleteIndexRequest deleteIndexRequest = new DeleteIndexRequest(index);  
    deleteIndexRequest.timeout("2m");  
  
    AcknowledgedResponse response = indices
	    .delete(deleteIndexRequest, RequestOptions.DEFAULT);  
	
    System.out.println(
	    response.isAcknowledged() ? index + "删除成功" : index + "删除失败"
	);  
  
}
```

此时在控制台查看

![](../../markdown_img/Pasted%20image%2020221111164442.png)

**更新索引**

更新前

![](../../markdown_img/Pasted%20image%2020221111165206.png)

```java
public void update(RestHighLevelClient client, String index) throws IOException {  
    //获取ES的索引对象  
    IndicesClient indices = client.indices();  
    UpdateSettingsRequest request = new UpdateSettingsRequest(index);  
  
    // 修改 setting    
    String settingKey = "index.number_of_replicas";  
    int settingValue = 0;  
    Settings.Builder settingsBuilder =  
            Settings.builder()  
                    .put(settingKey, settingValue);  
    request.settings(settingsBuilder);  
  
    AcknowledgedResponse  response = indices
	    .putSettings(request, RequestOptions.DEFAULT);  
    System.out.println(
	    response.isAcknowledged() ? index + "修改成功" : index + "修改失败"
	);  
}
```

更新后

![](../../markdown_img/Pasted%20image%2020221111170113.png)

**获取Mappings**

```java
public void getMappings(RestHighLevelClient client, String index) throws IOException {  
    IndicesClient indices = client.indices();  
    GetMappingsRequest request = new GetMappingsRequest();  
    request.indices(index);  
    GetMappingsResponse response = indices.getMapping(request, RequestOptions.DEFAULT);  
    System.out.println(response);  
}
```

![](../../markdown_img/Pasted%20image%2020221111170721.png)


### 文档操作

**新增文档**

先写一个生成数据的方法

```java
public static List<String> getData() {  
    Random r = new Random();  
    String[] firstName = {"李", "王", "张", "陈", "牛", "江", "宋", "刘"};  
    String[] lastName = {"可", "汉", "方", "新", "晴", "成", "辖"};  
    String[] addrs = {  
            "北京", "天津", "河北", "山西", "内蒙", "辽宁", "吉林", "黑龙江",  
            "上海", "江苏", "浙江", "安徽", "福建", "江西", "山东", "河南",  
            "湖北", "湖南", "广东", "广西", "海南", "重庆", "四川", "贵州",  
            "云南", "西藏", "陕西", "甘肃", "青海", "宁夏", "新疆", "台湾",  
            "香港", "澳门"  
    };  
    List<String> list = new ArrayList<String>();  
    for (int i = 0; i < 20; i ++) {  
        String iid = String.valueOf(r.nextInt(100));  
        String name = firstName[r.nextInt(firstName.length)] + lastName[r.nextInt(lastName.length)];  
        String addr = addrs[r.nextInt(addrs.length)];  
        String info = String.format("{" +  
                "\"iid\":\"%s\"," +  
                "\"name\":\"%s\"," +  
                "\"addr\":\"%s\"" +  
                "}", iid, name, addr);  
        list.add(info);  
    }  
    return list;  
}
```

在`doc`类中创建添加数据方法

```java
public void add(RestHighLevelClient client, String index, String id, String source) throws IOException {  
    IndexRequest indexRequest = new IndexRequest(index);  
    //2.   设置文档ID。  
    indexRequest.id(id);  
  
    indexRequest.source(source, XContentType.JSON);  
  
    IndexResponse response = client.index(indexRequest, RequestOptions.DEFAULT);  
  
	System.out.println(response.getResult().toString());
}
```

添加数据

```java
for (int i = 0; i < data.size(); i++){  
    doc.add(esClient, "index001", String.valueOf(i), data.get(i));  
}
```

在控制台查看

![](../../markdown_img/Pasted%20image%2020221112001002.png)

**修改文档**

基于 doc 修改

```java
public void update(RestHighLevelClient client, String index, String id, Map<String, Object> doc) throws IOException {  
    UpdateRequest updateRequest = new UpdateRequest(index, id);  
  
    updateRequest.doc(doc);  
   
    UpdateResponse response = client.update(updateRequest, RequestOptions.DEFAULT);  
    System.out.println(response.getResult().toString());  
}
```

调用

```java
// 新数据
Map<String, Object> newSource = new HashMap<String, Object>();  
newSource.put("name", "张三");  
newSource.put("iid", "9999");  
newSource.put("addr", "武汉华夏理工学院");  
  
doc.update(esClient, "index001", "5", newSource);
```


在控制台查看结果

![](../../markdown_img/Pasted%20image%2020221112002329.png)

**删除文档**

```java
public void delete(RestHighLevelClient client, String index, String id) throws IOException {  
    DeleteRequest request = new DeleteRequest(index, id);  
  
    DeleteResponse response = client
	    .delete(request, RequestOptions.DEFAULT);  
    System.out.println(response.getResult().toString());  
  
}
```

在控制台查看

![](../../markdown_img/Pasted%20image%2020221112002750.png)


**搜索**

简单搜索

```java
public void search(RestHighLevelClient client, String index, String key, String value) throws IOException {  
    SearchRequest searchRequest = new SearchRequest(index);  
  
    SearchSourceBuilder sourceBuilder = new SearchSourceBuilder()  
            .query(QueryBuilders.termQuery(key, value));  
    searchRequest.source(sourceBuilder);  
  
  
    SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);  
  
    for (SearchHit hit : searchResponse.getHits()){  
        System.out.println(hit);  
    }  
}
```

```java
// main
doc.search(esClient, "index001", "name", "李");
```

结果


![](../../markdown_img/Pasted%20image%2020221112005457.png)

控制台中搜索

![](../../markdown_img/Pasted%20image%2020221112005841.png)