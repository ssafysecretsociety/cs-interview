# Transaction

## 트랜젝션이란?

> - 더이상 분할이 불가능한 업무처리의 단위
> - 데이터베이스의 상태를 변화시키기 위해서 수행하는 논리적인 작업의 단위
> - 수행 되어야 할 일련의 연산 모음

트랜잭션은 작업의 완전성을 보장한다. 한 단위의 작업을 모두 완벽하게 처리`[Commit]`하거나, 처리하지 못할 경우에는 원래 상태로 복구`[Rollback]` 함으로써 완전성을 보장할 수 있다.

## 트랜잭션 연산

> Transaction control language `TCL`  
> 트랜잭션을 제어하는 언어

<br>

### Commit

완료된 작업이 전혀 문제가 없다고 판단되었을 경우 Commit을 통해 트랜잭션을 완료한다.

- 변경사항이 데이터베이스에 `반영`됩니다.
- 이전 데이터로 `Rollback`할 수 없습니다.
- `Lock`이 풀려 다른 사용자들이 해당 행을 조작할 수 있습니다.
- 하나의 트랜잭션이 `종료` 됨을 의미한다.

<br>

### Rollback

Commit 이전에 변경사항을 취소할때 Rollback을 이용한다.

- 데이터 변경사항은 `취소`된다.
- 이전 Commit 혹은 Save Point 지점으로 `재저장`된다.
- `Lock`이 풀려 다른 사용자들이 해당 행을 조작할 수 있습니다.

<br>

### Save Point

사용자가 Commit 이전에 Rollback 지점을 정의한다.

- Save Point는 `다수`가 생길 수 있다.
- 특정 Save Point로 Rollback을 실행했다면 `그 뒤에 나오는 Save Point는` 사라진다.
- Save Point없이 Rollback을 시도하면 이전 `Commit 시점`으로 Rollback된다.

<br> 
<br>

```
DB마다 DDL 명령어 수행시 Commit 되는 auto로 실행될 수 있기 때문에 사용자가 임의로 Commit을 수행해줘야 하거나 그렇지 않은 경우가 있다.
```

<br>

## 트랜잭션 상태와 동작

![트랜잭션 동작](./transaction/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98_%EC%83%81%ED%83%9C%EC%A0%84%EC%9D%B4%EB%8F%84.png)

### 트랜잭션 상태

- Active: 트랜잭션 작업 실행 상태

  - `Active -> Partially Committed`  
    설계된 작업까지는 모두 성공했을때 동작이 일어난다

  - `Active -> Failed`  
    트랜잭션 실행에 오류가 생겨 중단될때 동작이 일어난다

<br>

- Partially committed: 트랜잭션의 연산은은 완료되고 Commit연산이 실행되기 전의 상태, 데이터베이스 반영 직전의 상태

  - `Partially Committed -> Committed`  
    설계된 작업까지는 모두 성공했을때 동작이 일어난다

  - `Partially Committed -> Failed`  
    트랜잭션에 Rollback연산을 수행하여 그 상태를 철회하는 동작

<br>

- Failed: 트랜잭션 실행에 오류가 발생하여 중단된 상태

  - `Failed -> Aborted`  
    트랜잭션에 Rollback연산을 수행하여 그 상태를 철회하는 동작

<br>

- Committed: 연산과 Commit이 성공적으로 수행된 상태

<br>

- Abort: Rollback연산을 수행하여 철회한 상태

<br>

## 트랜잭션 특징

> 트랜잭션의 4가지 특징 `ACID`

- 원자성`[Atomicity]`: 트랜잭션에서 정의된 연산들은 모두 성공적으로 실행되던지 아니면 전혀 실행되지 않은 상태로 남아있어야 한다. 하나의 트랜잭션에서 일부 연산만 실행되면 안된다.

<br>

- 일관성`[Consistency]`: 트랜잭션이 완료된 상태에서 이전과 동일하게 데이터의 일관성이 있어야 한다.

<br>

- 독립성 `[Isolation]`: 트랜잭션이 실행되는 도중에 다른 트랜잭션의 영향을 받으면 안된다. 트랜잭션은 독립적으로 수행되어야 한다

<br>

- 지속성 `[Durability]`: 트랜잭션이 성공적으로 수행되면 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적으로 반영되어야 한다.

<br>

## Lock

> 데이터의 일관성을 보장하기 위한 방법 + 동시성 제한
> Lock은 여러 커넥션에서 한 자원에 동시에 연산을 요청할 경우 한 커넥션에서만 연산을 수행할 수 있게 해주는 역할이다.

<br>

### Lock의 종류

- Shared Lock

  - 데이터를 `읽는 연산`에서 사용
  - 한 커넥션에서 Lock을 걸었어도 다른 커넥션에서 `읽을 수 있다`.
  - 다른 커넥션은 확인만 가능하고 변경할 수는 없다.

- Exclusive Lock
  - 데이터를 `변경 연산`에서 사용
  - Lock이 해제되기 전까지 다른 Shared Lock도 사용 불가하며` 읽기 쓰기도 불가능` 하다.

<br>

## 교착상태

> 여러개의 트랜잭션이 실행을 하지 못하고 서로 무한정 기다리는 상태` [Deadlock]`  
> 한 트랜젝션이 Lock을 획득한 채 다른 트랜잭션이 소유하고 있는 잠금을 요구하면 아무리 기다려도 상황이 바뀌지 않는 상

### 교착상태 방지

- 트랜잭션을 `작은 단위`로 Commit
- 정해진 `순서`로 커넥트
- 테이블 단위로 잠금 -> `동시성 감소, 방지효과 증가`

<br>

## MySQL 트랜잭션 예외 명령어

```
DDL: CREATE, DROP, ALTER, RENAME, TRUNCATE
```
