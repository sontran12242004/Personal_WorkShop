---
title: "Blog 2"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Tăng tốc các truy vấn Amazon Redshift Data Lake với AWS Glue Data Catalog Column Statistics

by Kalaiselvi Kamaraj, Asser Moustafa, and Mark Lyons | on 01 OCT 2024 in | Amazon Redshift | Amazon Simple Storage Service (S3) | Analytics, Announcements | AWS Big Data | AWS Glue | Best Practices Permalink

Amazon Redshift cho phép bạn truy vấn và truy xuất dữ liệu có cấu trúc và bán cấu trúc từ các open format files trong Amazon S3 data lake mà không cần phải load dữ liệu vào các bảng Amazon Redshift. Amazon Redshift mở rộng SQL capabilities tới data lake, cho phép bạn thực hiện các analytical queries. Redshift hỗ trợ nhiều định dạng dữ liệu bảng khác nhau như CSV, JSON, Parquet, ORC và các định dạng bảng mở như Apache Hudi, Linux Foundation Delta Lake, và Apache Iceberg.

Bạn tạo Redshift external tables bằng cách định nghĩa cấu trúc cho các file, S3 location của file và đăng ký chúng như bảng trong external data catalog. External data catalog có thể là AWS Glue Data Catalog, data catalog đi kèm với Amazon Athena, hoặc Apache Hive metastore của bạn.

Trong năm vừa qua, Amazon Redshift đã thêm một số performance optimizations cho data lake queries trên nhiều khía cạnh của query engine như rewrite, planning, scan execution và sử dụng AWS Glue Data Catalog column statistics. Để đạt performance tốt nhất cho các truy vấn data lake với Redshift, bạn có thể sử dụng column statistics của AWS Glue Data Catalog để thu thập thống kê trên các bảng Data Lake. Đối với Amazon Redshift Serverless instances, bạn sẽ thấy scan performance cải thiện nhờ parallel processing tăng lên của các file S3 và điều này diễn ra tự động dựa trên RPUs sử dụng.

Trong bài viết này, chúng tôi nhấn mạnh các cải thiện về hiệu suất quan sát được bằng cách sử dụng industry standard TPC-DS benchmarks. Tổng execution time của TPC-DS 3 TB benchmark cải thiện gấp 3 lần. Một số truy vấn trong benchmark đạt tốc độ nhanh hơn gấp 12 lần.

## Cải thiện hiệu suất

Trong năm vừa qua, đã có nhiều performance optimizations được thực hiện để cải thiện hiệu suất của các data lake queries, bao gồm:

- Sử dụng AWS Glue Data Catalog column statistics và tuning Redshift optimizer để cải thiện chất lượng query plans.
- Sử dụng bloom filters cho các partition columns.
- Cải thiện scan efficiency cho Amazon Redshift Serverless instances thông qua tăng parallel processing của các file.
- Áp dụng các novel query rewrite rules để merge các scans tương tự.
- Truy xuất metadata nhanh hơn từ AWS Glue Data Catalog.

Để hiểu rõ performance gains, chúng tôi đã kiểm tra hiệu suất bằng industry-standard TPC-DS benchmark sử dụng 3 TB data sets và queries, đại diện cho các customer use cases khác nhau. Hiệu suất được kiểm tra trên Redshift serverless data warehouse với 128 RPU. Trong thử nghiệm, dataset được lưu trữ trên Amazon S3 ở định dạng Parquet, và AWS Glue Data Catalog được sử dụng để quản lý các external databases và tables. Các fact tables được partition theo date column, mỗi bảng gồm khoảng 2,000 partitions. Tất cả các bảng đều có row count table property, numRows, được thiết lập theo spectrum query performance guidelines.

Chúng tôi thực hiện một baseline run trên Redshift patch version (patch 172) của năm trước. Sau đó, chạy tất cả các truy vấn TPC-DS trên latest patch version (patch 180), bao gồm tất cả performance optimizations được thêm trong năm qua. Tiếp đó, sử dụng AWS Glue Data Catalog column statistics để tính toán thống kê cho tất cả bảng và đo lường cải thiện hiệu suất khi có column statistics.

