---
layout: post
title: "[Java] Java 8 Stream Data Processing Example"
author: "Qoopooh"
category: Java
---

# Java 8 Stream Data Processing Example
## Setup project
### 1) gradle project dependency
```groovy
dependencies {
    compile group: 'org.projectlombok', name: 'lombok', version: '1.16.10'
    compile group: 'com.google.guava', name: 'guava', version: 'r05'
    testCompile group: 'org.easytesting', name: 'fest-assert', version: '1.4'

    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```

### 2) java class
#### User.java
```java
@Setter
@Getter
@Builder
@NoArgsConstructor
@AllArgsConstructor(access = AccessLevel.PRIVATE)
@ToString
public class User {
	private String id;
	private String name;
	private Position position;
	private int monthlySalary;

	public boolean isExecutive() {
		return EXECUTIVE.contains(this.position);
	}
}

```
#### Position.java
```java
public enum Position {
	CEO, MANAGING_DIRECTOR, MANAGER, STAFF;

	public static final List<Position> EXECUTIVE = ImmutableList.of(
		CEO, MANAGING_DIRECTOR
	);
}

```

#### 3) Init Data and Function
```java
public class StreamTest {

	private List<User> userList;

	@Before
	public void setUp() {
		userList = Lists.newArrayList(
			User.builder()
				.id("qoo")
				.name("qoo")
				.position(Position.CEO)
				.monthlySalary(100000000)
				.build(),
			User.builder()
				.id("peter")
				.name("peter")
				.position(Position.MANAGING_DIRECTOR)
				.monthlySalary(10000000)
				.build(),
			User.builder()
				.id("ann")
				.name("ann")
				.position(Position.MANAGER)
				.monthlySalary(5000000)
				.build(),
			User.builder()
				.id("mac")
				.name("mac")
				.position(Position.MANAGER)
				.monthlySalary(7000000)
				.build(),
			User.builder()
				.id("john")
				.name("john")
				.position(Position.STAFF)
				.monthlySalary(3000000)
				.build(),
			User.builder()
				.id("martin")
				.name("martin")
				.position(Position.STAFF)
				.monthlySalary(2500000)
				.build(),
			User.builder()
				.id("park")
				.name("park")
				.position(Position.STAFF)
				.monthlySalary(2300000)
				.build(),
			User.builder()
				.id("potter")
				.name("potter")
				.position(Position.STAFF)
				.monthlySalary(2100000)
				.build());
	}

	private void print(Object object) {
		System.out.println(object.toString());
	}
}
```


## 1. Max and Min
### Test Code
```java
	@Test
	public void maxAndMinByTest() {
		// given
		Comparator<User> monthlySalaryComparator =
			Comparator.comparingInt(User::getMonthlySalary);

		// 1. expect - max
		Optional<User> highestPaidUser = userList.stream()
			.max(monthlySalaryComparator);

		assertThat(highestPaidUser.get().getId()).isEqualTo("qoo");
		print(highestPaidUser.get());

		// same  method
		userList.stream()
			.collect(maxBy(monthlySalaryComparator));

		// 2. expect - min
		Optional<User> lowestPaidUser = userList.stream()
			.min(monthlySalaryComparator);

		assertThat(lowestPaidUser.get().getId()).isEqualTo("potter");
		print(lowestPaidUser.get());
		// same  method
		userList.stream()
			.collect(minBy(monthlySalaryComparator));
	}
```
### Console Result
```
User(id=qoo, name=qoo, position=CEO, monthlySalary=100000000)
User(id=potter, name=potter, position=STAFF, monthlySalary=2100000)
```

## 2. Reduce
### Test Code
```java
	@Test
	public void reduceTest() {
		// 1.  expect - totalSalary
		int totalSalary = userList.stream()
			.map(User::getMonthlySalary)
			.reduce(0, (x, y) -> x + y);
		
		assertThat(totalSalary).isEqualTo(131900000);
		print(totalSalary);

		// 2.  expect - totalSalary
		totalSalary = userList.stream()
			.map(User::getMonthlySalary)
			.reduce(0, Integer::sum);
		
		assertThat(totalSalary).isEqualTo(131900000);
		print(totalSalary);

		// 3. expect - fixedCharge
		int rent = 100000000;
		int fixedCharge = userList.stream()
			.map(User::getMonthlySalary)
			.reduce(rent, Integer::sum);
		assertThat(fixedCharge).isEqualTo(231900000);
		print(fixedCharge);
	}nt(fixedCharge);
	}
```

### Console Result
```java
131900000
131900000
231900000
```

## 3. Grouping
### Test Code
```java
	@Test
	public void groupingTest() {
		// when
		Map<Position, List<User>> usersByPosition = userList.stream()
			.collect(groupingBy(User::getPosition));

		// then - CEO
		assertThat(usersByPosition.get(Position.CEO))
			.hasSize(1);
		print(usersByPosition.get(Position.CEO));

		// then - MANAGING_DIRECTOR
		assertThat(usersByPosition.get(Position.MANAGING_DIRECTOR))
			.hasSize(1);
		print(usersByPosition.get(Position.MANAGING_DIRECTOR));

		// then - MANAGER
		assertThat(usersByPosition.get(Position.MANAGER))
			.hasSize(2);
		print(usersByPosition.get(Position.MANAGER));

		// then - STAFF
		assertThat(usersByPosition.get(Position.STAFF))
			.hasSize(4);
		print(usersByPosition.get(Position.STAFF));
	}
```

