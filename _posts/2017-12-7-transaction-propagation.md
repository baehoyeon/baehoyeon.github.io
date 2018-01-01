---
layout: post
title: "[ISSUE] Transactional RollBackException"
author: "Qoo"
category: Issue
---

# Spring Transactional RollBackException
## 에러정보
```java
org.springframework.transaction.HeuristicCompletionException : Heuristic completion: outcome state is rolled back; nested exception is org.springframework.transaction.TransactionSystemException: Could not commit JPA transaction; nested exception is javax.persistence.RollbackException: Transaction marked as rollbackOnly
```


## 재현
```java
@Transactional
public class UserApplication {

	@Autowired
	private UserService userService;

	public void saveUser(){
		if(userService.isNotExistUser(1L)) {
			userService.saveUser(new User());
		}
	}
}
```
유저가 없을때만 저장하는 코드가 있다고 하자.

```java
@Slf4j
@Transactional
public class UserService {

	@Autowired
	private UserRepository userRepository;

	public boolean isNotExistUser(Long userId){
		try{
			throw new NullPointerException("없음");
		} catch (Exception e){
			log.info("user is null");
			return true;
		}
	}


	public void saveUser(User user){
		userRepository.save(user);
	}
}

```
isNotExistUser 에서는 NullPointerException 을 통해서 boolean 값을 return 한다고 가정했다.

이렇게 처리를 하게되면 재현을 할 수 있다.

이런 에러는 왜 발생하게 되는것인가?

## 원인 분석
기본적으로 
@Transactional Propagation 은 TransactionDefinition.PROPAGATION_REQUIRED 가 default 이다.

이 속성은 미리 시작된 Transaction 이 있다면 참여하고, 미리 시작된 Transaction 이 없다면 새로 시작한다.

그렇기 문에 UserService 에서는  UserApplication 에서 시작한 Transaction 을 그대로 사용하게 된다.

UserService 의 isNotExistUser 에서 NullPointerException 이 발생하면서 UserApplication 에서 시작한

Transaction 은 rollBackOnly 로 설정된다.

하지만 Exception 을 먹어버리게 코딩이 되어있었기 때문에 문제없이 코드는 계속 진행된다.

하지만 user 를 save 하고 Transaction 을 종료하고 commit 하려고 했지만, 
Transaction 은 rollBackOnly 상태이기 때문에 Exception 이 발생한 것이다.