Phân tích cho thấy TPC-DS 3TB Parquet benchmark đạt được performance gains đáng kể nhờ các tối ưu hóa này. Cụ thể, partitioned Parquet với các tối ưu hóa mới nhất đạt runtime nhanh hơn 2x so với triển khai trước đó. Việc bật AWS Glue Data Catalog column statistics tiếp tục cải thiện hiệu suất gấp 3 lần so với năm trước. Đồ thị dưới đây minh họa runtime improvements cho toàn bộ benchmark (tất cả truy vấn TPC-DS) trong năm qua, bao gồm boost bổ sung nhờ sử dụng AWS Glue Data Catalog column statistics.

![Hình 1: Cải thiện tổng thời gian thực thi (total runtime) của TPC-DS 3T workload](/images/3-BlogsTranslated/3.2-Blog2/figure1-total-runtime-improvements.png)

*Hình 1: Cải thiện tổng thời gian thực thi (total runtime) của TPC-DS 3T workload*

Đồ thị dưới đây trình bày các top queries từ TPC-DS benchmark có cải thiện hiệu suất lớn nhất trong năm qua, với và không có AWS Glue Data Catalog column statistics. Bạn có thể thấy rằng hiệu suất cải thiện đáng kể khi có statistics trên AWS Glue Data Catalog (để biết chi tiết cách thu thập statistics cho các Data Lake tables, vui lòng tham khảo optimizing query performance using AWS Glue Data Catalog column statistics). Cụ thể, các multi-join queries sẽ hưởng lợi nhiều nhất từ AWS Glue Data Catalog column statistics, bởi vì optimizer sử dụng statistics để chọn join order và distribution strategy phù hợp.

![Hình 2: Tăng tốc (Speed-up) trong các truy vấn TPC-DS](/images/3-BlogsTranslated/3.2-Blog2/figure2-speedup-queries.png)

*Hình 2: Tăng tốc (Speed-up) trong các truy vấn TPC-DS*

Hãy cùng thảo luận một số optimizations đã góp phần cải thiện query performance.

## Tối ưu hóa với thống kê ở cấp bảng (table-level statistics)

Amazon Redshift được thiết kế để xử lý các thách thức large-scale data với speed và cost-efficiency vượt trội. Massively parallel processing (MPP) query engine, AI-powered query optimizer, auto-scaling capabilities và các tính năng tiên tiến khác giúp Redshift xuất sắc trong việc searching, aggregating, và transforming petabytes of data.

Tuy nhiên, ngay cả những hệ thống mạnh nhất cũng có thể gặp phải performance degradation nếu gặp phải các anti-patterns như table statistics cực kỳ inaccurate, ví dụ như row count metadata.

Thiếu metadata quan trọng này, query optimizer của Redshift có thể bị hạn chế trong số lượng các possible optimizations, đặc biệt là các tối ưu liên quan đến data distribution trong quá trình query execution. Điều này có thể ảnh hưởng đáng kể đến overall query performance.

Để minh họa, hãy xem xét một truy vấn đơn giản liên quan đến inner join giữa một large table với hàng tỷ rows và một small table chỉ có vài trăm nghìn rows.

```sql
select small_table.sellerid, sum(large_table.qtysold)
from large_table, small_table
where large_table.salesid = small_table.listid
 and small_table.listtime > '2023-12-01'
 and large_table.saletime > '2023-12-01'
group by 1 order by 1
```

Nếu được executed as-is, với large table nằm ở right-hand side của join, truy vấn sẽ dẫn đến sub-optimal performance. Điều này là do large table sẽ cần được distributed (broadcast) đến tất cả các Redshift compute nodes để thực hiện inner join với small table, như minh họa trong diagram dưới đây.

![Hình 3: Inaccurate table statistics dẫn đến limited optimizations và lượng lớn dữ liệu phải broadcast giữa các compute nodes cho một simple inner join](/images/3-BlogsTranslated/3.2-Blog2/figure3-inaccurate-statistics.png)

*Hình 3: Inaccurate table statistics dẫn đến limited optimizations và lượng lớn dữ liệu phải broadcast giữa các compute nodes cho một simple inner join*

