# JAVA操作ES聚合查询

通过一层层嵌套实现

可以先写出es控制台的代码，然后通过JAVA  API一层层嵌套实现

聚合搜索

```json
# 查询学生爱好为changge或者tiaowu，按性别分组，并统计各分组内文档数量、年龄的最大、最小、平均、求和等信息，文档详细内容输出1个即可
GET /student/_search
{
  "size": 1,
  "query": {
    "match": {
      "interest": "changge tiaowu"
    }
  },
  "aggregations": {
    "group_sex": {
      "terms": {
        "field": "sex"
      },
      "aggs": {
        "age_agg": {
          "stats": {
            "field": "age"
          }
        }
      }
    }
  }
}
```

对应的JAVA代码

```java
// 查询学生爱好为changge或者tiaowu，
// 按性别分组，并统计各分组内文档数量、年龄的最大、最小、平均、求和等信息，
// 文档详细内容输出1个即可
public static void search_agg(RestHighLevelClient esClient) throws IOException {
	SearchRequest searchRequest = new SearchRequest("stu");
	// 构建器
	SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

	// 查爱好
	MatchQueryBuilder matchQueryBuilder = QueryBuilders.matchQuery("interest", "changge tiaowu");
	// 分组
	TermsAggregationBuilder group_sex = AggregationBuilders.terms("group_sex").field("sex");
	// 聚合计算
	StatsAggregationBuilder statsAggregationBuilder = AggregationBuilders.stats("age_stats").field("age");

	sourceBuilder.aggregation(group_sex);
	sourceBuilder.aggregation(statsAggregationBuilder);

	// 封装
	sourceBuilder.query(matchQueryBuilder);
	sourceBuilder.size(1);
	searchRequest.source(sourceBuilder);

	SearchResponse searchResponse = esClient.search(searchRequest, RequestOptions.DEFAULT);
	System.out.println(searchResponse);
}
```


再如

```json
# 查询学生年龄在18-25之间，且爱好为changge或者tiaowu，且住址不含有1008的文档信息，结果按学号升序排列
GET /student/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "interest": "changge tiaowu"
          }
        },
        {
          "range": {
            "age": {
              "gt": 18,
              "lte": 25
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "addr": "1008"
          }
        }
      ]
    }
  },
  "sort": [
    {
      "stu_no": "asc"
    }
  ]
}
```

对应的java代码

```java
// 查询学生年龄在18-25之间，
// 且爱好为changge或者tiaowu，
// 且住址不含有1008的文档信息，
// 结果按学号升序排列，并对爱好字段进行高亮显示，输出查询结果
public static void search_group(RestHighLevelClient esClient) throws IOException {
	SearchRequest searchRequest = new SearchRequest("stu");

	// 年龄以及爱好
	MatchQueryBuilder matchQueryBuilder = QueryBuilders.matchQuery("interest", "changge tiaowu");
	RangeQueryBuilder rangeQueryBuilder = QueryBuilders.rangeQuery("age").gt(18).lte(25);

	// 住址不包含 1008
	TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("addr", "1008");

	// 添加到 must 和 must-not 中
	BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery();
	boolQueryBuilder.must(matchQueryBuilder);
	boolQueryBuilder.must(rangeQueryBuilder);
	boolQueryBuilder.mustNot(termQueryBuilder);



	// 控制搜索行为
	SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
	// 封装
	sourceBuilder.query(boolQueryBuilder);
	// 排序
	sourceBuilder.sort(new FieldSortBuilder("stu_no").order(SortOrder.ASC));

	// 高亮查询
	HighlightBuilder highlightBuilder = new HighlightBuilder();
	highlightBuilder.preTags("<em>"); // 高亮前缀
	highlightBuilder.postTags("</em>"); // 高亮后缀
	highlightBuilder.fields().add(new HighlightBuilder.Field("interest")); // 高亮字段
	// 添加高亮查询条件到搜索源
	sourceBuilder.highlighter(highlightBuilder);

	searchRequest.source(sourceBuilder);

	SearchResponse searchResponse = esClient.search(searchRequest, RequestOptions.DEFAULT);

	for (SearchHit hit : searchResponse.getHits()){
		System.out.println(hit);
	}
}

// 查询学生爱好为changge或者tiaowu，
// 按性别分组，并统计各分组内文档数量、年龄的最大、最小、平均、求和等信息，
// 文档详细内容输出1个即可
public static void search_agg(RestHighLevelClient esClient) throws IOException {
	SearchRequest searchRequest = new SearchRequest("stu");
	// 构建器
	SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

	// 查爱好
	MatchQueryBuilder matchQueryBuilder = QueryBuilders.matchQuery("interest", "changge tiaowu");
	// 分组
	TermsAggregationBuilder group_sex = AggregationBuilders.terms("group_sex").field("sex");
	// 聚合计算
	StatsAggregationBuilder statsAggregationBuilder = AggregationBuilders.stats("age_stats").field("age");

	sourceBuilder.aggregation(group_sex);
	sourceBuilder.aggregation(statsAggregationBuilder);

	// 封装
	sourceBuilder.query(matchQueryBuilder);
	sourceBuilder.size(1);
	searchRequest.source(sourceBuilder);

	SearchResponse searchResponse = esClient.search(searchRequest, RequestOptions.DEFAULT);
	System.out.println(searchResponse);
}
```