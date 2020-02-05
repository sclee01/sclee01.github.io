---
layout: post
title: Comparison between Spark and MapReduce
excerpt: "스파크와 맵리듀스 성능비교"
date: 2018-06-29
modified: 2018-06-29
author: sclee
categories: blog
tags: [Spark,BigData,Hadoop]
image:
  url: http://i.imgur.com/p0TzIOM.gif
comments: true
share: true
---


<h2 id="mapreduce-vs.-spark">MapReduce vs. Spark</h2>
<p>논문 리뷰.<br>
스파크와 맵리듀스의 성능에 대해 비교하는 논문입니다.</p>
<h3 id="고려요소">고려요소</h3>
<p><strong>Shuffle selectivity</strong> : the ratio of the map output size to the job input size, which represents the amount of disk and network I/O for a shuffle.</p>
<p><strong>Job selectivity</strong> : the ratio of the reduce output size to the job input size, which represnets the amount of HDFS writes.</p>
<p><strong>Iteration selectivity</strong> : the ratio of the output size to the input size for each iteration, which represents the amout of intermediate data exchanged across iteration.</p>
<h3 id="workload">Workload</h3>
<h4 id="word-count-wc">Word Count (WC)</h4>
<ul>
<li>Map : Spark is faster than MapReduce about 3x.<br>
Task initialization, input read, map operation 과정이 Spark가 MapReduce보다 성능이 좋습니다. 특히, Map의 combine 과정에서 Spark의 Hash-based combine이 MapReduce의 Sort-based combine 보다 efficient합니다.</li>
<li>Reduce : Shuffle selectivity가 매우 낮기 때문에 WC에서 reduce때문에 성능의 차이는 보이지 않습니다.</li>
</ul>
<h4 id="sort">Sort</h4>
<ul>
<li>Sampling : Spark는 전체 파일을 다 스캔한다. MapReduce는 부분적으로 파일을 스캔한다. 따라서 Spark이 시간이 오래 소요됩니다.</li>
<li>Map : Spark의 경우 Sampling에서 전체 파일을 다 스캔하였기 때문에 Map에서는 disk I/O가 이전에 비해 줄어드는 것이 확인된다. Spark가 데이터를 읽고 map하는 과정이 MapReduce에 비해 효율적입니다.</li>
<li>Reduce : MapReduce는 Map과 Reduce를 overlap하여 Network I/O를 줄이는 방식을 채택하고 있기 때문에 그렇지 않은 스파크 보다 시간이 적게 걸립니다.</li>
</ul>
<h3 id="k-means">K-means</h3>
<ul>
<li>Map : K-means의 map stage 는 cpu-bound 과정입니다. shuffle selectivity는 매우 낮습니다.  Spark의 in-memory정책 때문에 subsequent iteration부터는 disk I/O가 발생하지 않습니다.</li>
<li>Reduce : low shuffle selectivity때문에 K-means에서 reduce 과정은 bottleneck이 아닙니다.</li>
</ul>
<h4 id="what-is-the-bottleneck-for-k-means">What is the bottleneck for k-means?</h4>
<ul>
<li>RDD caching으로 DISK_ONLY를 사용했을 경우, 첫 번째 iteration 이후에 OS buffer cache를 drop시킬때, 그 다음부터는 disk I/O가 많이 발생합니다. 즉, DISK_ONLY를 사용했을 경우, OS buffer cache를 활용한다는 것을 알 수 있습니다. 여기서 disk의 개수가 충분하여 disk-bound가 고려대상이 아닐 경우에는 성능 차이가 발생하지 않습니다.
</li>
</ul>