Bây giờ, hãy xem xét một scenario trong đó table statistics, chẳng hạn như row count, là accurate. Điều này cho phép Amazon Redshift query optimizer đưa ra các quyết định informed hơn, chẳng hạn như xác định optimal join order. Trong trường hợp này, optimizer sẽ ngay lập tức rewrite truy vấn để large table nằm ở left-hand side của inner join, sao cho small table là bảng được broadcast đến các Redshift compute nodes, như minh họa trong diagram dưới đây.

![Hình 4: Accurate table statistics dẫn đến high degree of optimizations và rất ít dữ liệu được broadcast giữa các compute nodes cho một simple inner join](/images/3-BlogsTranslated/3.2-Blog2/figure4-accurate-statistics.png)

*Hình 4: Accurate table statistics dẫn đến high degree of optimizations và rất ít dữ liệu được broadcast giữa các compute nodes cho một simple inner join*

May mắn thay, Amazon Redshift tự động duy trì accurate table statistics cho các local tables bằng cách chạy lệnh ANALYZE ở chế độ nền. Tuy nhiên, đối với các external tables (data lake tables), việc sử dụng AWS Glue Data Catalog column statistics được khuyến nghị khi làm việc với Amazon Redshift, như chúng tôi sẽ thảo luận trong phần tiếp theo. Để biết thêm thông tin tổng quát về optimizing queries trong Amazon Redshift, vui lòng tham khảo tài liệu về factors affecting query performance, data redistribution, và Amazon Redshift best practices for designing queries.

## Cải thiện hiệu suất với AWS Glue Data Catalog column statistics

AWS Glue Data Catalog có một tính năng để compute column level statistics cho các Amazon S3 backed external tables. AWS Glue Data Catalog có thể tính toán các column level statistics như NDV, Number of Nulls, Min/Max, và Avg. column width cho các cột mà không cần các data pipelines bổ sung.

Amazon Redshift cost-based optimizer sử dụng các statistics này để tạo ra các query plans có chất lượng tốt hơn. Bên cạnh việc sử dụng statistics, chúng tôi cũng thực hiện một số cải tiến trong cardinality estimations và cost tuning để có được các high quality query plans, từ đó cải thiện query performance.

Với TPC-DS 3TB dataset, tổng query execution time đã cải thiện 40% khi sử dụng AWS Glue Data Catalog column statistics. Một số TPC-DS queries riêng lẻ đã cải thiện lên đến 5x về thời gian thực thi. Một số truy vấn có tác động lớn đến thời gian thực thi bao gồm Q85, Q64, Q75, Q78, Q94, Q16, Q04, Q24, và Q11.

Chúng tôi sẽ đi qua một ví dụ, nơi cost-based optimizer tạo ra query plan tốt hơn nhờ statistics và cách nó cải thiện execution time.

Hãy xem xét phiên bản simpler của TPC-DS Q64 dưới đây để minh họa query plan differences khi có statistics.

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

### Không sử dụng Statistics

Hình dưới đây thể hiện logical query plan của Q64. Bạn có thể nhận thấy rằng cardinality estimation của các joins không chính xác. Với cardinalities không chính xác, optimizer sẽ tạo ra một query plan không tối ưu, dẫn đến execution time cao hơn.

![Hình 5: Logical query plan của Q64 khi không sử dụng statistics](/images/3-BlogsTranslated/3.2-Blog2/figure5-q64-without-statistics.png)

*Hình 5: Logical query plan của Q64 khi không sử dụng statistics*

### Sử dụng Statistics

Hình dưới đây thể hiện logical query plan sau khi sử dụng AWS Glue Data Catalog column statistics. Dựa trên các thay đổi được highlight, bạn có thể thấy rằng cardinality estimations của JOIN đã được cải thiện đáng kể, giúp optimizer chọn được join order và join strategy tốt hơn (broadcast DS_BCAST_INNER so với distribute DS_DIST_BOTH). Việc hoán đổi bảng customer_address và customer từ inner sang outer table và chuyển join strategies sang distribute có tác động lớn vì điều này giảm data movement giữa các nodes và tránh spilling từ hash table.

