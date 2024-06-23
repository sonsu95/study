## JpaRepository 

기본적인 사용법은 다음과 같다. 

```java
// ItemRepository.java
public interface ItemRepository extends JpaRepository<Item, Long> {

}
```

여기서 `ItemRepository`는 해당 Repository 인터페이스의 이름을 지칭하며, 이 인터페이스는 `JpaRepository` 인터페이스를 확장한다. `JpaRepository`에서 사용되는 제네릭의 첫 번째 인자인 `Item`은 엔티티 타입을 나타내며, 두 번째 인자인 `Long`은 이 엔티티의 기본키 타입을 의미한다.

이 구조는 `Item` 엔티티에 대한 CRUD(Create, Read, Update, Delete) 연산을 간편하게 수행할 수 있도록 설계되었으며, JpaRepository 인터페이스는 다양한 메소드를 제공하여 편리하게 데이터 접근이 가능하도록 도와준다. 

JpaRepository 인터페이스에서 기본적으로 제공하는 메서드들은 컴파일된 JpaRepository.class 파일에서 찾아볼 수 있는데, 이를 살펴보면 다음과 같다. 

```java
// JpaRepository.class
@NoRepositoryBean
public interface JpaRepository<T, ID> extends ListCrudRepository<T, ID>, ListPagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T> {
	void flush();  
  
	<S extends T> S saveAndFlush(S entity);  
	  
	<S extends T> List<S> saveAllAndFlush(Iterable<S> entities);  
	  
	void deleteAllInBatch(Iterable<T> entities);  
	  
	void deleteAllByIdInBatch(Iterable<ID> ids);  
	  
	void deleteAllInBatch();  
	  
	T getReferenceById(ID id);  
	  
	<S extends T> List<S> findAll(Example<S> example);  
	  
	<S extends T> List<S> findAll(Example<S> example, Sort sort);
}

// ListCrudRepository.class
@NoRepositoryBean  
public interface ListCrudRepository<T, ID> extends CrudRepository<T, ID> {  
    <S extends T> List<S> saveAll(Iterable<S> entities);  
  
    List<T> findAll();  
  
    List<T> findAllById(Iterable<ID> ids);
}

// ListPagingAndSortingRepository.class
@NoRepositoryBean  
public interface ListPagingAndSortingRepository<T, ID> extends PagingAndSortingRepository<T, ID> {  
    List<T> findAll(Sort sort);  
}

// QueryByExampleExecutor.class
public interface QueryByExampleExecutor<T> {  
    <S extends T> Optional<S> findOne(Example<S> example);  
  
    <S extends T> Iterable<S> findAll(Example<S> example);  
  
    <S extends T> Iterable<S> findAll(Example<S> example, Sort sort);  
  
    <S extends T> Page<S> findAll(Example<S> example, Pageable pageable);  
  
    <S extends T> long count(Example<S> example);  
  
    <S extends T> boolean exists(Example<S> example);  
  
    <S extends T, R> R findBy(Example<S> example, Function<FluentQuery.FetchableFluentQuery<S>, R> queryFunction);  
}
```

위 코드들을 참고하면 사용 가능한 메서드들을 알 수 있다. 

`@NoRepositoryBean` 
- 스프링 데이터가 레포지토리 인터페이스를 스프링 빈으로 자동 등록하는 것을 방지.
- 레포지토리 인터페이스가 보통 상속 목적으로 사용될 때 중복 등록을 막기 위함.

`QueryByExampleExecutor`가 `@NoRepositoryBean`을 사용하지 않는 이유
- 특정 기능 제공
	- `QueryByExampleExecutor`는 QBE 기능을 제공하는 인터페이스로, 다른 레포지토리 인터페이스에 쿼리 기능을 추가하는 역할
	- 이 인터페이스 자체는 특정 데이터 액세스 기능을 제공하는 것이 목적이기 때문에, 그 자체로는 리포지토리 구현체가 될 필요가 없음.
- 재사용성
	- 일반적으로, `QueryByExampleExecutor`는 다른 리포지토리 인터페이스와 함께 사용되어 그 인터페이스들이 QBE 기능을 이용할 수 있게 한다. 
	- `@NoRepositoryBean`을 붙이지 않음으로써, 실수로 이 인터페이스만 단독으로 빈으로 등록되는 일 방지
- 즉, `QueryByExampleExecutor` 인터페이스가 `@NoRepositoryBean` 어노테이션을 사용하지 않는 것은 이 인터페이스가 주로 기능 추가 목적으로 사용되며, 자체적으로 리포지토리 빈으로 사용될 필요가 없기 때문

QBE(Query by Example) 기능
- 데이터베이스에서 데이터를 조회하는 방법 중 하나로, 사용자가 샘플 객체를 만들어 이 객체가 가진 속성값을 기준으로 데이터를 검색하는 방식