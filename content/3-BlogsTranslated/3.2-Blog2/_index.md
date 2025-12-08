---
title: "Blog 2"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Accelerate Amazon Redshift Data Lake queries with AWS Glue Data Catalog Column Statistics

by Kalaiselvi Kamaraj, Asser Moustafa, and Mark Lyons | on 01 OCT 2024 in | Amazon Redshift | Amazon Simple Storage Service (S3) | Analytics, Announcements | AWS Big Data | AWS Glue | Best Practices Permalink

Amazon Redshift allows you to query and retrieve structured and semi-structured data from open format files in Amazon S3 data lakes without needing to load data into Amazon Redshift tables. Amazon Redshift extends SQL capabilities to the data lake, enabling you to run analytical queries. Redshift supports many different table data formats such as CSV, JSON, Parquet, ORC, and open table formats like Apache Hudi, Linux Foundation Delta Lake, and Apache Iceberg.

You create Redshift external tables by defining the structure for files, the S3 location of files, and registering them as tables in an external data catalog. The external data catalog can be AWS Glue Data Catalog, the data catalog that comes with Amazon Athena, or your Apache Hive metastore.

Over the past year, Amazon Redshift has added several performance optimizations for data lake queries across many aspects of the query engine such as rewrite, planning, scan execution, and using AWS Glue Data Catalog column statistics. To achieve the best performance for data lake queries with Redshift, you can use AWS Glue Data Catalog column statistics to collect statistics on Data Lake tables. For Amazon Redshift Serverless instances, you'll see scan performance improvements due to increased parallel processing of S3 files, and this happens automatically based on RPUs used.

In this post, we highlight the performance improvements observed using industry-standard TPC-DS benchmarks. The total execution time of the TPC-DS 3 TB benchmark improved by 3x. Some queries in the benchmark achieved speeds up to 12x faster.

## Performance improvements

Over the past year, there have been many performance optimizations made to improve the performance of data lake queries, including:

- Using AWS Glue Data Catalog column statistics and tuning the Redshift optimizer to improve query plan quality.
- Using bloom filters for partition columns.
- Improving scan efficiency for Amazon Redshift Serverless instances through increased parallel processing of files.
- Applying novel query rewrite rules to merge similar scans.
- Faster metadata retrieval from AWS Glue Data Catalog.

To understand the performance gains, we tested performance using the industry-standard TPC-DS benchmark with 3 TB datasets and queries, representing different customer use cases. Performance was tested on a Redshift serverless data warehouse with 128 RPU. In the test, the dataset was stored on Amazon S3 in Parquet format, and AWS Glue Data Catalog was used to manage external databases and tables. Fact tables were partitioned by date column, with each table containing approximately 2,000 partitions. All tables had the row count table property, numRows, set according to spectrum query performance guidelines.

We performed a baseline run on last year's Redshift patch version (patch 172). Then, we ran all TPC-DS queries on the latest patch version (patch 180), including all performance optimizations added over the past year. Next, we used AWS Glue Data Catalog column statistics to compute statistics for all tables and measured the performance improvement when column statistics are available.

Analysis showed that the TPC-DS 3TB Parquet benchmark achieved significant performance gains thanks to these optimizations. Specifically, partitioned Parquet with the latest optimizations achieved 2x faster runtime compared to the previous implementation. Enabling AWS Glue Data Catalog column statistics further improved performance by 3x compared to last year. The graph below illustrates runtime improvements for the entire benchmark (all TPC-DS queries) over the past year, including the additional boost from using AWS Glue Data Catalog column statistics.

![Figure 1: Total execution time improvements for TPC-DS 3T workload](/images/3-BlogsTranslated/3.2-Blog2/figure1-total-runtime-improvements.png)

*Figure 1: Total execution time improvements for TPC-DS 3T workload*

