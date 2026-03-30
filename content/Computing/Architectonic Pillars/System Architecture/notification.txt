
Step by Step
1. Define Requrirement 
2. Explain Request Flow 
3. Architectural Layer 
4. LLD 


I will design this using layered architecture with controller as entry point, service as orchestration layer, and strategy pattern for channel-specific delivery.

End to End Flow
1. Client sends notification request via REST API
2. Controller receives request and converts to DTO 
3. Service validates authorization 
4. TargetResolver converts logical target -> userList 
5. NotificationSender dispatches messages 
6. Strategy pattern handle channel specific delivery 


Code Correct Order
1. Enums (type safety)
2. Models (domain objects)
3. DTO (API contract)
4. Controller (entry point)
5. Service (Orchestration)
6. Supporting Services 
7. Strategy pattern
8. Factory 
9. Resolver 

Client -> Controller -> Notification Service -> AuthorizationService  -> NotificationChannel(Interface) 
                                                TargetResolver
                                                NotificationSender 

Enum
1. enforce type safety 
2. avoid magic string 
3. prevent invalid input and compile time validation 

public enum Role{
    STUDENT,
    TEACHER,
    STAFF,
    ADMIN
}
public enum ChannelType{
    SMS, 
    EMAIL,
    PUSH
}

public enum NotificationStatus{
    PENDING, 
    SENT,
    FAILED,
    CANCELLED
}

Model 
1. Model Layer represents core business / domain entities 
2. Simple POJO without Business logic 
3. lombok to reduce boilerplate


@Data
@NoArgsConstructor 
@AllArgsConstructor 
public class User{
    private String userId;
    private String name;
    private String email;
    private String phoneNumber;
    private String deviceToken;
    private Role role,
    private String className;
    private String section;
    private String groupName;
}

@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Notification{
    private String notificationId;
    private String title;
    private String message;
    private Role createdByRole;
    private String targetType;
    private String targetValue;
    private List<ChannelType> channels; //one notification via multiple channels 
    private LocalDateTime createdAt;
    private LocalDateTime scheduledAt;
    private NotificationStatus status;
}

DTO Layer
/notification/send
{
    "senderRole" : "TEACHER",
    "targetType": "CLASS",
    "targetValue" : "XA",
    "title" : "Nexus Author, Yuval Noah Hariri visit"
    "message": "",
    "channels": ["EMAIL", "SMS"]
}

1. this is what client sends
2. decouples API contract from domain model 
3. Why ? 
    Prevent Exposing internal structure, 
    validation layer, 
    versioning flexibility 
2. We use lombok lib to reduce boilerplate code 
3. @Data generates - getters, setter, toString, equalsAndHashCode, requiredArgsConstructor

@Data
public class NotificationRequest {
    private String targetType;
    private String targetValue;
    private String title;
    private String message;
    private List<String> channels;
}


Controller 
1. Entry point of the system
2. receives HTTP request
3. Maps it to DTO
4. Delegate to service layer

Constructor injection, why?
    - immutability 
    - cannot create object without dependency - compile time safety 
    - unit testing easier
    - fail fast and hence helps in detecting circular dependency earlier. 
Constructor injection ensures immutability, testability, and compile-time safety, while field injection hides dependencies and allows partially constructed objects.