![Hình 6: Logical query plan của Q64 sau khi sử dụng AWS Glue Data Catalog column statistics](/images/3-BlogsTranslated/3.2-Blog2/figure6-q64-with-statistics.png)

*Hình 6: Logical query plan của Q64 sau khi sử dụng AWS Glue Data Catalog column statistics*

Sự thay đổi trong query plan này đã cải thiện query execution time của Q64 từ 383s xuống còn 81s. Với lợi ích lớn hơn khi sử dụng AWS Glue Data Catalog column statistics cho optimizer, bạn nên xem xét việc collecting stats cho data lake của mình bằng AWS Glue. Nếu workload của bạn là JOIN heavy workload, việc collecting stats sẽ mang lại cải thiện lớn hơn cho workload của bạn. Vui lòng tham khảo generating AWS Glue Data Catalog column statistics để biết hướng dẫn về cách collect statistics trong AWS Glue Data Catalog.

## Tối ưu hóa bằng cách viết lại truy vấn

Chúng tôi đã giới thiệu một new query rewrite rule kết hợp các scalar aggregates trên cùng một common expression nhưng sử dụng các predicates hơi khác nhau. Việc rewrite này đã dẫn đến performance improvements trên các TPC-DS queries Q09, Q28, và Q88. Hãy tập trung vào Q09 như một representative của các truy vấn này, được trình bày trong fragment dưới đây:

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

Tổng cộng có 15 scans của fact table store_sales, mỗi scan trả về các aggregates khác nhau trên các subsets of data khác nhau. Query engine trước tiên thực hiện subquery removal và biến đổi các expressions khác nhau trong CASE statements thành các relational subtrees được kết nối thông qua cross products, sau đó chúng được fused thành một subquery duy nhất xử lý tất cả các scalar aggregates. Query plan kết quả cho Q09, được mô tả dưới đây bằng SQL để dễ hiểu, như sau:

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

Nhìn chung, rewrite rule này mang lại cải thiện lớn nhất cả về latency (tăng tốc từ 3x đến 8x) và bytes read from Amazon S3 (giảm từ 6x đến 8x scanned bytes và, theo đó, giảm cost).

## Bloom filter cho các cột phân vùng

Amazon Redshift đã sử dụng Bloom filters trên các data columns của external tables trong Amazon S3 để cho phép early and effective data filtering. Năm ngoái, chúng tôi đã mở rộng hỗ trợ này cho cả các partition columns.

Một Bloom filter là một probabilistic, memory-efficient data structure giúp accelerate join queries at scale bằng cách filter các rows không khớp với join relation, từ đó giảm đáng kể lượng dữ liệu được chuyển qua mạng. Amazon Redshift tự động xác định các queries phù hợp để sử dụng Bloom filters tại thời điểm query runtime.

Optimization này đã mang lại performance improvements cho các TPC-DS queries Q05, Q17, và Q54, với cải thiện lớn cả về latency (tăng từ 2x đến 3x) và bytes read from S3 (giảm từ 9x đến 15x scanned bytes và theo đó giảm cost).

Dưới đây là subquery của Q05 minh họa improvements với runtime filter.

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

Hình dưới đây là logical query plan cho sub-query của Q05. Query này appends hai large fact tables là store_sales (8B rows) và store_returns (863M rows), sau đó joins với các very selective dimension tables date_dim và tiếp theo với dimension table store. Bạn có thể quan sát rằng join với date_dim table đã reduce số lượng rows từ 9B xuống còn 93M rows.

![Hình 7: Logical query plan cho sub-query của Q05 without bloom filter support on partition columns](/images/3-BlogsTranslated/3.2-Blog2/figure7-q05-without-bloom-filter.png)

*Hình 7: Logical query plan cho sub-query của Q05 without bloom filter support on partition columns*

### With bloom filter support on partition columns

Với support of bloom filter trên các partition columns, chúng tôi hiện tạo bloom filter cho d_date_sk column của date_dim table và push down các bloom filters này xuống store_sales và store_returns table.

Các bloom filters này giúp filter out các partitions trong cả store_sales và store_returns table vì join diễn ra trên partition column (số lượng partitions processed giảm 10x).

