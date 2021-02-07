# Cache

**:Contents**
* [ehcache란] 
* [캐시 TTL 설정할때 기준이 뭘까요]


### ehcache란
>> ehcache는 Spring에서 간단하게 사용할 수 있는 Java기반 오픈소스 캐시 라이브러리이다.

* redis나 memcached같은 캐시 엔진들도 있지만, 저 2개의 캐시 엔진과는 달리 ehcache는 데몬을 가지지 않고 Spring 내부적으로 동작하여 캐싱 처리를 한다.
* 3 버전 부터는 javax.cache API (JSR-107)와의 호환성을 제공한다. 따라서 표준을 기반으로 만들어졌다고 볼 수 있다.
* 기존 2.x 버전과는 달리 3 버전에서는 offheap 이라는 저장 공간을 제공한다. 
* offheap이란 말 그대로 힙 메모리를 벗어난 메모리로 Java GC에 의해 데이터가 정리되지 않는 공간이다.



`참고사항`<br>
* ehcache3 는 캐싱할 데이터를 외부 메모리(offheap 혹은 disk)에 저장하기 위해서는 저장할 데이터(객체 혹은 인스턴스)가 Serializable이 구현 되어 있어야 한다.
* 즉, 캐싱할 데이터는 Serializable을 상속받은 클래스여야 한다.
* 왜냐하면, ehcache가 JVM의 힙 메모리가 아닌 곳(offheap 혹은 disk)에 캐시를 저장하기 위해서는
* JVM 메모리에 인스턴스화 되어있는 객체의 데이터를 외부에서 사용할 수 있게 하기 위해 Serialize(직렬화)가 필요하기 때문이다.

`설정`<br>
전체 코드는 아래의 링크를 참고하자<br>
https://github.com/whdms705/conference/commit/0526135fa7f96621f9ab227acd851c59b91405d3<br>

```java

@Mapper
@Repository	@Repository
public interface ConferenceMapper {

    @Cacheable("selectAllConference")
    List<Conference> selectAllConference();
    	
}

```

```java

@EnableCaching
@org.springframework.context.annotation.Configuration
public class MeetingEhcacheConfig {
    @Bean(destroyMethod = "shutdown")
    public CacheManager makeEhcacheManager(){
        Configuration config = new Configuration();
        config.setName("__meeting");

        config.addCache(getCacheConfiguration("selectAllConference",60*10));
        return CacheManager.newInstance(config);
    }

    private CacheConfiguration getCacheConfiguration(String name , long second){
        return getCacheConfiguration(name ,second , 100L);
    }

    private CacheConfiguration getCacheConfiguration(String name , long second , long size){
        CacheConfiguration cacheConfiguration = new CacheConfiguration();
        cacheConfiguration.setName(name);
        cacheConfiguration.setMemoryStoreEvictionPolicy("LRU");
        cacheConfiguration.setMaxEntriesLocalHeap(size);
        cacheConfiguration.setTimeToLiveSeconds(second);
        cacheConfiguration.setTimeToIdleSeconds(0L);

        return cacheConfiguration;
    }

}

```

### 캐시 TTL 설정할때 기준이 뭘까요
https://community.akamai.com/customers/s/article/TTL?language=en_US

