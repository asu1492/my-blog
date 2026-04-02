FeedService.txt

Requirement 
1. Functional
    User - see personalized feed 
    Feed - ordered as per timestamp/ranking
    Pagination Support
    Update - when new post arrives
2. Non Functional 
    Scalability - million
    Availability - eventual consistency
    Reliability
    Performance - low latency - <100ms 
    Security 
    Observability
    Extensibility


Core Entities / Domain Models 
Post -> Post created by User 
FeedEntry -> precomputed feed entries per user 

@Entity 
@Table(name="post_entries",
        indexes = {
         @Index (name ="idx_post_created", columnList = "authorId, createdAt")
       }
)
@Getter
@NoArgsConstructor 
public class Post{
   
    @Id
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    private Long id;

    @Column(nullable=false)
    private Long authorId;

    @Column(nullable=false, length=1000)
    private String content;

    @Column(nullable=false)
    private Instant createdAt;

    @Version 
    private Long version;

    public Post(Long authorId, String content, Instant createdAt){
        this.authorId = authorId;
        this.content = content;
        this.createdAt =createdAt;
    }
}

@Entity
@Table(name=feed_entries,
       indexes = {
         @Index (name-="idx_user_created", columList = "userId, createdAt")
       },
       uniqueConstraints={
        @uniqueConstraints(columnName={"userId","postId"})
       }
)
@Getter
@NoArgsConstructor
public class FeedEntry {

    @Id
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    private Long id;

    private Long userId;
    private Long postId;
    private Instant createdAt;

    public FeedEntry(Long userId, Long postId, Instant createdAt){
        this.userId = userId;
        this.postId = postId;
        this.createdAt = createdAt;
    }
}

@Entity
@Table(name = "followers",
      indexes = {
        @Index(name="idx_followers", columList="followerId")
      })
@Getter
@NoArgsConstructor
public class Follower {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private Long userId;       // follower
    private Long followeeId;   // person being followed
}


Controller -> Service -> Respository 
strategy pattern for feed Generation
factory pattern for select strategy
state machine for post lifecycle 
kafa for fan-out 
retry and DLQ for failure handling 
optimistic locking 
caching for hot feeds 

Controller
@RestController
@RequestMapping("/api/feed")
public class FeedController {

    private final FeedSerice feedService;

    public FeedController(FeedSerice feedService){
        this.feedService = feedService;
    }

    @GetMapping("/{userId}")
    public List<FeedEntry> getFeed(@PathVariable Long userId, @RequestParam int page, @RequestParam int size){
        return feedService.getFeed(userId,page,size);
    }
}

@Service 
public class FeedSerice{

    private final FeedRepository feedrepository;
    private final FeedStrategyFactory feedStrategyFactory;
    private final ChacheService cacheService;

    public FeedSerice(FeedRepository feedrepository, FeedStrategyFactory feedStrategyFactory, ChacheService cacheService){
        this.feedrepository = feedrepository;
        this.feedStrategyFactory = feedStrategyFactory;
        this.cacheService = cacheService;
    }

    public Page<FeedEntry> getFeed(Long userId, int Page, int size){

        Pageable pageable = PageRequest.of(page,size);

        Page<FeedEntry> cached = chacheService.get(userId,page);
        if(cached !=null) return cached;

        Page<FeedEntry> feed = feedrepository.findByUserIDOrderByCreatedAtDesc(userId,pageable);

        cacheService.put(userId, page, feed);

        return feed;
    }
}

public interface FeedRepository extends JpaRepository<FeedEntry, Long>{

    Page<FeedbEntry> findByUserIDOrderByCreatedAtDesc(
        Long userId, Pageable pageable
    );
}

pubic interface FollowerRepository extends JpaRepository<FeedEntry, Long>{
    @Query(select f.userId from Follower f where f.followeeId = followeeId)
}


@Service 
public Class CacheService{
    private final RedisTemplate<String, Object> redisTemplate;

    private String buildKey(Long userId, int page){
        return "feed:" + userId + ":" + page
    }

    //get
    //put
    //evict 
}

strategy pattern 
1. Fan Out Write 
2. Fan Out Read 

public interface FeedGenerationStategy{
    void generateFeed(Post post);
}

@Component
@Builder
@AllArgsConstructor 
public class FanoutWriteStrategy implments FeedGenerationStategy{

    private final FeedRepository feedrepository;
    private final FollowerRepository followerRepository;
    private final CacheService cacheService;

    @Override 
    public void generateFeed(Post post){
        List<Long> followers = followerRepositor.findFollowers(post.getAuthorId);

        for(Long followerId: followers){
            FeedEntry entry = FeedEntry.builder()
                                .userId(followerId)
                                .postId(post.getId())
                                .createdAt(post.getCreatedAt()).build()

            feedrepository.save(entry);
            cacheService.evict(followerId);
        }
    }
}

@Component
@AllArgsConstructor
public class FeedStrategyFactory {
    private final FanoutWriteStrategy fanoutWriteStrategy;

    public FeedGenerationStategy getStrategy(){
        return fanoutWriteStrategy;
    }
}

@Component
public class PostEventProducer {
    private final KafkaTemplate<String, PostEvent> kafkaTemplate;

    public void publish(Post post){
        kafkaTemplate.send("post-created-topic", new PostEvent(post.getId(), post.getAuthorId()))
    }
}

@KafkaListener(topic=""post-created-topic")
public void consume(PostEvent event){

}