The graph below presents the top queries from the TPC-DS benchmark with the largest performance improvements over the past year, with and without AWS Glue Data Catalog column statistics. You can see that performance improves significantly when statistics are available on AWS Glue Data Catalog (for details on how to collect statistics for Data Lake tables, please refer to optimizing query performance using AWS Glue Data Catalog column statistics). Specifically, multi-join queries will benefit most from AWS Glue Data Catalog column statistics, because the optimizer uses statistics to choose the appropriate join order and distribution strategy.

![Figure 2: Speed-up in TPC-DS queries](/images/3-BlogsTranslated/3.2-Blog2/figure2-speedup-queries.png)

*Figure 2: Speed-up in TPC-DS queries*

Let's discuss some optimizations that have contributed to improved query performance.

## Optimization with table-level statistics

Amazon Redshift is designed to handle large-scale data challenges with superior speed and cost-efficiency. The massively parallel processing (MPP) query engine, AI-powered query optimizer, auto-scaling capabilities, and other advanced features help Redshift excel at searching, aggregating, and transforming petabytes of data.

However, even the most powerful systems can experience performance degradation if they encounter anti-patterns such as extremely inaccurate table statistics, for example, row count metadata.

Without this critical metadata, Redshift's query optimizer can be limited in the number of possible optimizations, especially those related to data distribution during query execution. This can significantly impact overall query performance.

To illustrate, consider a simple query involving an inner join between a large table with billions of rows and a small table with only a few hundred thousand rows.

```sql
select small_table.sellerid, sum(large_table.qtysold)
from large_table, small_table
where large_table.salesid = small_table.listid
 and small_table.listtime > '2023-12-01'
 and large_table.saletime > '2023-12-01'
group by 1 order by 1
```

If executed as-is, with the large table on the right-hand side of the join, the query will lead to sub-optimal performance. This is because the large table will need to be distributed (broadcast) to all Redshift compute nodes to perform the inner join with the small table, as illustrated in the diagram below.

![Figure 3: Inaccurate table statistics lead to limited optimizations and large amounts of data being broadcast between compute nodes for a simple inner join](/images/3-BlogsTranslated/3.2-Blog2/figure3-inaccurate-statistics.png)

*Figure 3: Inaccurate table statistics lead to limited optimizations and large amounts of data being broadcast between compute nodes for a simple inner join*

Now, consider a scenario where table statistics, such as row count, are accurate. This allows the Amazon Redshift query optimizer to make more informed decisions, such as determining the optimal join order. In this case, the optimizer will immediately rewrite the query so the large table is on the left-hand side of the inner join, making the small table the one broadcast to Redshift compute nodes, as illustrated in the diagram below.

![Figure 4: Accurate table statistics lead to a high degree of optimizations and very little data being broadcast between compute nodes for a simple inner join](/images/3-BlogsTranslated/3.2-Blog2/figure4-accurate-statistics.png)

*Figure 4: Accurate table statistics lead to a high degree of optimizations and very little data being broadcast between compute nodes for a simple inner join*

Fortunately, Amazon Redshift automatically maintains accurate table statistics for local tables by running ANALYZE in the background. However, for external tables (data lake tables), using AWS Glue Data Catalog column statistics is recommended when working with Amazon Redshift, as we will discuss in the next section. For more general information about optimizing queries in Amazon Redshift, please refer to the documentation on factors affecting query performance, data redistribution, and Amazon Redshift best practices for designing queries.

## Performance improvements with AWS Glue Data Catalog column statistics

AWS Glue Data Catalog has a feature to compute column-level statistics for Amazon S3-backed external tables. AWS Glue Data Catalog can compute column-level statistics such as NDV, Number of Nulls, Min/Max, and Avg. column width for columns without requiring additional data pipelines.

Amazon Redshift's cost-based optimizer uses these statistics to generate better quality query plans. In addition to using statistics, we also made improvements in cardinality estimations and cost tuning to obtain high-quality query plans, thereby improving query performance.

