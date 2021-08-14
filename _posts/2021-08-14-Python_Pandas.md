Python - Pandas 의 이해
==================



### Series

------

+ numpy의 ndarrary를 기반으로 인덱싱을 추가한 1차원 배열

  ```
  s5 = pd.sSeries(([1,2,3], ['a', 'b', 'c']), dtype=int16)
                    data         index          data type
  ```

  + Functions

    > .size( ) : 개수								.unique( ) : 유일갑만 ndarrary로 반환 
    >
    > .shape( ) : 튜플형태 shape 		.count( ) :  갯수(NaN 제외)
    >
    > .mean( ) : 평균(NaN 제외)		   .value_counts( ) : 값들의 빈도 (NaN 제외)
    >
    > .head( ) : 상위 몇개 row를 보여줄지 default 는 5개, 하위는 .tail( )
    >
    > .index( ) : index 정보 

  + 산술연산 

    > Series 간의 연산은 각 인덱스에 맞는 값끼리 연산되므로, pair가 맞지 않으면 결과는 NaN
    >
    > S1 ** S2               S1**2

  + Boolean Selection 

    > [ ]와 함께 사용되면 True 값에 해당하는값만 새로 반환 되는 Series 객체에 표함됨
    >
    > 다중 조건의 경우 '&' , '|'  사용해 연결 가능

    ```python
     s = pd.Series(np.arrange(10), np.arrange(10)+1)
       s > 5 		s[s>5]		(s>=7).sum()
       s.drop(5, inpalce=true)   //S 에서 5 index와 data 삭제 
    ```

  + Slicing

    > 문자 index 라도 순서상 동작 가능 
    >
    > S[1:3]       // (1)부터 (3-1)번까지의 index 순서에 해당하는 값 반환 



### Data Frame

------

- Series가 1차원이라면 Data Frame은 2차원 확대 버전

  - df 파악하기 위한 기본 함수 

    > head/tail() -> shape() -> describe() -> info() 
    >
    > x.index -> x.column 

  - read_csv함수 파라메터 

    > index_col : index 지정 			usecols : 사용할 컬럼 지정 

  - 복수의 컬럼 선택하기 

    ```python
    train_data[['Survived', 'Name', 'Age']]
    ```

  - dataframe slicing 

    ```
    train_data[:10]    #0~9까지 보여줌 
    
    train_data.index = np.arrange(100, 991)
    train_data.loc(986)		#index 986인 data 정보 
    train_data.iloc(10)		#순서상 10번째 data 정보 
    
    #Pclass 가 1인 고객 중 나이가 30대인 data 조건 
    class_ = train_data['Pclass'] == 1
    age_ = (train_data['Age'] > 30) & (train_data['Age']<40)
    train_data[class_ & age_]
    
    #3번째 컬럼에 마지막 파라메터의 연산결과를 'Fare10' 칼럼으로 추가 
    train_data.insert[3, 'Fare10', train_data['Fare']/10]
    ```

- Corr 함수 - 변수(Column)들 사이의 상관계수 (Correlation)	   

  ```
  # -1과 1사이의 값
  train_data.corr() 
  plt.matshow(train_data.corr())
  ```

- NaN 처리 

  1.  info ( ) - Colomn 정보 확인  

  2.  isna ( ) - boolean 타입으로 확인, True는 NaN임 

     ```python
     trian_data['Age'].isna()
     ```

     

  3.  dropna( ) - NaN 포함 칼럼 삭제 

     ```
     train_data.dropna(subset = ['Age', 'Cabin'])
     train_data.dropna(axis=1)
     ```

     

  4.  fillna( ) - NaN을 채워넣기 

     ```
     train_data['Age'].fillna(train_data['Age'].mean()
     ```

- 숫자 & 범주형 data 타입 변경

  - astype( ) - 타입 변경하기 

    ```
    train_data['Pclass'] = train_data['Pclass'].astype(str)
    ```

  - apply( ) - 변환 로직을 함수로 만든 후 적용 

    ```python
    #Age 를 10대, 20대... 로 분류 
    def age_categorize(age):
    	if math.isna(age):
            reture -1
        return math.floor(age/10)*10
    
    train_data['Age'].apply(age_categorize)
    ```

  - One-hot encoding (범주 --> 숫자로)

    ```
    #각 범주(category)를 column 레벨로 변경 
    pd.get = dummies(train_data, column['Sex'], drop_first=True)
    ```

- groupby ( ) - 그룹화 통해 데이터 분할, operation 적용, 데이터 병합 

  ```python
  gender_group = df.groupby('Sex')
  gender_group.groups
  ```

  - Grouping 함수 

    > #NaN은 제외하고 연산 
    >
    > .count()			.sum()			.mean()		.std()		.var()		.min()		.max()

    ```
    df.groupby(['Pclass','Sex']).mean()['Survived']
    
    df.set_index(['Pclass', 'Sex']).reset_index()
    
    df.set_index('Age').groupby(age_categorize).mean()
    
    ~.aggregate([np.mean, np.sum, np.var])
    ```

- transform( ) - groupby 이후 transform 함수로 원래 index 유지한 채로 통계함수 사용 가능, 전체 data 아닌 그룹에서의 집계 계산 
  - Pivot( ) 

    ```
    #'요일'은 index 로, '지역'은 column 으로 지정 
    df.pivot('요일', '지역')
    
    # aggfunc(=np.mean)로 NaN은 평균으로 채움 
    pd = pivot_table(df, index = '요일', index='지역', aggfunc(=np.mean))
    ```

  - value_counts() - 열의 고유값 빈도 

  - sort_values(ascending = False) - 내림차순 정렬

  - stack/unstak - stack 컬럼 레벨에서 인덱스 레벨로 dataframe 변경 

    ```python
    # (0) 첫번째 index를 column으로 변경, (1)두번째를 index 로 지정	 
    new_df.unstack(0).stack(1)
    ```

    

  - concat 

    ```python
    # axis =1 칼럼 기준으로 df1과 df2 연결 
    pd.concat([df1, df2], axis=1)
    ```

  - merge/join

    ```python
    #'inner' | 'left' | 'right' | 'outer' 선택 가능 
    pd.merge(customer, orders, on='customer_id', now='inner')
    
    #기본적으로 index를 사용하여 left join
    cust1.join(order, how='inner')
    ```

- quantie( ) - 0:min, 1:max, default : 중위값,  ex)0.95 : 상위 5%