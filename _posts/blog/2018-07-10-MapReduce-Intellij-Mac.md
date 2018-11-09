---
layout: post
title: MapReduce setup for Intellij on Mac
excerpt: "맵리듀스 개발 환경 구성하기"
date: 2018-07-10
modified: 2018-07-10
author: sclee
categories: blog
tags: [MapReduce]
image:
  url: http://i.imgur.com/p0TzIOM.gif
comments: true
share: true
---

##   MapReduce + Intellij + Mac 실행환경 구성

### 개요

 맥 OS에서 Intellij를 사용하여 하둡의 MapReduce 개발 환경을 구성하는  방법에 대해 알아보고자 합니다.


###  구성 방법

Intellij IDEA를 실행 시킵니다. 그리고 `File | New | Project | Maven` 순서로 Maven 기반의 Java 프로젝트를 실행합니다. `pom.xml`을 클릭 후에
아래의 하둡관련 디펜던시를 `pom.xml`에 추가해줍니다.

```
<dependencies>  
  <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common -->  
  <dependency>  
 <groupId>org.apache.hadoop</groupId>  
 <artifactId>hadoop-common</artifactId>  
 <version>2.5.2</version>  
 </dependency>  
  <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-mapreduce-client-core -->  
  <dependency>  
 <groupId>org.apache.hadoop</groupId>  
 <artifactId>hadoop-mapreduce-client-core</artifactId>  
 <version>2.5.2</version>  
 </dependency>
```


조금 기다리시면 자동으로 Maven이 라이브러리를 가져오는 것을 확인할 수 있습니다.

이제 Java 버전을 확인합니다. Intellij에서 setting 하는 자바 버전이 모두 동일한지 확인해야 합니다. `File | Other settings | Default settings ` 에서 `Build tools` 그리고 `Maven` 에 있는 `Importer`와 `Runner`에서 JRE 및 JDK가 동일한 자바 버전인지 확인합니다. 저의 경우 Java 1.8을 사용하였습니다. 추가적으로 `Project Settings`에 `Project`의 `Project SDK`가 어떤 자바 버전을 사용하고 있는지도 확인해 봅니다. 

여기 까지 완료 후에 MapReduce의 대표 예제 코드인 WordCount를 실행합니다.

### WordCount 소스
```
import org.apache.hadoop.conf.Configuration;  
import org.apache.hadoop.fs.Path;  
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.LongWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Job;  
import org.apache.hadoop.mapreduce.Mapper;  
import org.apache.hadoop.mapreduce.Reducer;  
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;  
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;  
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;  
import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;  
import java.io.IOException;  
import java.util.StringTokenizer;  
  
  
  
public class WordCount {  
    public static class Map extends Mapper<LongWritable, Text, Text, IntWritable> {  
        private final static IntWritable one = new IntWritable(1);  
 private Text word = new Text();  
 public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {  
            String line = value.toString();  
  StringTokenizer tokenizer = new StringTokenizer(line);  
 while (tokenizer.hasMoreTokens()) {  
                word.set(tokenizer.nextToken());  
  context.write(word, one);  
  }  
        }  
    }  
    public static class Reduce extends Reducer<Text, IntWritable, Text, IntWritable> {  
        public void reduce(Text key, Iterable<IntWritable> values, Context context)  
                throws IOException, InterruptedException {  
            int sum = 0;  
 for (IntWritable val : values) {  
                sum += val.get();  
  }  
            context.write(key, new IntWritable(sum));  
  }  
    }  
    public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {  
        Configuration conf = new Configuration();  
  Job job = new Job(conf, "wordcount");  
  job.setJarByClass(WordCount.class);  
  job.setOutputKeyClass(Text.class);  
  job.setOutputValueClass(IntWritable.class);  
  job.setMapperClass(Map.class);  
  job.setReducerClass(Reduce.class);  
  job.setInputFormatClass(TextInputFormat.class);  
  job.setOutputFormatClass(TextOutputFormat.class);  
  FileInputFormat.addInputPath(job, new Path(args[0]));  
  FileOutputFormat.setOutputPath(job, new Path(args[1]));  
  job.waitForCompletion(true);  
  }  
}
```

###  빌드 과정

보통 artifact을 이용해서 jar파일을 생성하는데, 이 방법보다 Maven 빌드를 추천드립니다.

![enter image description here](https://lh3.googleusercontent.com/vRBfYK3ksq9SPVRjzljqdT8UPqZkmMHXBqcfrc_dfQdrElpVmtrcX-4Oybfk5GDOJzCIPxKdVePl)

위의 그림에서 보듯이 오른쪽 Maven 화면에서 빌드를 진행합니다. 보통 `Clean|Compile|Package`를 하여 jar 파일을 생성합니다.

### WordCount 실행

생성된 Jar파일을 제 맥북에서 분산 환경으로 옮겨 놓습니다.

![enter image description here](https://lh3.googleusercontent.com/-u2XY9mXEk7Qczo192s__MsjWW181HkdLzMMTGYBhYSd0ZKX2kgXsf30wK7w6tjGFbn_haSxnKL2)

하둡을 이용해서 Jar파일을 실행합니다. 이때 제가 만든 Jar의 클래스 이름이 WordCount이기 때문에 명령어의 인자에 WordCount를 넣어주었습니다.

![enter image description here](https://lh3.googleusercontent.com/ggatE9OImT-DFcBID1Hywc7aC9gzNXL4ruVdmT392FKwgbLGjm7pXSbIP2F9llq7IoJY7yowo4yq)

맵리듀스가 실행됩니다. 그리고 해당 디렉토리로 가니 생성된 파일을 확인해 볼 수 있습니다.

![enter image description here](https://lh3.googleusercontent.com/apX0BXYPBgGr1wzlXSfCwzntAUmHj4QUPd9ekTO1SaibJTi5Y21RdAncMoSjbgkDI93FAIBs0iFv)

---
참고 문서
1. [http://msoliman.me/2017/04/24/first-map-reduce-inside-intellij-with-docker-container/](http://msoliman.me/2017/04/24/first-map-reduce-inside-intellij-with-docker-container/)
