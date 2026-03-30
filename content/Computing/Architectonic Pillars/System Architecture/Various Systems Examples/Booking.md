Movie Ticket Booking System
Requirements (compact):
Theaters → screens → shows.
Seat layout per screen.
Seat statuses: Available / Held / Booked.
	• Hold seats for a few minutes before payment.


Fucntional
1. Mutiple screen
2. Screen - specific time 
3. User
    - seat layoud
    - hold seat temp(5 min)
    - confirm Booking
4. Seat statuses
    AVAILABLE
    Held
    BOOKED


Entities 
Theate
Screen
Seat
Show 
SeatInventory 
Booking 

@Entity
@Table(name="threatre")
@Getter
@NoArgsConstructor 
public class Theatre {

    @id
    @GeneratedValue(strategy=GeneratationType.IDENTITy)
    private Long id ;

    @Column(nullable=false)
    private String name;

    @Column(nullable=false)
    private String location;

    @OneToMany(mappedBy="threatre", cascade= CascadeType.ALL)
    private List<Screen> = new ArrayList<>();

    public Theate(String name, String location){
        this.name = name;
        this.location = location;
    }
}

@Entity
@Table(name="screens")
@Getter
@NoArgsConstructor 
public class Screens{
    
    @id
    @GeneratedValue(strategy=GeneratationType.IDENTITy)
    private Long id ;

    @ManyToOne()
    @JoinColumn(
        name="threatre_id",
        nullable = false,
        foreignKey = @ForeignKey(name="fk_screen_threatre)
    )
    private Theatre threatre
    private String screenName;

    private List<Seat> seats = new ArrayList<>();
}


@Entity
@Table(name="seats")
@Getter
@NoArgsConstructor 
public class Seats{
    
    @id
    @GeneratedValue(strategy=GeneratationType.IDENTITy)
    private Long id ;

    private Screen screen;

    private String seatNumber;

}


@Entity
@Table(name="show")
@Getter
@NoArgsConstructor 
public class Show{

    @id
    @GeneratedValue(strategy=GeneratationType.IDENTITy)
    private Long id ;

    private Screen screen;

    private String movieName;

    private LocalDateTime showTime


}

@Entity
@Table(name="seat_inventory")
@Getter
@NoArgsConstructor 
public class SeatInventory{
    @id
    @GeneratedValue(strategy=GeneratationType.IDENTITy)
    private Long id ;

    private Show show;

    private Seat seat;

    private SeatStatus status;

    private Long heldByUser;

    private Instant holdExpiry;


    @version
    private Long version;


    //hold
    private void hold(Long heldByUser, Instant holdExpiry){
        this.heldByUser = heldByUser;
        this.holdExpiry = holdExpiry;
    }
    //book
    private book(Long Id )
    //release 

}

@Entity
@Table(name="booking")
@Getter
@NoArgsConstructor 
public class Booking{

    @id
    @GeneratedValue(strategy=GeneratationType.IDENTITy)
    private Long id ;

    private Long userId;

    private Show show;

    private BookingStatus status;

    //Constructor initialization


    //confirm
    //cancel 

}


@Service 
@RequiredArgsConstructor 
public class SeatService {

    private final SeatInventoryRepository seatInventoryRepository;

    @Transactional
    pubic void holdSeat(Long showId, List<Long> seatIds, Long userId){

        List<SeatInventory> seats = seatInventoryRepository.findByShowAndSeatId(showId, seatIds)

        for(SeatInventory seat: seats){
            if(seat.getStatus() != SeatStatus.AVAILABLE){
                thow new IllegalArgumentException()
            }

            //hold till payment is completed 
            //update - complete 
            seat.hold(userId, Duration.ofMinutes(5));
        }
    }

    public void bookSeat(BookingId bookingId){
    Booking booking = 
       Payment payment =  paymentService.getBookingDetails(bookingId)
           if(payment.geStatus() = PAYMENT.PAID){
             seat.book(userId);
           }
        }
    }


{
    "ShowId" : "ABC",
    "SeatIds" : [882, 883, 884],
    "UserId": "User1232",
    "BookingTime" : "13:30"
}
@RestController
@RequestMapping("/api/bookings")

public class BookingController{

   private final SeatService seatService;

   @PostMapping
   public ResponseEntity<BookingResponse> createBooking(
    @RequestBody BookingRequest request
   )

   BookingResponse response = seatService.holdSeat(request.getShowId(), request.getSeatIds(), request.getUserId(), request.getBookingTime());


}

//Interface 



//DTO
public class BookingRequest{

    private Long showId;

    private Long userId;

    private List<Long> seatIds;

    private LocalDateTime bookingTime 
}

public class BookingResponse{

    private Long bookingId;
    private Long showId;
    private Long userId;
    private List<Long> seatIds;
    private Status status;
    private LocalDateTime bookedAt;
    private Double totalAmount;
}


ACID
1. Atomiticity
2. Consist

version = 1


SARPSO-E
Scalability
Availability
Reliability - CAP (Consistency[eventual vs immediate], Availability and Performance)
Performance - latency & throughput 
Security
Observability 
Extensibility 




Functional Requirement 

Non Functional
1. Scalability - million
2. Availability - highly Availability 
3. Reliability - Strongly consistent - no double booking ; idempotency - payment retries 
4. Performance - less than <100ms 
5. Security
6. Observability
7. Extensible 


Core Entities 
1. Show
2. Seat
3. SeatInventory 
4. Reservation 

Enum 
public enum SeatStatus{
    AVAILABLE,
    LOCKED,
    BOOKED
}


@Entity 
@Table
public class Show{
    @Id
    @GeneratedValue(stategy=GenerationType.IDENTITY)
    private Long id;

    private String movieNamel;
    private LocalDateTime showTime;
}

@Entity
public class Seat{
    
    @Id
    @GeneratedValue(stategy=GenerationType.IDENTITY)
    private Long id;

    private String seatNumber;

    @ManyToOne
    private Show show;
}

