##   LabelEoncoder in Python3

여러개의 columns이 있는 data frame에서 label이 있는 상태로 Encoding 하는 방법.

예를 들어 아래와 같이 dataFrame을 생성합니다.
```
from sklearn import preprocessing
df = pd.DataFrame({'pets':['cat', 'dog', 'cat', 'monkey', 'dog', 'dog'], 'owner':['Champ', 'Ron', 'Brick', 'Champ', 'Veronica', 'Ron'], 'location':['San_Diego', 'New_York', 'New_York', 'San_Diego', 'San_Diego', 'New_York']})
```

생성한 dataFrame에서 LabelEncoder를 사용하여 Encoding을 해줍니다.
```
df.apply(LabelEncoder().fit_transform)
```

#### 변환 전
![enter image description here](https://lh3.googleusercontent.com/-_cGHwpuDdxNrkzspjBrsW_vS3Lgr8oO-dCjPwYJh_xVDcMl44MNN6cgYKa-REMPK1UOtWZrUBFg)

#### 변환 후
![enter image description here](https://lh3.googleusercontent.com/Nf1zHKGJszgDcSK--DOeUqSwb_juu5klanaig5UuvYr7leIeSrz_42eit2WdfNJSOyhailg_OFvR)

Columns의 값들이 label이 된 것을 확인할 수 있습니다.

