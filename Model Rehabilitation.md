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

# What do model classes contribute in my current system design?
The model classes represent real world entities like in person the national ID number , 


# TCMS Reverse Engineering — Person

Date: 2026-09-16
Project: Capital Commute
Phase: Reverse Engineering
Component: model.Person

## Question

What does Person represent in the original TCMS design?

## Evidence

public abstract class Person - The class being an abstract class means it cannot be instantiated on its own , it has to be instantiated using the child classes
Fields :

--
nationalId,
fullName,
password,
phoneNumber;
email,
registrationDate,
isActive.
**Constructor** takes this values nationalId  ,fullName,phoneNumber,email ,password.
and internally:
registrationDate = LocalDateTime.now()
isActive = true

**Getters**
// Ndio hizi getters zangu hapa hivi zina shine//
    public String getNationalId() {
        return nationalId;
    }// This one is set at registration and never changes//

    public LocalDateTime getRegistrationDate() {
        return registrationDate;
    }


//Sasa mwelcome hawa wako na setters and getters chonjo//
    public String getFullName() {
        return fullName;
    }

    public void setFullName(String fullName) {
        this.fullName = fullName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;

    }
    public boolean isActive() {
        return isActive;
    }

    public void setActive(boolean active) {
        isActive = active;
    }
}


## What I discovered
**Inheritance**


--

Person is a parent class for all user classes that includes  both client and driver.

**Setters and Getters**

--
Getters and setters serve different purposes  in a well designed class.
Getter only like the national Id , Registration date - are methods you can only see but you cannot change it. - in the getter situation other classes for example can only ask to get the time but they cannot ask to set the time.
It has proven to be useful when the value should remain fixed after creation.

## What is still unknown

Is person responsible for authentication by merely carrying the presence of a password field ? especially in the first generation the second generation of the project the responsibility may be handed over to Auth.Service.

## Architectural observation

**Inheritance**


--

Person is a parent class for all user classes that includes  both client and driver.

            |Person|
           /   |    \
       Client Driver Mechanic

Person appears to combine two categories of information:

Identity

national ID
name
phone
email
registration date

Account/state

password
active/inactive statusPerson appears to combine two categories of information:

             

## Git commit

Not yet