### Console Result
```
[User(id=qoo, name=qoo, position=CEO, monthlySalary=100000000)]
[User(id=peter, name=peter, position=MANAGING_DIRECTOR, monthlySalary=10000000)]
[User(id=ann, name=ann, position=MANAGER, monthlySalary=5000000), User(id=mac, name=mac, position=MANAGER, monthlySalary=7000000)]
[User(id=john, name=john, position=STAFF, monthlySalary=3000000), User(id=martin, name=martin, position=STAFF, monthlySalary=2500000), User(id=park, name=park, position=STAFF, monthlySalary=2300000), User(id=potter, name=potter, position=STAFF, monthlySalary=2100000)]
```

## 4. Grouping and Data Processing
### Test Code
```java
	@Test
  	public void groupingAndDataProcessingTest() {
  		// 1. when - topSalaryUserByPosition
  		Map<Position, Optional<User>> topSalaryUserByPosition = userList.stream()
  			.collect(
  				groupingBy(
  					User::getPosition,
  					maxBy(Comparator.comparingInt(User::getMonthlySalary))));
  
  		// then - CEO
  		assertThat(topSalaryUserByPosition.get(Position.CEO).get().getId()).isEqualTo("qoo");
  		print(topSalaryUserByPosition.get(Position.CEO).get());
  
  		// then - MANAGING_DIRECTOR
  		assertThat(topSalaryUserByPosition.get(Position.MANAGING_DIRECTOR).get().getId()).isEqualTo("peter");
  		print(topSalaryUserByPosition.get(Position.MANAGING_DIRECTOR).get());
  
  		// then - MANAGER
  		assertThat(topSalaryUserByPosition.get(Position.MANAGER).get().getId()).isEqualTo("mac");
  		print(topSalaryUserByPosition.get(Position.MANAGER).get());
  
  		// then - STAFF
  		assertThat(topSalaryUserByPosition.get(Position.STAFF).get().getId()).isEqualTo("john");
  		print(topSalaryUserByPosition.get(Position.STAFF).get());
  
  		// 2. when - totalSalaryByPosition
  		Map<Position, Integer> totalSalaryByPosition = userList.stream()
  			.collect(
  				groupingBy(
  					User::getPosition,
  					reducing(0, User::getMonthlySalary, Integer::sum)));
  
  		// then - CEO
  		assertThat(totalSalaryByPosition.get(Position.CEO))
  			.isEqualTo(100000000);
  		print(totalSalaryByPosition.get(Position.CEO));
  
  		// then - MANAGING_DIRECTOR
  		assertThat(totalSalaryByPosition.get(Position.MANAGING_DIRECTOR))
  			.isEqualTo(10000000);
  		print(totalSalaryByPosition.get(Position.MANAGING_DIRECTOR));
  
  		// then - MANAGER
  		assertThat(totalSalaryByPosition.get(Position.MANAGER))
  			.isEqualTo(12000000);
  		print(totalSalaryByPosition.get(Position.MANAGER));
  
  		// then - STAFF
  		assertThat(totalSalaryByPosition.get(Position.STAFF))
  			.isEqualTo(9900000);
  		print(totalSalaryByPosition.get(Position.STAFF));
  }
```
### Console Result
```
User(id=qoo, name=qoo, position=CEO, monthlySalary=100000000)
User(id=peter, name=peter, position=MANAGING_DIRECTOR, monthlySalary=10000000)
User(id=mac, name=mac, position=MANAGER, monthlySalary=7000000)
User(id=john, name=john, position=STAFF, monthlySalary=3000000)
100000000
10000000
12000000
9900000
```

## 5. Partitioning
### Test Code
```java
	@Test
	public void partitioningTest() {
		// when - partitioning executive
		Map<Boolean, List<User>> usersByIsExecutive = userList
			.stream()
			.collect(partitioningBy(User::isExecutive));
		print(usersByIsExecutive);

		// then - executive user
		List<User> executiveUserList = usersByIsExecutive
			.get(true);
		assertThat(executiveUserList).hasSize(2);
		print(executiveUserList);

		// then - non-executive user
		List<User> nonExecutiveUserList = usersByIsExecutive
			.get(false);
		assertThat(nonExecutiveUserList).hasSize(6);
		print(nonExecutiveUserList);
	}

```

### Console Result
```
{false=[User(id=ann, name=ann, position=MANAGER, monthlySalary=5000000), User(id=mac, name=mac, position=MANAGER, monthlySalary=7000000), User(id=john, name=john, position=STAFF, monthlySalary=3000000), User(id=martin, name=martin, position=STAFF, monthlySalary=2500000), User(id=park, name=park, position=STAFF, monthlySalary=2300000), User(id=potter, name=potter, position=STAFF, monthlySalary=2100000)], true=[User(id=qoo, name=qoo, position=CEO, monthlySalary=100000000), User(id=peter, name=peter, position=MANAGING_DIRECTOR, monthlySalary=10000000)]}
[User(id=qoo, name=qoo, position=CEO, monthlySalary=100000000), User(id=peter, name=peter, position=MANAGING_DIRECTOR, monthlySalary=10000000)]
[User(id=ann, name=ann, position=MANAGER, monthlySalary=5000000), User(id=mac, name=mac, position=MANAGER, monthlySalary=7000000), User(id=john, name=john, position=STAFF, monthlySalary=3000000), User(id=martin, name=martin, position=STAFF, monthlySalary=2500000), User(id=park, name=park, position=STAFF, monthlySalary=2300000), User(id=potter, name=potter, position=STAFF, monthlySalary=2100000)]
```