# 🐲ORM이란?
: ORM은 object relational mapping의 약자로 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 것을 말한다.
객체 간의 관계를 바탕으로 SQL을 자동으로 생성해서 sql쿼리문 없이도 데이터베이스의 데이터를 다룰 수 있게 해준다.

# ⭐Lazy-loading(지연로딩)
: ORM을 사용해서 입력한다고 해서 바로 SQL로 치환하여 동작하지 않고 실제로 데이터를 가져와야하는 부분에서 필요한 데이터를 가져오도록 동작한다.
> ORM을 SQL로 치환하기 위해 최대한 __지연한다!!!__
>> 변수에 어사인되는 등 실제로 데이터를 사용하는 부분에서 사용된다!!

### 지연로딩 예시1
```python
  users = User.objects.filter(first_name='kim')  ---a 
  first_user = users[0].full_name                ---b
  user_list = list(users)                        ---c
```
: ORM -> SQL로의 변환은 실제로 데이터를 어사인하는 부분에서 사용된다. 따라서 위의 코드에서 `b`,`c`에서 SQL이 두번 실행된다.

### 지연로딩 예시2
```python
  users = User.objects.filter(first_name='kim')  ---a 
  user_list = list(users)                        ---c
  first_user = user_list[0].full_name            ---b
```
: 하지만 `b`,`c`를 바꿔줌으로써 user object의 모든 내용들을 list에 담는 `C`에서 SQL이 한번 실행된다.

# 🔥Eager-loading(즉시로딩)
: 지금 당장 사용하지 않을 데이터도 즉시 가져와 caching하여 Lazy-loading의 N+1문제의 해결책으로 많이 사용하게 된다.
### `N+1` 문제
-  User모델과 User모델의 PK를 FK로 가지는 UserProfile 모델이 있다고 가정
-  김씨 성을 가진 user의 주소를 확인하기 위한 ORM
```python
  kims_users = User.objects.filter(first_name='kim')

  for user in kims_users:
    user_address = user.user_profile.home_address
    print(f'user address :: {user_address}')
```
```SQL
# kims_users를 불러오기위한 ORM 실행
SELECT * FROM user u WHERE first_name='kim'

SELECT up.home_address FROM user_profile up WHERE user_id=u.id=1
SELECT up.home_address FROM user_profile up WHERE user_id=u.id=2
SELECT up.home_address FROM user_profile up WHERE user_id=u.id=3
```
  - 3명의 김씨를 조회하기 위해 SQL이 4번 실행됨
  - 만약 100만명을 조회 하기 위해서는??

### select_related
- 정참조 관계(참조 테이블 -> 피참조 테이블)이거나 역참조 객체가 단일 객체 일때(one-to-one, many-to-one) 사용 가능
- `join`하여 필요한 데이터를 모두 캐싱 
```python
  kims_users = User.objects.select_related('user_profile').filter(first_name='kim')

  for user in kims_users:
    user_address = user.user_profile.home_address
    print(f'user address :: {user_address}')
```
- `lazy-loading`과 다르게 `select_related` 순간 부터 즉시 로딩 함!
```SQL
# kims_users를 불러오기위한 ORM 실행
SELECT * FROM user u JOIN user_profile using(id) WHERE first_name='kim'
```

### prefetch_related
- 보통의 역참조 관계에서 사용(피참조 테이블 -> 참조 테이블)
- 2개의 테이블을 각각 불러들여 파이썬이 ORM을 처리하는 단계에서 결합하는 방식을 사용한다는 뜻
```python
  kims_users = User.objects.prefetch_related('user_profile').filter(first_name='kim')

  for user in kims_users:
    user_address = user.user_profile.home_address
    print(f'user address :: {user_address}')
```
- 비효율 적일 수 있으나 특정 상황에 유용하게 사용 할 수 있음
- __prefetch_related는 항상 추가쿼리가 발생 한다는 것을 염두 해야함!!__





---------------------------------------------------


```
출처 : https://velog.io/@emrrbs9090/Django-ORM%EC%9D%98-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC%EC%99%80-%ED%8A%B9%EC%A7%95
```
