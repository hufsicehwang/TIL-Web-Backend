## 🦢 Transaction 특징

- 원자성(Atomicity)
    
    > 트랜잭션이 DB에 모두 반영되거나, 혹은 전혀 반영되지 않아야 된다.
    > 
- 일관성(Consistency)
    
    > 트랜잭션의 작업 처리 결과는 항상 일관성 있어야 한다.
    > 
- 독립성(Isolation)
    
    > 둘 이상의 트랜잭션이 동시에 병행 실행되고 있을 때, 어떤 트랜잭션도 다른 트랜잭션 연산에 끼어들 수 없다.
    > 
- 지속성(Durability)
    
    > 트랜잭션이 성공적으로 완료되었으면, 결과는 영구적으로 반영되어야 한다.
    > 

## 🐱 Django의 transaction

: Transaction의 결과는 commit(transaction을 DB에 반영)하거나 rollback(D 반영 취소) 해야 한다.

사용자가 commit과 rollback 구간을 수동적으로 관리하게 된다면 상당히 번거롭기 때문에 장고에서는 `Auto Commit` 기능을 제공한다.

> 장고는 `Auto Commit`을 기본값으로 제공하고 있습니다. 즉,  insert, update와 같은 문장을 바로 DB에 commit을 진행한다.
> 

## 🐲 Transaction 관리

- transaction은 간 DB와 connection를 지속적으로 확인하며 유지해야 한다.
- transaction 자체를 활성, 비활성 구분 해야 한다.
- transaction의 결과는 commit 되거나 rollback 되어야 한다.
- transaction은 save point를 지정하고 관리 할 수 있어야 한다.

⇒ 이것들을 다 고려하며 transaction를 관리하기에는 코드가 복잡해지고 관리가 힘들기 때문에 명시적으로 관리하기 위해 우리는 `transaction.atomic()`를 이용 할 수 있다.

## 🦓 수동 Rollback 함수

1. `set_rollback(rollback, using)`  : 수동 rollback
    - `rollback=True` ⇒ 가장 안쪽 atomic를 rollback
    - `rollback=False` ⇒ transaction 전체를 rollback
    - `using=`  ⇒ `atomic(using=)`과 DB를 맞춰 줘야함
2. `transaction.rollback()` : transaction 전체를 rollback
3. `transaction.savepoint_rollback(sid)` : 특정 sid(save point) 까지 rollback 

## 🦁 transaction 관련 함수

1. `commit()` - 수동 commit
2. `savepoint()` - 수동 관리
3. `savepoint_commit(sid)` - savepoint 별로 수동 commit

## 🐣 transaction.atomic()

- `atomic()`는 블록이 정상적으로 종료되었는지 아니면 예외적으로 종료되었는지 확인하여 commit, rollback 결정한다.
- `atomic()` 자체가 해당 구간 안에서 비정상 종료되면 rollback 해준다 ⇒ try-except 역할
- `atomic()` 시작과 함께 새로운 transaction이 활성화 되며 save point를 저장한다.
- `atomic(using=)` 옵션과 내부에 objects`using`의 DB를 맞춰 주면 rollback 가능
- try-except을 사용하지 않고 atomic() 구간을 중첩해도 최상단 atomic까지 rollback 한다.

### WHY? ⇒ atomic 구간 내부에서 왜 try-except을 사용하지 않아야 할까?

- [https://docs.djangoproject.com/en/4.1/topics/db/transactions/](https://docs.djangoproject.com/en/4.1/topics/db/transactions/)

: django 공식 reference에도 나와 있듯이 `atomic`  구간 내부에서는 try-except를 지양 해야 한다. 

이유는 except 구문에서 예외를 숨길 수 있고 이로인해 예기치 않은 동작이 발생 할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/535af82e-7cc9-4a4f-86f4-52a4d91a5fc0/Untitled.png)

### WHY ⇒ 예외를 숨기지 않고 잘 처리 한다면 문제가 안되지 않을까?

:  예외를 잘 처리하여 rollback을 수행 한다고 해도 rollback 작업을 수행하기 전 이전 값을 읽어 들일 수 있다.

## Extra
1. debug log 양식 예시 : request.data['debug_log'] = 'In function : create_collections / Operlation : create CollectionChild’ 
2. update와 delete 연산 시 transaction.atomic(using=DB_ZZ_W)와 내부 using(DB_ZZ_W)

