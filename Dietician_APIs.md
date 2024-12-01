Here’s a detailed list of APIs and their endpoints for the **Dietician Web Application**, along with examples for using them in **Postman**. These APIs are categorized based on their functionality.

---

## **1. Authentication APIs**
### **a. User Login**
- **Endpoint**: `POST /api/auth/login`
- **Description**: Authenticates users and returns a JWT token.
- **Headers**:
  - Content-Type: `application/json`
- **Request Body**:
  ```json
  {
    "email": "user@example.com",
    "password": "password123"
  }
  ```
- **Response**:
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR..."
  }
  ```
- **Example in Postman**:
  - Select `POST`.
  - Add the endpoint URL.
  - Add the JSON body in the `Body` tab.
  - Send the request to get the JWT token.

---

### **b. User Registration**
- **Endpoint**: `POST /api/auth/register`
- **Description**: Registers a new user.
- **Headers**:
  - Content-Type: `application/json`
- **Request Body**:
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "securePass!123",
    "role": "patient"
  }
  ```
- **Response**:
  ```json
  {
    "message": "User registered successfully",
    "userId": 101
  }
  ```

---

## **2. Patient Management APIs**
### **a. Get All Patients**
- **Endpoint**: `GET /api/patients`
- **Description**: Fetches a list of all patients.
- **Headers**:
  - Authorization: `Bearer <JWT token>`
- **Response**:
  ```json
  [
    {
      "id": 1,
      "name": "Alice Smith",
      "age": 30,
      "lastVisit": "2024-11-01"
    },
    {
      "id": 2,
      "name": "Bob Johnson",
      "age": 40,
      "lastVisit": "2024-11-10"
    }
  ]
  ```
- **Example in Postman**:
  - Select `GET`.
  - Add the endpoint URL.
  - Add the `Authorization` token in the `Headers` tab.

---

### **b. Get Patient Details**
- **Endpoint**: `GET /api/patients/{id}`
- **Description**: Fetches detailed information about a patient.
- **Headers**:
  - Authorization: `Bearer <JWT token>`
- **Response**:
  ```json
  {
    "id": 1,
    "name": "Alice Smith",
    "age": 30,
    "medicalHistory": "Diabetes",
    "dietPlans": []
  }
  ```

---

### **c. Add New Patient**
- **Endpoint**: `POST /api/patients`
- **Description**: Adds a new patient to the system.
- **Headers**:
  - Content-Type: `application/json`
  - Authorization: `Bearer <JWT token>`
- **Request Body**:
  ```json
  {
    "name": "Charlie Brown",
    "age": 28,
    "contact": "charlie@example.com",
    "medicalHistory": "None"
  }
  ```
- **Response**:
  ```json
  {
    "message": "Patient added successfully",
    "patientId": 3
  }
  ```

---

## **3. Clinical Visits APIs**
### **a. Add Visit**
- **Endpoint**: `POST /api/visits`
- **Description**: Logs a clinical visit for a patient.
- **Headers**:
  - Content-Type: `application/json`
  - Authorization: `Bearer <JWT token>`
- **Request Body**:
  ```json
  {
    "patientId": 1,
    "visitDate": "2024-12-01",
    "notes": "Patient showing improvement.",
    "healthMetrics": {
      "weight": 70,
      "bloodPressure": "120/80"
    }
  }
  ```
- **Response**:
  ```json
  {
    "message": "Visit recorded successfully",
    "visitId": 15
  }
  ```

---

### **b. Get Patient Visits**
- **Endpoint**: `GET /api/patients/{id}/visits`
- **Description**: Fetches a patient’s clinical visits.
- **Response**:
  ```json
  [
    {
      "visitId": 15,
      "visitDate": "2024-12-01",
      "notes": "Patient showing improvement."
    }
  ]
  ```

---

## **4. Diet Plan APIs**
### **a. Create Diet Plan**
- **Endpoint**: `POST /api/diet-plans`
- **Description**: Creates a new diet plan for a patient.
- **Request Body**:
  ```json
  {
    "patientId": 1,
    "startDate": "2024-12-01",
    "endDate": "2024-12-07",
    "meals": [
      {
        "day": "Monday",
        "breakfast": "Oatmeal",
        "lunch": "Grilled chicken salad",
        "dinner": "Soup and bread"
      }
    ]
  }
  ```
- **Response**:
  ```json
  {
    "message": "Diet plan created successfully",
    "planId": 5
  }
  ```

---

### **b. Get Diet Plans**
- **Endpoint**: `GET /api/patients/{id}/diet-plans`
- **Description**: Retrieves diet plans for a patient.
- **Response**:
  ```json
  [
    {
      "planId": 5,
      "startDate": "2024-12-01",
      "endDate": "2024-12-07",
      "status": "active"
    }
  ]
  ```

---

## **5. Appointment Management APIs**
### **a. Schedule Appointment**
- **Endpoint**: `POST /api/appointments`
- **Description**: Schedules a new appointment.
- **Request Body**:
  ```json
  {
    "patientId": 1,
    "dieticianId": 2,
    "appointmentDate": "2024-12-05",
    "notes": "Initial consultation"
  }
  ```
- **Response**:
  ```json
  {
    "message": "Appointment scheduled",
    "appointmentId": 7
  }
  ```

---

### **b. Get Appointments**
- **Endpoint**: `GET /api/appointments`
- **Description**: Fetches all appointments.
- **Response**:
  ```json
  [
    {
      "appointmentId": 7,
      "patientName": "Alice Smith",
      "dieticianName": "Dr. John Doe",
      "appointmentDate": "2024-12-05"
    }
  ]
  ```

---

