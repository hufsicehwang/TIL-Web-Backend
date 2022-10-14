## 🐳 django의 transaction
- Django에서 한 개이상의 쿼리를 트랜잭션으로 handling하는 방법은 크게 두 가지이다.
  - ATOMIC_REQUESTS 를 이용하여 request단위로 transaction을 묶는 방법
  - django.db.transaction 을 사용하여 명시적으로 transaction을 핸들링 하는 방법.


## 🐱 django autocommit
: Transaction의 결과는 commit(transaction을 DB에 반영)하거나 rollback(D 반영 취소) 해야 한다.

사용자가 commit과 rollback 구간을 수동적으로 관리하게 된다면 상당히 번거롭기 때문에 장고에서는 `Auto Commit` 기능을 제공한다.

> 장고는 `Auto Commit`을 기본값으로 제공하고 있습니다. 즉,  insert, update와 같은 문장을 바로 DB에 commit을 진행한다.
> 

## 🐲 transaction 관리

- transaction은 간 DB와 connection를 지속적으로 확인하며 유지해야 한다.
- transaction 자체를 활성, 비활성 구분 해야 한다.
- transaction의 결과는 commit 되거나 rollback 되어야 한다.
- transaction은 save point를 지정하고 관리 할 수 있어야 한다.

⇒ 이것들을 다 고려하며 transaction를 관리하기에는 코드가 복잡해지고 관리가 힘들기 때문에 명시적으로 관리하기 위해 우리는 `transaction.atomic()`를 이용 할 수 있다.

## 🐣 transaction.atomic()

- `atomic()`는 블록이 정상적으로 종료되었는지 아니면 예외적으로 종료되었는지 확인하여 commit, rollback 결정
- `atomic()` 자체가 해당 구간 안에서 비정상 종료되면 rollback 해준다 ⇒ try-except 역할
- `atomic()` 시작과 함께 새로운 transaction이 활성화 되며 save point를 저장
- `atomic(using=)` 옵션과 내부에 objects`using`의 DB를 맞춰 주면 rollback 가능

## 🦓 수동 Rollback 함수

1. `set_rollback(True)`  : 가장 안쪽 atomic block을 종료할 때 atomic 하나 만 rollback
2. `transaction.rollback()` : transaction 전체를 rollback
3. `transaction.savepoint_rollback(sid)` : 특정 sid(save point) 까지 rollback 

## 🦁 transaction 관련 함수

1. `commit()` - 수동 commit
2. `savepoint()` - 수동 관리
3. `savepoint_commit(sid)` - savepoint 별로 수동 commit

### WHY? ⇒ atomic 구간 내부에서 왜 try-except을 사용하지 않아야 할까?

: django 공식 reference에도 나와 있듯이 `atomic`  구간 내부에서는 try-except를 지양 해야 한다. 

이유는 except 구문에서 예외를 숨길 수 있고 이로인해 예기치 않은 동작이 발생 할 수 있다.

### WHY ⇒ 예외를 숨기지 않고 잘 처리 한다면 문제가 안되지 않을까?

:  예외를 잘 처리하여 rollback을 수행 한다고 해도 rollback 작업을 수행하기 전 이전 값을 읽어 들일 수 있다.

## 🦄 현재 코드에서 해결 할 수 있는 방법

1. transaction.atomic 마다 savepoint를 작성해 try-except 구문에서 수동으로 해당 지점까지 savepoint_rollback(sid)

- 함수 간 save point를 다 전달해 줘야함
1. 가장 하위 atomic 구문을 제거하여 가장 상단 atomic 까지 자동 rollback
