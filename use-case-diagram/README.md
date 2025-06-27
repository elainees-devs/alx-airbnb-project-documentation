# Airbnb Clone - Use Case Diagram

## 📌 Objective

To visualize system interactions through a **Use Case Diagram** that highlights how users interact with the core functionalities of an Airbnb-style rental platform.

---

## 🧩 Core Functionalities

The major functionalities represented in the diagram:

### 🔐 User Management
- **User Registration**  
  Users (guests or hosts) can register securely.
- **Login & Authentication**  
  Includes login via email/password and third-party OAuth (Google, Facebook).
- **Profile Management**  
  Users can update personal information.

### 🏡 Property Management
- **List Property**  
  Hosts can add new property listings.
- **Edit/Delete Property**  
  Hosts manage their listings.

### 📅 Booking System
- **Search Properties**  
  Guests can search based on filters like location, price, availability.
- **Book a Property**  
  Guests can book available listings.
- **View Booking History**  
  Users can see past and upcoming bookings.

### 💳 Payments
- **Make Payment**  
  Guests can make secure payments for bookings.
- **Receive Payment**  
  Hosts receive earnings for completed bookings.

---

## 👤 Actors

- **Guest**
- **Host**
- **Admin** *(optional, for system moderation)*
- **OAuth**
- **Payment Gateway**

---

## 🛠️ Tools

- [Draw.io](https://draw.io)

---

## 📂 Deliverables

- `use-case-diagram.png` – Exported visual of the diagram.

---

## ✅ Use Case Flow

- A **Guest** registers/logins, searches for properties, books one, and makes a payment.
- A **Host** registers/logins, view booking status, respond to reviews
- An **Admin** registers, lists a property, and receives payment after a booking.
- A **Payment Gateway** Make Payment, Receives Payout
- **OAuth** Login with OAuth