With the TPC-DS 3TB dataset, total query execution time improved by 40% when using AWS Glue Data Catalog column statistics. Some individual TPC-DS queries improved up to 5x in execution time. Some queries with significant impact on execution time include Q85, Q64, Q75, Q78, Q94, Q16, Q04, Q24, and Q11.

We'll go through an example where the cost-based optimizer generates a better query plan thanks to statistics and how it improves execution time.

Consider the simpler version of TPC-DS Q64 below to illustrate query plan differences when statistics are available.

```sql
select i_product_name product_name
,i_item_sk item_sk
,ad1.ca_street_number b_street_number
,ad1.ca_street_name b_street_name
,ad1.ca_city b_city
,ad1.ca_zip b_zip
,d1.d_year as syear
,count(*) cnt
,sum(ss_wholesale_cost) s1
,sum(ss_list_price) s2
,sum(ss_coupon_amt) s3
FROM   tpcds_3t_alls3_pp_ext.store_sales
,tpcds_3t_alls3_pp_ext.store_returns
,tpcds_3t_alls3_pp_ext.date_dim d1
,tpcds_3t_alls3_pp_ext.customer
,tpcds_3t_alls3_pp_ext.customer_address ad1
,tpcds_3t_alls3_pp_ext.item
WHERE
ss_sold_date_sk = d1.d_date_sk AND
ss_customer_sk = c_customer_sk AND
ss_addr_sk = ad1.ca_address_sk and
ss_item_sk = i_item_sk and
ss_item_sk = sr_item_sk and
ss_ticket_number = sr_ticket_number and
i_color in ('firebrick','papaya','orange','cream','turquoise','deep') and
i_current_price between 42 and 42 + 10 and
i_current_price between 42 + 1 and 42 + 15
group by i_product_name
,i_item_sk
,ad1.ca_street_number
,ad1.ca_street_name
,ad1.ca_city
,ad1.ca_zip
,d1.d_year
```

### Without using Statistics

The figure below shows the logical query plan of Q64. You can notice that the cardinality estimation of the joins is inaccurate. With inaccurate cardinalities, the optimizer will generate a sub-optimal query plan, leading to higher execution time.

![Figure 5: Logical query plan of Q64 when not using statistics](/images/3-BlogsTranslated/3.2-Blog2/figure5-q64-without-statistics.png)

*Figure 5: Logical query plan of Q64 when not using statistics*

### Using Statistics

The figure below shows the logical query plan after using AWS Glue Data Catalog column statistics. Based on the highlighted changes, you can see that the cardinality estimations of JOINs have improved significantly, helping the optimizer choose a better join order and join strategy (broadcast DS_BCAST_INNER vs. distribute DS_DIST_BOTH). The swapping of customer_address and customer tables from inner to outer table and changing join strategies to distribute has a large impact because this reduces data movement between nodes and avoids spilling from hash tables.

![Figure 6: Logical query plan of Q64 after consuming AWS Glue Data Catalog column statistics](/images/3-BlogsTranslated/3.2-Blog2/figure6-q64-with-statistics.png)

*Figure 6: Logical query plan of Q64 after consuming AWS Glue Data Catalog column statistics*

This change in query plan improved Q64's query execution time from 383s to 81s. With the greater benefits of using AWS Glue Data Catalog column statistics for the optimizer, you should consider collecting stats for your data lake using AWS Glue. If your workload is a JOIN-heavy workload, collecting stats will bring greater improvements to your workload. Please refer to generating AWS Glue Data Catalog column statistics for guidance on how to collect statistics in AWS Glue Data Catalog.

## Optimization by query rewriting

We introduced a new query rewrite rule that combines scalar aggregates on the same common expression but using slightly different predicates. This rewrite has led to performance improvements on TPC-DS queries Q09, Q28, and Q88. Let's focus on Q09 as a representative of these queries, presented in the fragment below:

