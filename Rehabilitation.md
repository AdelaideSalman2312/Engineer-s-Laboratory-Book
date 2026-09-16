GUI
 ↓
Services
 ↓
DAO
 ↓
Database
 ↓
Model



| Class | What does it represent? | What other things does it know about? |
| :--- | :--- | :--- |
| Person | This is a class that holds all the details of the users the common details .  |the common details like the name ,national id , email ,full password, registration date . |
| Client | Represents a customer requesting transport or hiring vehicles. Inherits core attributes from `Person`. | Payment methods, ride/booking history, preferred pickup/dropoff locations, rating. |
| Driver | Represents an authorized vehicle operator. Inherits core attributes from `Person`. | Driver's license number, assigned `Vehicle`, current availability status, overall rating, assigned `Booking`s. |
| Vehicle | Represents a physical transport unit managed by the system. | License plate, make, model, capacity, current operational status, `Maintenance` logs. |
| Booking | Tracks a single service transaction linking clients, drivers, and vehicles. | Associated `Client`, assigned `Driver`, assigned `Vehicle`, pickup/destination locations, fare, timestamp, status. |
| Maintenance | Tracks repair and routine inspection logs for a vehicle. | Associated `Vehicle`, assigned `Mechanic`, assigned `Driver`, repair date, service description, cost, parts replaced. |
| Mechanic | Represents technical staff performing vehicle maintenance. Inherits core attributes from `Person`. | Specialization,  assigned `Maintenance` tasks, service history records. |





