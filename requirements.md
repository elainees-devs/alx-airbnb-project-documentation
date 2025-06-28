## Backend Requirement Specifications

### ✅ 1. User Authentication

#### 📌 Feature Summary:

Allow guests and hosts to register, log in (via email/password or OAuth), and manage sessions securely.

#### 🔗 API Endpoints:

| Method | Endpoint             | Description                   |
| ------ | -------------------- | ----------------------------- |
| POST   | `/api/auth/register` | Register a new user           |
| POST   | `/api/auth/login`    | Login via email/password      |
| POST   | `/api/auth/oauth`    | OAuth login (Google/Facebook) |
| GET    | `/api/auth/me`       | Get authenticated user info   |
| POST   | `/api/auth/logout`   | Terminate session             |

#### 📥 Input / 📤 Output Specifications:

**`/register` Input (JSON):**

```json
{
  "first_name": "Jane",
  "last_name": "Doe",
  "email": "jane@example.com",
  "password": "Secret123!",
  "role": "guest"
}
```

**Validation:**

* `email`: must be unique, valid email format
* `password`: min 8 chars, at least one letter and one number
* `role`: must be one of `["guest", "host"]`

**Output:**

```json
{
  "message": "Registration successful",
  "token": "JWT_TOKEN"
}
```

**`/oauth` Input:**

```json
{
  "provider": "google",
  "token": "OAUTH_ACCESS_TOKEN"
}
```

**Output:**

```json
{
  "message": "Login successful",
  "token": "JWT_TOKEN",
  "user": { "id": "uuid", "email": "..." }
}
```

#### ⚙️ Performance & Security:

* Passwords must be hashed with bcrypt
* OAuth must be verified with the provider’s SDK/token introspection
* JWT token expiration: 15 minutes with refresh token support
* Rate limit login attempts: 5/minute per IP

---

### ✅ 2. Property Management

#### 📌 Feature Summary:

Allow hosts (or admins) to create, update, retrieve, and deactivate property listings.

#### 🔗 API Endpoints:

| Method | Endpoint              | Description                       |
| ------ | --------------------- | --------------------------------- |
| POST   | `/api/properties`     | Create a new property             |
| GET    | `/api/properties`     | List/search properties            |
| GET    | `/api/properties/:id` | Get a specific property           |
| PUT    | `/api/properties/:id` | Update a property                 |
| DELETE | `/api/properties/:id` | Deactivate (soft delete) property |

#### 📥 Input / 📤 Output Specifications:

**POST `/properties` Input (JSON):**

```json
{
  "title": "Cozy Cottage",
  "location": "Nairobi",
  "price_per_night": 45,
  "amenities": ["WiFi", "Kitchen"],
  "availability": {
    "start_date": "2025-07-01",
    "end_date": "2025-09-01"
  }
}
```

**Validation:**

* `title`: required, min 5 chars
* `location`: required
* `price_per_night`: positive number
* `availability`: valid ISO date format

**Output:**

```json
{
  "id": "property_uuid",
  "message": "Property listed successfully"
}
```

#### ⚙️ Performance Criteria:

* Properties indexed by location, price, availability
* Paginate results: default page size = 10
* Support filters: price range, location, amenities

---

### ✅ 3. Booking System

#### 📌 Feature Summary:

Enable guests to book available properties and allow hosts to manage their bookings.

#### 🔗 API Endpoints:

| Method | Endpoint             | Description                    |
| ------ | -------------------- | ------------------------------ |
| POST   | `/api/bookings`      | Create a booking               |
| GET    | `/api/bookings`      | Get user bookings              |
| PUT    | `/api/bookings/:id`  | Cancel or modify booking       |
| GET    | `/api/host/bookings` | Host view of received bookings |

#### 📥 Input / 📤 Output Specifications:

**POST `/bookings` Input (JSON):**

```json
{
  "property_id": "property_uuid",
  "start_date": "2025-08-01",
  "end_date": "2025-08-10",
  "guest_count": 2
}
```

**Validation:**

* Dates must be in the future
* `start_date < end_date`
* Check for availability conflict
* Property must be active

**Output:**

```json
{
  "id": "booking_uuid",
  "status": "pending",
  "message": "Booking created successfully"
}
```

#### ⚙️ Performance Criteria:

* Ensure atomic transaction for booking + availability update
* Use locking or re-checking to prevent overbooking
* Auto-cancel unconfirmed bookings after 15 mins (via cron or job queue)
