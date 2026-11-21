# Gym Management System: System Diagrams

**Course:** CS (CF313)

نظام لإدارة الجيم: الأعضاء، الاشتراكات، الدفع، الحصص، خطط التمرين، الحضور، المعدات، والتقارير.

**المحتوى:**
1. Context Diagram
2. DFD Level 1
3. Use Case Diagram
4. Class Diagram

---

## 1) Context Diagram

```mermaid
flowchart LR
    M["Member"]
    T["Trainer"]
    R["Receptionist"]
    A["Admin"]
    PG["Payment Gateway"]
    S(("Gym Management\nSystem"))

    M -->|"Registration data, booking requests, payment details"| S
    S -->|"Membership status, class schedule, workout plan, receipts"| M

    T -->|"Workout plans, class schedule, assignments"| S
    S -->|"Assigned members list, class attendees"| T

    R -->|"New member data, check-in, payment records"| S
    S -->|"Member info, membership status, receipts"| R

    A -->|"Staff data, plans, equipment data, report requests"| S
    S -->|"Reports, equipment status"| A

    S -->|"Payment request"| PG
    PG -->|"Payment confirmation"| S
```

---

## 2) DFD Level 1

- الدواير = Processes
- الأشكال الأسطوانية = Data Stores

```mermaid
flowchart LR
    M["Member"]
    T["Trainer"]
    R["Receptionist"]
    A["Admin"]
    PG["Payment Gateway"]

    P1(("1.0\nManage Members"))
    P2(("2.0\nManage Memberships\nand Payments"))
    P3(("3.0\nManage Classes\nand Bookings"))
    P4(("4.0\nManage Workout\nPlans"))
    P5(("5.0\nTrack Attendance"))
    P6(("6.0\nManage Equipment"))
    P7(("7.0\nGenerate Reports"))

    D1[("D1 Members")]
    D2[("D2 Memberships")]
    D3[("D3 Payments")]
    D4[("D4 Classes and Bookings")]
    D5[("D5 Workout Plans")]
    D6[("D6 Attendance")]
    D7[("D7 Equipment")]

    R -->|"New member data"| P1
    M -->|"Profile data"| P1
    P1 -->|"Member record"| D1
    P1 -->|"Registration confirmation"| M

    M -->|"Renewal request, payment details"| P2
    R -->|"Payment record"| P2
    P2 <-->|"Payment request / confirmation"| PG
    P2 -->|"Membership data"| D2
    P2 -->|"Payment data"| D3
    D2 -->|"Membership status"| P2
    P2 -->|"Receipt, membership status"| M

    M -->|"Booking / cancel request"| P3
    T -->|"Class schedule"| P3
    D2 -->|"Active membership check"| P3
    P3 <-->|"Class and booking data"| D4
    P3 -->|"Booking confirmation"| M

    T -->|"Workout plan, assignment"| P4
    P4 -->|"Plan data"| D5
    D5 -->|"Plan details"| P4
    P4 -->|"Workout plan"| M
    P4 -->|"Assigned members"| T

    R -->|"Check-in"| P5
    D2 -->|"Membership validity"| P5
    P5 -->|"Attendance record"| D6
    P5 -->|"Check-in result"| R

    A -->|"Equipment data"| P6
    P6 <-->|"Equipment records"| D7
    P6 -->|"Equipment status"| A

    A -->|"Report request"| P7
    D1 -->|"Member data"| P7
    D3 -->|"Payment data"| P7
    D6 -->|"Attendance data"| P7
    D7 -->|"Equipment data"| P7
    P7 -->|"Reports"| A
```

---

## 3) Use Case Diagram

```mermaid
flowchart LR
    Member(["Member"])
    Trainer(["Trainer"])
    Receptionist(["Receptionist"])
    Admin(["Admin"])

    subgraph SYS["Gym Management System"]
        UC1(["Login"])
        UC2(["View Membership"])
        UC3(["Renew Membership"])
        UC4(["Pay Fees"])
        UC5(["Book Class"])
        UC6(["Cancel Booking"])
        UC7(["View Workout Plan"])
        UC8(["View Attendance History"])
        UC9(["Create Workout Plan"])
        UC10(["Assign Plan to Member"])
        UC11(["Manage Class Schedule"])
        UC12(["Register New Member"])
        UC13(["Record Payment"])
        UC14(["Check-in Member"])
        UC15(["Manage Staff"])
        UC16(["Manage Membership Plans"])
        UC17(["Manage Equipment"])
        UC18(["Generate Reports"])
        UC19(["Verify Active Membership"])
    end

    Member --- UC1
    Member --- UC2
    Member --- UC3
    Member --- UC4
    Member --- UC5
    Member --- UC6
    Member --- UC7
    Member --- UC8

    Trainer --- UC1
    Trainer --- UC9
    Trainer --- UC10
    Trainer --- UC11

    Receptionist --- UC1
    Receptionist --- UC12
    Receptionist --- UC13
    Receptionist --- UC14
    Receptionist --- UC3

    Admin --- UC1
    Admin --- UC15
    Admin --- UC16
    Admin --- UC17
    Admin --- UC18

    UC5 -.->|"include"| UC19
    UC14 -.->|"include"| UC19
    UC3 -.->|"include"| UC4
    UC10 -.->|"extend"| UC9
```

---

## 4) Class Diagram

```mermaid
classDiagram
    class User {
        -int id
        -String name
        -String email
        -String phone
        -String password
        +login()
        +logout()
    }
    class Member {
        -Date joinDate
        -float weight
        -float height
        +bookClass()
        +cancelBooking()
        +viewWorkoutPlan()
    }
    class Trainer {
        -String specialty
        +createWorkoutPlan()
        +assignPlan()
    }
    class Receptionist {
        +registerMember()
        +checkInMember()
        +recordPayment()
    }
    class Admin {
        +manageStaff()
        +manageEquipment()
        +generateReport()
    }
    class MembershipPlan {
        -int planId
        -String name
        -int durationMonths
        -float price
    }
    class Membership {
        -int membershipId
        -Date startDate
        -Date endDate
        -String status
        +renew()
        +isActive()
    }
    class Payment {
        -int paymentId
        -float amount
        -Date date
        -String method
        +processPayment()
    }
    class GymClass {
        -int classId
        -String name
        -DateTime schedule
        -int capacity
        +addBooking()
    }
    class Booking {
        -int bookingId
        -Date bookingDate
        -String status
        +cancel()
    }
    class WorkoutPlan {
        -int planId
        -String goal
        -String exercises
        -int durationWeeks
    }
    class Attendance {
        -int id
        -DateTime checkIn
        -DateTime checkOut
    }
    class Equipment {
        -int equipmentId
        -String name
        -String condition
        +scheduleMaintenance()
    }

    User <|-- Member
    User <|-- Trainer
    User <|-- Receptionist
    User <|-- Admin

    Member "1" --> "0..*" Membership : has
    Membership "*" --> "1" MembershipPlan : based on
    Membership "1" --> "0..*" Payment : paid by
    Member "1" --> "0..*" Booking : makes
    GymClass "1" --> "0..*" Booking : has
    Trainer "1" --> "0..*" GymClass : teaches
    Trainer "1" --> "0..*" WorkoutPlan : creates
    Member "1" --> "0..*" WorkoutPlan : follows
    Member "1" --> "0..*" Attendance : records
    Admin "1" --> "0..*" Equipment : manages
```

---

## طريقة الرفع على GitHub

1. اعمل repository جديد.
2. ارفع الملف ده باسم `README.md`.
3. GitHub هيرندر كل الـ diagrams تلقائيًا.
4. ابعت لينك الـ repo للمعيد.