```sql
SELECT CASE
WHEN (SELECT COUNT(*)
FROM store_sales
WHERE ss_quantity BETWEEN 1 AND 20) > 48409437
THEN (SELECT AVG(ss_ext_discount_amt)
FROM store_sales
WHERE ss_quantity BETWEEN 1 AND 20)
ELSE (SELECT AVG(ss_net_profit)
FROM store_sales
WHERE ss_quantity BETWEEN 1 AND 20) END
AS bucket1,
<<4 more variations of the CASE expression above>>
FROM reason
WHERE r_reason_sk = 1
```

There are a total of 15 scans of the fact table store_sales, each scan returning different aggregates on different subsets of data. The query engine first performs subquery removal and transforms the different expressions in CASE statements into relational subtrees connected through cross products, then they are fused into a single subquery handling all scalar aggregates. The resulting query plan for Q09, described below in SQL for easier understanding, is as follows:

```sql
SELECT CASE WHEN v1 > 48409437 THEN t1 ELSE e1 END,
<4 more variations>
FROM (SELECT COUNT(CASE WHEN b1 THEN 1 END) AS v1,
AVG(CASE WHEN b1 THEN ss_ext_discount_amt END) AS t1,
AVG(CASE WHEN b1 THEN ss_net_profit END) AS e1,
<4 more variations>
FROM reason,
(SELECT *,
ss_quantity BETWEEN 1 AND 20 AS b1,
<4 more variations>
FROM store_sales
WHERE ss_quantity BETWEEN 1 AND 20 OR
<4 more variations>))
WHERE r_reason_sk = 1)
```

Overall, this rewrite rule brings the greatest improvements both in latency (3x to 8x speedup) and bytes read from Amazon S3 (6x to 8x reduction in scanned bytes and, accordingly, cost reduction).

## Bloom filter for partition columns

Amazon Redshift has been using Bloom filters on data columns of external tables in Amazon S3 to enable early and effective data filtering. Last year, we extended this support to partition columns as well.

A Bloom filter is a probabilistic, memory-efficient data structure that helps accelerate join queries at scale by filtering rows that don't match the join relation, thereby significantly reducing the amount of data transferred over the network. Amazon Redshift automatically identifies suitable queries to use Bloom filters at query runtime.

This optimization has brought performance improvements to TPC-DS queries Q05, Q17, and Q54, with large improvements both in latency (2x to 3x speedup) and bytes read from S3 (9x to 15x reduction in scanned bytes and accordingly cost reduction).

Below is a subquery of Q05 illustrating improvements with runtime filter.

```sql
select s_store_id,
sum(sales_price) as sales,
sum(profit) as profit,
sum(return_amt) as returns,
sum(net_loss) as profit_loss
from
( select  ss_store_sk as store_sk,
ss_sold_date_sk  as date_sk,
ss_ext_sales_price as sales_price,
ss_net_profit as profit,
cast(0 as decimal(7,2)) as return_amt,
cast(0 as decimal(7,2)) as net_loss
from tpcds_3t_alls3_pp_ext.store_sales
union all
select sr_store_sk as store_sk,
sr_returned_date_sk as date_sk,
cast(0 as decimal(7,2)) as sales_price,
cast(0 as decimal(7,2)) as profit,
sr_return_amt as return_amt,
sr_net_loss as net_loss
from tpcds_3t_alls3_pp_ext.store_returns
) salesreturnss,
tpcds_3t_alls3_pp_ext.date_dim,
tpcds_3t_alls3_pp_ext.store
where date_sk = d_date_sk
and d_date between cast('1998-08-13' as date)
and (cast('1998-08-13' as date) +  14)
and store_sk = s_store_sk
group by s_store_id
```

### Without bloom filter support on partition columns

The figure below is the logical query plan for the sub-query of Q05. This query appends two large fact tables: store_sales (8B rows) and store_returns (863M rows), then joins with very selective dimension tables date_dim and then with dimension table store. You can observe that the join with the date_dim table has reduced the number of rows from 9B to 93M rows.