![Hình 8: Logical query plan cho sub-query của Q05 with bloom filter support on partition columns](/images/3-BlogsTranslated/3.2-Blog2/figure8-q05-with-bloom-filter.png)

*Hình 8: Logical query plan cho sub-query của Q05 with bloom filter support on partition columns*

Nhìn chung, bloom filter trên partition column sẽ reduce the number of partitions processed, dẫn đến giảm các S3 listing calls và giảm số lượng data files cần đọc (reduction in scanned bytes). Bạn có thể thấy rằng chúng ta chỉ scan 89M rows từ store_sales và 4M rows từ store_returns nhờ bloom filter. Số lượng rows giảm này tại JOIN level đã giúp cải thiện overall query performance 2x và scanned bytes 9x.

## Kết luận

Trong bài viết này, chúng tôi đã trình bày các performance optimizations mới trong Amazon Redshift data lake query processing và cách AWS Glue Data Catalog statistics giúp enhance quality of query plans cho các data lake queries trong Amazon Redshift. Các optimizations này đã cải thiện đáng kể query performance, với TPC-DS 3TB benchmark cho thấy cải thiện 3x về tổng thời gian thực thi. Một số truy vấn riêng lẻ đạt tốc độ nhanh hơn lên đến 12x. Chúng tôi khuyến nghị sử dụng AWS Glue Data Catalog column statistics, đặc biệt cho các JOIN-heavy workloads, để đạt được query performance tối ưu cho các truy vấn data lake của bạn.

Tóm lại, Amazon Redshift hiện cung cấp enhanced query performance với các optimizations như: AWS Glue Data Catalog column statistics, bloom filters trên partition columns, new query rewrite rules, và faster retrieval of metadata. Các optimizations này được enabled by default, và người dùng Amazon Redshift sẽ được hưởng lợi với better query response times cho workloads của mình.

Để biết thêm thông tin, vui lòng liên hệ với AWS technical account manager hoặc AWS account solutions architect của bạn. Họ sẽ sẵn sàng cung cấp additional guidance and support.

---

## Về các Tác giả

![Kalaiselvi Kamaraj](/images/3-BlogsTranslated/3.2-Blog2/Kalaiselvi-Kamaraj.png)

### Kalaiselvi Kamaraj

Kalaiselvi Kamaraj là Sr. Software Development Engineer tại Amazon. Cô đã tham gia nhiều dự án trong đội ngũ Redshift Query Processing và hiện đang tập trung vào các dự án liên quan đến performance cho Redshift Data Lake.

![Mark Lyons](/images/3-BlogsTranslated/3.2-Blog2/Mark-Lyons.png)

### Mark Lyons

Mark Lyons là Principal Product Manager trong đội ngũ Amazon Redshift. Anh làm việc tại giao điểm giữa data lakes và data warehouses. Trước khi gia nhập AWS, Mark đảm nhiệm các vị trí product leadership tại Dremio và Vertica. Anh đam mê data analytics và mong muốn empowering customers để họ có thể change the world with their data.

![Asser Moustafa](/images/3-BlogsTranslated/3.2-Blog2/Asser-Moustafa.png)

### Asser Moustafa

Asser Moustafa là Principal Worldwide Specialist Solutions Architect tại AWS, có trụ sở tại Dallas, Texas, USA. Ông hợp tác với khách hàng trên toàn cầu, tư vấn về tất cả các khía cạnh của data architectures, migrations, và strategic data visions nhằm giúp các tổ chức adopt cloud-based solutions, maximize the value of their data assets, modernize legacy infrastructures, và triển khai các cutting-edge capabilities như machine learning và advanced analytics. Trước khi gia nhập AWS, Asser đảm nhiệm nhiều vị trí lãnh đạo về data và analytics, đồng thời hoàn thành MBA tại New York University và MS in Computer Science tại Columbia University, New York. Ông đam mê việc giúp các tổ chức trở nên truly data-driven và khai thác transformative potential từ dữ liệu của họ.

**TAGS:** Amazon Redshift Spectrum, Analytics, Data Lake, optimization, Performance
