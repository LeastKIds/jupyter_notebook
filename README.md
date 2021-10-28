# jupyter_notebook





```python
Pandas 사용
import pandas as pd

데이터 프레임 만들기
people = {
    "first" : ["길동", "춘향", "몽룡"],
    "last" : ["홍", "성", "이"],
    "email" : ["gdhong@gmail.com", "chsung@gmail.com", "mylee@gmail.com"]
}
df = pd.DataFrame(people)

원하는 행들을 보기
df[["email","last"]]

0번 index 보기
df.loc[0]

하나의 행과 열만 보기
df.loc[0,'first']

여러개의 index 보기
df.loc[[0,1]]

여러개의 index와 열 보기
df.loc[[0,1], ['last','email']]

특정 열을 index로 설정 (원본에 영향)
df.set_index('email', inplace=True)

설정된 index의 모든 결과값 출력
df.index

iloc의 경우 안에 숫자만 들어갈 수 있음. 수식이나, 문자열을 넣고 싶으면 loc 이용
df.iloc[0,0] -> '길동'

index 정렬하기
df.sort_index(inplace=True)
df.sort_index(ascending=False, inplace=True) (역순 정렬)


두 번째 index
df.index[2]

두 번째 index(0,1,2)의 뒤에서 첫 번째 열
df.iloc[2,-1]

index 0,1,2번 보기
df.iloc[[0,1,2]]

index 0,1 그리고 2번 행을 보기
df.iloc[[0,1],2]

index 초기화
df.reset_index(inplace=True)

성이 '성'인 사람의 결과물
df[df['last'] == '성']

변수로 저장해서 사용할 수 도 있음
filt = (df['last'] == '성')
df[filt]
같은 결과물
df[filt]['first']
df.loc[filt, 'first']

해당 결과값을 제외한
df.loc[~filt, 'email']

csv파일 읽기
df = pd.read_csv('fortune500.csv')

출력될 표의 크기 설정
pd.set_option("display.max_columns", 100)
pd.set_option("display.max_rows",20)

끝의 20개만 보여주기
df.tail(20)

앞의 20개만 보여주기
df.head(20)

타입확인하기 (행이 하나면 시리즈, 여러개면 데이터프레임)
type(df["Company"])

__DataFrame__ 의 각 칼럼은 __Serise__이다.
![Serise]()

'Age'의 최대값, 최소값, 평균
df["Age"].max()
df["Age"].min()
df["Age"].mean() (평균)

칼럼명 보기
df.columns

칼럼명 변경
df.columns = ['first_name', 'last_name', 'email']
df.rename(columns={"FIRST_NAME" : "first", "LAST_NAME" : "last"}, inplace=True)
df.rename(columns={'EMAIL' : 'email'}, inplace=True)

칼럼명 모두 대문자
df.columns = [x.upper() for x in df.columns]

칼럼명에서 특정 문자 변경
df.columns = [x.replace("-", "") for x in df.columns]
df.columns = df.columns.str.replace('-','')

칼럼명 모두 소문자
df.columns = [x.lower() for x in df.columns]

해당 열 변경
df.loc[2] = ['몽룡', '이', 'melee@yju.ac.kr']

해당 열 부분 변경
df.loc[2,['last','EMAIL']] = ['Lee', 'mrlee@gmail.com']

수식을 활용해 변경도 가능
filt = df['first'] == '몽룡'
df.loc[filt,['last','email']] = ['Park','mrpark@gmail.com']
df

해당 열 대문자로 바꾸기
df['last'] = df['last'].str.upper()4

해당 열 소문자로 바꾸기
df['last'] = df['last'].str.lower()

해당 열 글자 수
df['last'].str.len()

apply는 함수 적용
def upper(strval):
    return strval.upper()
def lower(strval):
    return strval.lower()
  
 df['last'].apply(upper)
df['last'].apply(lower)
df['last'] = df['last'].apply(lower)

해당 열 삽입
df.loc[3] = ['chunHyang',None,'chsung@gmail.com']

각 행들 크기
df.apply(len)

각 열 크기
df['first'].count()
다만 None의 경우는 세지 않음

각 행의 크기 (가로)('first', 'last', 'email')
df.apply(len, axis='columns')

디폴트값은 index값
df.apply(len, axis='index')

apply를 이용해 각 열들의 최소값을 구할 시 None이 없어야 한다
df.apply(min)

각 시리즈마다 적응
df.apply(lambda x:x.min())

각 행 열의 값들을 하나하나 표현 (dataframe에만 적용가능)
df.applymap(len)

맵을 사용해 열들을 수정가능(제외단 값들은 None으로 바뀜)
df['first'] = df['first'].map({'길동' : 'Gildong', '춘향' : 'ChunHyang'})

replace를 사용해 열들을 수정가능 (없는 값은 수정되지 않음)
df['first'] = df['first'].replace({'길동' : 'Gildong', '춘향' : 'ChunHyang'})


해당 행을 기준으로 정렬
df.sort_values(by='성')

내림차순으로
df.sort_values(by='성', ascending=False)

기준이 여러개도 가능
df.sort_values(by=['성','이름'], ascending=[True,False])

데이트프레임을 추가함
person = {'이름' : '준표', '성' : '홍', '이메일' : 'jphong@gmail.com'}
df = df.append(person, ignore_index=True)

데이터프레임에서 열 삭제
df.drop(index=3, inplace=True)

데이터프레임 정렬 변경
df.sort_values(by=['성','이름'], ascending=[False, True], inplace=True)

인덱스 기준 정렬
df.sort_index()

하나의 칼럼만 정렬
df['성'].sort_values()

csv파일 읽기 (index_col : 인덱스 설정)
df = pd.read_csv('./survey/survey_results_public.csv', index_col = 'Respondent')

가장 큰 10개의 값
df['SalaryUSD'].nlargest(10)

다른 정보와 함께
df.nlargest(10, 'SalaryUSD')[['Country','EdLevel','SalaryUSD']]

이런식으로 출력도 가능
df['이름'] + ' ' + df['성']

새로운 행을 만들기
df['full_name'] = df['이름'] + ' ' + df['성']

칼럼삭제
df.drop(columns=['이름','성'],inplace=True)

하나으 칼럼 2개로 나누기 (칼럼명은 따로 정하고 데이터가 나누어 지는 거)
df[['first', 'last']] = df['full_name'].str.split(' ', expand=True)

나누어진 칼럼에 값 추가하기
df = df.append({'first' : 'chanho', 'last' : 'Park'}, ignore_index=True)

```
