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


기말 --------------------------------------------------------------------------

해당 칼럼으로 정렬 (오름차순 정렬)
df.sort_values(by='성')

해당 칼럼으로 정렬 (내림차순 정렬)
df.sort_values(by='성', ascending=False)

여러 칼럼으로 정렬 
df.sort_values(by=['성','이름'], ascending=[True,False])

행 추가
df = df.append(person, ignore_index=True)

행 삭제
df.drop(index=3, inplace=True)

정렬한 뒤 그 순서 저장
df.sort_values(by=['성','이름'], ascending=[False, True], inplace=True)

하나의 칼럼만을 정렬해서 보고 싶으면
df['성'].sort_values()

가장 큰 값
df['SalaryUSD'].nlargest(10)

가장 큰 값과 다른 칼럼의 값들도 보고 싶으면
df.nlargest(10, 'SalaryUSD')[['Country','EdLevel','SalaryUSD']]

값을 순서대로 정렬 했을 때, 그 중 중앙값
df['ConvertedComp'].median()

값의 평균
df['ConvertedComp'].mean()

데이터 요약
df.describe()

행 갯수
df['ConvertedComp'].count()

중복 행 체크
df['Hobbyist'].value_counts()

비율 표시
df['Hobbyist'].value_counts(normalize=True)

그룹으로 묶기
country_grp = df.groupby(['Country'])

그룹으로 묶은 것 중에서 하나의 그룹을 가져오기
country_grp.get_group('United States')

그룹으로 묶은 것 중 하나의 칼럼의 중복 체크
country_grp['SocialMedia'].value_counts().head(50)

그렇게 중복값을 체크 한 것 중 하나의 값 알아보기
country_grp['SocialMedia'].value_counts().loc['South Korea']
country_grp['SocialMedia'].value_counts().loc[['South Korea','Republic of Korea']]

위의 그룹을 filt로 묶어서도 가능
filt = df['Country'] == 'India'
df.loc[filt, 'SocialMedia'].value_counts()

그룹으로 묶은 값중 중간값 중에서 한국만 걸러내기
country_grp['ConvertedComp'].median().loc['South Korea']

평균값과 중간값 한꺼번에 표시하기
country_grp['ConvertedComp'].agg(['median', 'mean'])

그 중 캐나다만 뽑아내기
country_grp['ConvertedComp'].agg(['median', 'mean']).loc['Canada']

해당 필터링 중, 파이썬을 포함하고 있다면 true 아니면 false
df[filt]['LanguageWorkedWith'].str.contains('Python')

의 총 갯수
df[filt]['LanguageWorkedWith'].str.contains('Python').sum()

그룹화 하기
country_grp['LanguageWorkedWith']

각 항들이 파이썬을 얼마나 가지고 있는지
country_grp['LanguageWorkedWith'].apply(lambda x : x.str.contains('Python').sum())

두 데이터 프레임 합치기(칼럼이 다르면 이상해짐)
concat_df = pd.concat([country_respondents, country_python_uses])

칼럼에 맞춰 데이터 프레임 합치기
concat_df = pd.concat([country_respondents, country_python_uses], axis='columns')

칼럼 이름 바꾸기
concat_df.rename(columns = {'Country' : 'NumRespondents', 'LanguageWorkedWith' : 'NumKnowsPython'},inplace=True)

나라별 학위 소유자 
country_grp2=df.groupby(['Country'])
CountryNum = df['Country'].value_counts()
BachelorNum=country_grp2['EdLevel'].apply(lambda x : x.str.contains('Bachelor').sum())

null값 제거 (NaN)
df.dropna()

모든 값이 빠진 행이 있다면 삭제
df.dropna(axis='index', how='all')

하나라도 빠진 행이 있다면 삭제
df.dropna(axis='index', how='any')

모든 값이 빠진 열이 있다면 삭제
df.dropna(axis='columns', how='all')

하나라도 빠진 행이 있다면 삭제
df.dropna(axis='columns', how='any')

email 칼럼 값이 없는 행만 드롭
df.dropna(axis='index', subset=['email'])

last와 email 값이 모두 빠진 경우에만 삭제
df.dropna(axis='index', subset=['email','last'], how='all')

NA라는 값을 np.nan으로 변경
df.replace('NA', np.nan, inplace=True)

na를 모두 0으로
df.fillna(0)

데이터 타입 변경
df2['age'].astype(int)
df2['age'] = df2['age'].astype(int)

유일한 값 찾기
df['YearsCode'].unique()

데이텉 타입 변경
df['YearsCode'] = df['YearsCode'].astype(float)

여러 조건으로 검색
filt = (tips['time'] == 'Dinner') & (tips['tip'] > 5.00)
tips[filt]

null 검사
frame['col2'].isna()

notnull이 포함된 값 수 구하기
tips.groupby('sex').size()
count안 쓴 이유는 not null의 수를 반환하기 때문

해당 그룹의 편균과 사이트
tips.groupby('day').agg({'tip' : np.mean, 'day' : np.size})

2개의 조건을 가진 그룹이 2개의 분류로 나뉨
ips.groupby(['smoker','day']).agg({'tip' : [np.size, np.mean]})

merge(key 칼럼을 중심으로 합치기)
pd.merge(df1, df2, on='key')

레프트 조인
pd.merge(df1, df2, on='key', how='left')

라이트 조인
pd.merge(df1, df2, on='key', how='right')

아우터 조인
pd.merge(df1, df2, on='key', how='outer')

데이터프레임 합쳤을 시 중복 값 제거
pd.concat([df1,df2]).drop_duplicates()



```