@RestController
@RequestMapping("/api/notification)
public class NotificationController {

    private final notificationService;

    public NotificationController(NotificationService notificationService){
        this.notificationService = notificationService;
    }

    @PostMapping("/send")
    public String sendNotification(@RequestBody NotificationRequest request){

        Notification notification = Notification.builder()
                                      .title(request.getTitle())
                                      .message(request.getMesssage())
                                      .build();

        notificationService.sendNotification(request.getSenderRole(),
                                             request.getTargetType() 
                                             request.getTargetValue()
                                             notification,
                                             request.getChannel());

        return "Notification sent successfully";
    }
}



Service Layer / Orchestration Layer 
1. Orchestrates Business logic 
2. Coordinates across multiple Components

@Service 
public class NotificationService {
    private final TargetResolver targetResolver;
    private final AuthorizationService authorizationService;
    private final NotificationSender sender;

    public NotificationService(TargetResolver resolver, AuthorizationService authorizationService,NotificationSender sender){
        this.resolver = resolver;
        this.authorizationService = authorizationService;
        this.sender = sender;
    }

    public void sendNotification(Role senderRole, String targetType, String targetValue, Notification notification, List<ChannelType> channels){
       
       //Ensures role-based access control.
        if(!authorizationService.canSend(senderRole, targetType)){
            throw new RunTimeException("Unauthorized");
        }

        //Converts logical group into concrete recipients.
        List<User> users = resolver.resolveTargets(targetType, targetValue);
        
        //Delegates delivery to channel strategies.
        sender.send(notification,users,channel);
    }
}

@Service
public class AuthorizationService{
    public boolean canSend(Role senderRole, String targetType){
        if(senderRole == Role.ADMIN) return true;

        if(senderRole == Role.TEACHER && targetType.equals("CLASS")) return true;
         
        return false;
    }
}

//NotificationSender is responsible only for dispatching messages. It is channel-agnostic.
@Service
public class NotificationSender{

    public void send(Notification notification, List<User> users, List<ChannelType> channels){
         
         // Outer loop iterates channels, inner loop iterates users.  
        for(ChannelType type: channels){
            NotificationChannel channel = NotificationChannelFactory.getChannel(type);

            for(User user: users){
                channel.send(user, notification);
            }
        }
    }
}

Strategy Interface
1. Strategy Pattern to support multiple notification channels without changing core logic
2. Why? 
    open for extension
    avoids if-else
    pluggable delivery mechanism

public interface NotificationChannel{
    void send(User user, Notification notification)
}

//Each strategy encapsulates provider-specific delivery logic.
1. Email → SMTP / SendGrid
2. SMS → Twilio
3. Push → Firebase

Email Strategy 
public class EmailNotificationChannel implments NotificationChannel{

    @Override
    public void send(User user, Notification notification){
        user.getEmail()
    }
}
SMS Strategy
public class SMSNotificationChannel implments NotificationChannel{

    @Override
    public void send(User user, Notification notification){
        user.getPhone()
    }
}

PUSH Strategy
public class PushNotificationChannel implments NotificationChannel{

    @Override
    public void send(User user, Notification notification){
        user.getDeviceToken()
    }
}

Channel Factory 
1. Factory resolves correct strategy based on channel type.
2. Why ?
    centralized creation
    dependency injection friendly
    runtime resolution

@Component
public class NotificationChannelFactory {
    private static Map<ChannelType, NotificationChannel> channelMap = new HashMap<>();

    public NotificationChannelFactory(
        EmailNotificationChannel email,
        SMSNotificationChannel sms,
        PushNotificationChannel push
    ){
        channelMap.put(ChannelType.EMAIL, email);
        channelMap.put(ChannelType.SMS, sms);
        channelMap.put(ChannelType.PUSH, push);
    }

    public static NotificationChannel getChannel(ChannelType type){
        return channelMap.get(type);
    }
}

TargetResolver
1. abstracts user lookup logic. It isolates database queries from business flow.
2. Why ?
    caching possible
    distributed lookup
    different resolver implementations

public interface TargetResolver{
    List<User> resolveTargets(String targetType, String targetValue);
}
@Component
public class DummyTargetResolver implments TargetResolver{
    @Override
    public List<User> resolveTargets(String targetType, String targetValue){
        List<User> users = new ArrayList<>();
        users.add(new Users("1", "xyz@gmail.com", "23489023", "token1", Role.STUDENT))
         users.add(new Users("2", "x1z@gmail.com", "2333489023", "token2", Role.STUDENT))
        
        return users;
    }
}



This design follows 
    layered architecture, 
    strategy pattern for channel extensibility, and
     clear separation of concerns between 
        API, 
        orchestration, and 
        delivery.



Production Enhancements
    asynchronous processing via Kafka
    retry mechanism
    delivery tracking table
    scheduling service
    rate limiting
    monitoring



Controller -> Notification Service -> Repository(JPA) | Message Publisher -> Queue -> Notification Worker -> NotificationSender -> External Providers -> Update Delivery Status

Lifecycle with persistent and message queues
1. Controller receives request
2. Service validates authorization
3. Service saves notification in DB
4. Service publishes event to Kafka
5. HTTP response returned immediately
6. Worker consumes message
7. Sender delivers notification
8. Delivery status updated in DB

Persistence layer is introduced inside service to store notifications and track status.
Message queue is introduced after service to enable asynchronous delivery and horizontal scaling.


Notifications are stored before publishing event to Kafka to guarantee persistence.
Delivery is tracked per user per channel.
Failed deliveries are retried and eventually routed to Dead Letter Queue.

JPA Entities 
1. Notification (one message)
2. Delivery (per user per channel)
3. User (recipient)

UserEntity
@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class UserEntity{

    @Id
    private String userId;

    private String name;
    private String email;
    private String phoneNumber;
    private String deviceToken;
    
    @Enumerated(EnumType.STRING)
    private Role role,
    
    private String className;
    private String section;
    private String groupName;
}

@Entity
@Table(name="notifications")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class NotificationEntity{
   
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long notificationId;

    private String title;
    private String message;

    @Enumerated(EnumType.STRING)
    private Role createdByRole;

    private String targetType;
    private String targetValue;
    
    private List<ChannelType> channels; //one notification via multiple channels 
    private LocalDateTime createdAt;
    private LocalDateTime scheduledAt;
    
    @Enumerated(EnumType.STRING)
    private NotificationStatus status;


    //One Notification → many Delivery records;
    //CascadeType.ALL means Save notification → deliveries auto saved; Delete notification → deliveries auto deleted
   //OneToMany defines parent-child relationship. mappedBy indicates the owning side is defined in DeliveryEntity. Cascade ensures child records are persisted and deleted automatically with parent.
    
    @OneToMany(mappedBy = "notification", cascade = CascadeType.ALL)
    private List<DeliveryEntity> deliveries;
}

@Entity
@Table(name="notification_delivery)
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class DeliveryEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    
    //ManyToOne defines many deliveries referencing a single notification. JoinColumn specifies the foreign key column in delivery table.
    @ManyToOne
    @JoinColumn(name="notification_id")
    private NotificationEntity notification;

    private String userId;

    @Enumerated(EnumType.STRING)
    private ChannelType channel;

    @Enumerated(EnumType.STRING)
    private NotificationStatus status;

    private int retryCount;

    private LocalDateTime sentAt;
}

@Repository
public interface NotificationRepository extends JpaRepository<NotificationEntity, Long> {
    //new -> persist 
    else -> merge 
}

@Repository
public interface DeliveryRepository extends JpaRepository<DeliveryEntity, Long> {}


Kafka Event Model
@Data
@AllArgsConstructor
@NoArgsConstructor
public class NotificationEvent {
    private Long notificationId;
    private List<String> userids;
    private List<ChannelType> channels;
}

Kafka Producers 
@Service
@requiredArgsConstructor
public class NotificationEventProducer{
    private final KafkaTemplate<String, NotificationEvent> kafkaTemplate;
    private static final String TOPIC = "notification-topic";

    public void publish(NotificationEvent event){
        kafkaTemplate.send(TOPIC, event);
    }
}

Service Layer (Save + Publish)
@Service
@RequiredArgsConstructor
public class NotificationService {

    private final NotificationRepository notificationRepository;
    private final NotificationEventProducer producer;
    private final TargetResolver resolver;

    public void sendNotification(Notification notification,
                                 String targetType,
                                 String targetValue,
                                 List<ChannelType> channels) {

        List<User> users = resolver.resolveTargets(targetType, targetValue);

        NotificationEntity entity = mapToEntity(notification);
        entity = notificationRepository.save(entity);

        NotificationEvent event = new NotificationEvent(
                entity.getId(),
                users.stream().map(User::getUserId).toList(),
                channels
        );

        producer.publish(event);
    }
}

Kafka Consumer 
@Service
@RequiredArgsConstructor
puiblic class NotificationEventConsumer{
    private final NotificationSender sender;
    private final DeliveryRepository deliveryRepository;
    private final UserRepository userRepository;

    @KafkaListener(topic="notification-topic)
    public void consume(NotificationEvent event){
        for(String userId: event.getUserIds()){
            UserEntity user = userRepository.findById(userId).orElseThrow();
        }

        for(ChannelType channel: event.getChannels()){
            DeliveryEntity delivery = DeliveryEntity.builder()
                                        .userId(userId)
                                        .channel(channel)
                                        .status(NotificationStatus.PENDING)
                                        .build()
           
            deliveryRepository.save(delivery);

            sender.send(user, channel, delivery);
        }
    }
}

Notification Sender with retries 
@Service 
@RequiredArgsConstructor
public class NotificationSender{
    private final DeliveryRepository deliveryRepository;

    public void send(UserEntity user, ChannelType channel, DeliveryEntity delivery){
        try{
               NotificationChannel strategy = NotificationChannelFactory.getChannel(channel);
               strategy.send(user);
               delivery.setStatus(NotificationStatus.SENT);
               delivery.sentAt(LocalDateTime.now());
        }
        catch(Exception e){
            delivery.setRetryCount(delivery.getRetryCount() + 1);
            if(delivery.getRetryCount >= 3){
                delivery.setStatus(NotificationStatus.FAILED);
                throw e;
            }
        }
        deliveryRepository.save(delivery);
    }
}



Notes

NoArgsConstructor and AllArgsConstructor 
1. NoArgsConstructor is required for frameworks like Spring, Jackson, and JPA that instantiate objects via reflection. 
2. AllArgsConstructor provides convenient object creation with all fields initialized.
3. Using both supports both framework instantiation and manual construction.

Why not user @Data for JPA Entities 
@Data is avoided in JPA entities because it generates equals, hashCode and toString using all fields, which can cause recursion, performance issues, and unintended lazy loading.
1. equals() and hashCode() issue
2. Entities often have relationships (@OneToMany, @ManyToOne)
3. Using all fields causes:
        Infinite recursion
        StackOverflowError
        Performance problems
4. Example:
    NotificationEntity → has deliveries
    DeliveryEntity → has notification
    This creates circular reference in equals/hashCode.

@Enumerated(EnumType.STRING)
EnumType.STRING ensures enum values are stored as strings in the database instead of ordinals, preventing data corruption if enum order changes.
1. Without this annotation, JPA stores Enum as Ordinal(0,1,2) -> if Enum order changes, DB becomes corrupted. 
2. With this annoation, it is stored as id="xaw3r343" 