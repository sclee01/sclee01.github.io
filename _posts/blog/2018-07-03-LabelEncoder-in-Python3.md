---
layout: post
title: Label encoding across multiple columns in python3
excerpt: "여러개의 컬럼을 레이블 인코딩 하는 방법"
date: 2018-07-03
modified: 2018-07-03
author: sclee
categories: blog
tags: [python3,scikit-learn]
image:
  url: http://i.imgur.com/p0TzIOM.gif
comments: true
share: true
---


<h2 id="labeleoncoder-in-python3">LabelEoncoder in Python3</h2>
<p>여러개의 columns이 있는 data frame에서 label이 있는 상태로 Encoding 하는 방법.</p>
<p>예를 들어 아래와 같이 dataFrame을 생성합니다.</p>
<pre><code>from sklearn import preprocessing
df = pd.DataFrame({'pets':['cat', 'dog', 'cat', 'monkey', 'dog', 'dog'], 'owner':['Champ', 'Ron', 'Brick', 'Champ', 'Veronica', 'Ron'], 'location':['San_Diego', 'New_York', 'New_York', 'San_Diego', 'San_Diego', 'New_York']})
</code></pre>
<p>생성한 dataFrame에서 LabelEncoder를 사용하여 Encoding을 해줍니다.</p>
<pre><code>df.apply(LabelEncoder().fit_transform)
</code></pre>
<h4 id="변환-전">변환 전</h4>
<p><img src="https://lh3.googleusercontent.com/-_cGHwpuDdxNrkzspjBrsW_vS3Lgr8oO-dCjPwYJh_xVDcMl44MNN6cgYKa-REMPK1UOtWZrUBFg" alt="enter image description here"></p>
<h4 id="변환-후">변환 후</h4>
<p><img src="https://lh3.googleusercontent.com/Nf1zHKGJszgDcSK--DOeUqSwb_juu5klanaig5UuvYr7leIeSrz_42eit2WdfNJSOyhailg_OFvR" alt="enter image description here"></p>
<p>Columns의 값들이 label이 된 것을 확인할 수 있습니다.</p>

