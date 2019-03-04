<font color = red size = 8><B>网络trace的分类方法</B></font>  


<font color = red size = 5><B>K-Means聚类算法</B></font>  

----

<font color = red size = 4>聚类的定义:</font>

        K-均值算法：通过指定k值，随机分配k个质心，然后计算每个数据点到各个质心的距离，将点分配到距离最近的质心，重新计算每个簇的均值更新质心，反复迭代直到质心不在变化。（算法有效但初始k值不容易确定） 


<font color = red size = 4>轮廓系数法找最优K值</font>



相关网址：https://blog.csdn.net/qq_15738501/article/details/79036255





<font color = red size = 5><b>相关demo：</b></font>

`“0123”数字的分类:`
```
https://blog.csdn.net/fengbingchun/article/details/79511318
```

`对地图上的点进行分类`
```
https://blog.csdn.net/sinat_17196995/article/details/70332664
```

之前看到的demo：  
https://github.com/fengbingchun/NN_Test/tree/master/demo/Python

专区demo：  
https://scikit-learn.org/stable/modules/clustering.html#clustering




从数据库中提取数据的命令：
```
DROP TABLE IF EXISTS `query_hive_1084974`;

CREATE TABLE `query_hive_1084974` ROW FORMAT DELIMITED
     FIELDS TERMINATED BY '\t'
     ESCAPED BY '\\'
     LINES TERMINATED BY '\n'
     STORED AS TEXTFILE LOCATION '/user/renerbin1/hiveResult/2018_11_03'
     AS
select rtime,countrycode,httpclientIp, anchoruid,userip,useruid,cubeuid,tmpuid,isquic,tsbw,tsinterval 
from cubetv.cube_web_video_quality_data
where day = '2018-11-03'
and videoType = 1
and tmpuid <> '88888888'
and tsbw is not null
and tsbw like '[%]'
and tsinterval is not null
and tsinterval like '[%]'
and userbandwidth not in ('NaN', '[0,0,0,0,0,0]')
order by countrycode, anchoruid, tmpuid, rtime;

ALTER TABLE `query_hive_1084974` SET TBLPROPERTIES('EXTERNAL'='TRUE');

DROP TABLE IF EXISTS `query_hive_1084974`;
```

`K-Means算法函数：`
```
k_means = KMeans(init='k-means++', n_clusters=2, n_init=10)
    k_means.fit(features)
```

`获得分类的中心点：`     

        k_means_cluster_centers = k_means.cluster_centers_

`根据中心点和数据列表得出分类列表：`  

        k_means_labels = pairwise_distances_argmin(features, k_means_cluster_centers)