![Figure 7: Logical query plan for sub-query of Q05 without bloom filter support on partition columns](/images/3-BlogsTranslated/3.2-Blog2/figure7-q05-without-bloom-filter.png)

*Figure 7: Logical query plan for sub-query of Q05 without bloom filter support on partition columns*

### With bloom filter support on partition columns

With support of bloom filter on partition columns, we now create a bloom filter for the d_date_sk column of the date_dim table and push down these bloom filters to the store_sales and store_returns tables.

These bloom filters help filter out partitions in both store_sales and store_returns tables because the join occurs on the partition column (the number of partitions processed decreases by 10x).

![Figure 8: Logical query plan for sub-query of Q05 with bloom filter support on partition columns](/images/3-BlogsTranslated/3.2-Blog2/figure8-q05-with-bloom-filter.png)

*Figure 8: Logical query plan for sub-query of Q05 with bloom filter support on partition columns*

Overall, the bloom filter on partition column will reduce the number of partitions processed, leading to reduced S3 listing calls and reduced number of data files that need to be read (reduction in scanned bytes). You can see that we only scan 89M rows from store_sales and 4M rows from store_returns thanks to the bloom filter. This reduction in the number of rows at the JOIN level has helped improve overall query performance by 2x and scanned bytes by 9x.

## Conclusion

In this post, we presented the new performance optimizations in Amazon Redshift data lake query processing and how AWS Glue Data Catalog statistics help enhance the quality of query plans for data lake queries in Amazon Redshift. These optimizations have significantly improved query performance, with the TPC-DS 3TB benchmark showing a 3x improvement in total execution time. Some individual queries achieved speeds up to 12x faster. We recommend using AWS Glue Data Catalog column statistics, especially for JOIN-heavy workloads, to achieve optimal query performance for your data lake queries.

In summary, Amazon Redshift now provides enhanced query performance with optimizations such as: AWS Glue Data Catalog column statistics, bloom filters on partition columns, new query rewrite rules, and faster retrieval of metadata. These optimizations are enabled by default, and Amazon Redshift users will benefit from better query response times for their workloads.

For more information, please contact your AWS technical account manager or AWS account solutions architect. They will be ready to provide additional guidance and support.

---

## About the Authors

![Kalaiselvi Kamaraj](/images/3-BlogsTranslated/3.2-Blog2/Kalaiselvi-Kamaraj.png)

### Kalaiselvi Kamaraj

Kalaiselvi Kamaraj is a Sr. Software Development Engineer at Amazon. She has been involved in many projects in the Redshift Query Processing team and is currently focusing on performance-related projects for Redshift Data Lake.

![Mark Lyons](/images/3-BlogsTranslated/3.2-Blog2/Mark-Lyons.png)

### Mark Lyons

Mark Lyons is a Principal Product Manager in the Amazon Redshift team. He works at the intersection of data lakes and data warehouses. Before joining AWS, Mark held product leadership positions at Dremio and Vertica. He is passionate about data analytics and wants to empower customers to change the world with their data.

![Asser Moustafa](/images/3-BlogsTranslated/3.2-Blog2/Asser-Moustafa.png)

### Asser Moustafa

Asser Moustafa is a Principal Worldwide Specialist Solutions Architect at AWS, based in Dallas, Texas, USA. He collaborates with customers globally, advising on all aspects of data architectures, migrations, and strategic data visions to help organizations adopt cloud-based solutions, maximize the value of their data assets, modernize legacy infrastructures, and deploy cutting-edge capabilities such as machine learning and advanced analytics. Before joining AWS, Asser held many data and analytics leadership positions, and completed an MBA at New York University and an MS in Computer Science at Columbia University, New York. He is passionate about helping organizations become truly data-driven and harnessing the transformative potential from their data.

**TAGS:** Amazon Redshift Spectrum, Analytics, Data Lake, optimization, Performance
