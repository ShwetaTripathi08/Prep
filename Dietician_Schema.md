
Below is a detailed schema for the **Dietician Web Application** database, designed to support features like user management, diet plans, health metrics tracking, and analytics.  

### **Database Schema:**

#### **1. Users Table**
Stores user profile information.  
```sql
CREATE TABLE Users (
    UserID SERIAL PRIMARY KEY,
    FullName VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    PasswordHash VARCHAR(255) NOT NULL,
    Gender VARCHAR(10) CHECK (Gender IN ('Male', 'Female', 'Other')),
    DateOfBirth DATE NOT NULL,
    HeightCm FLOAT NOT NULL,
    WeightKg FLOAT NOT NULL,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **2. HealthMetrics Table**
Tracks users’ health metrics over time.  
```sql
CREATE TABLE HealthMetrics (
    MetricID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    Date DATE NOT NULL,
    BloodPressure VARCHAR(20),
    CholesterolLevel FLOAT,
    BloodSugarLevel FLOAT,
    WeightKg FLOAT,
    Notes TEXT,
    RecordedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **3. DietPlans Table**
Stores diet plans generated for users.  
```sql
CREATE TABLE DietPlans (
    PlanID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    CreatedDate DATE NOT NULL,
    ValidFrom DATE NOT NULL,
    ValidTo DATE NOT NULL,
    TotalCalories INT NOT NULL,
    Notes TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **4. DietPlanDetails Table**
Stores details of individual meals within a diet plan.  
```sql
CREATE TABLE DietPlanDetails (
    DetailID SERIAL PRIMARY KEY,
    PlanID INT REFERENCES DietPlans(PlanID),
    MealType VARCHAR(20) CHECK (MealType IN ('Breakfast', 'Lunch', 'Dinner', 'Snack')),
    FoodItem VARCHAR(100),
    Quantity VARCHAR(50),
    Calories INT NOT NULL,
    Protein FLOAT,
    Carbs FLOAT,
    Fats FLOAT
);
```

#### **5. ActivityLogs Table**
Logs API requests and user activities for tracking and debugging.  
```sql
CREATE TABLE ActivityLogs (
    LogID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    Action VARCHAR(255) NOT NULL,
    StatusCode INT,
    Timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    Details JSONB
);
```

#### **6. Notifications Table**
Tracks alerts or messages sent to users.  
```sql
CREATE TABLE Notifications (
    NotificationID SERIAL PRIMARY KEY,
    UserID INT REFERENCES Users(UserID),
    NotificationType VARCHAR(50) CHECK (NotificationType IN ('Alert', 'Reminder', 'Update')),
    Message TEXT NOT NULL,
    SentAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    IsRead BOOLEAN DEFAULT FALSE
);
```

#### **7. FoodItems Table**
Stores a database of food items for diet plan generation.  
```sql
CREATE TABLE FoodItems (
    FoodID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Category VARCHAR(50),
    Calories INT NOT NULL,
    Protein FLOAT,
    Carbs FLOAT,
    Fats FLOAT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **8. AuditLogs Table**
Tracks system-level changes for compliance and debugging.  
```sql
CREATE TABLE AuditLogs (
    AuditID SERIAL PRIMARY KEY,
    TableName VARCHAR(50) NOT NULL,
    Operation VARCHAR(20) CHECK (Operation IN ('INSERT', 'UPDATE', 'DELETE')),
    OldData JSONB,
    NewData JSONB,
    PerformedBy INT REFERENCES Users(UserID),
    Timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **Relationships and Explanation**  
1. **Users and HealthMetrics**: A one-to-many relationship where each user can have multiple health metric entries over time.  
2. **Users and DietPlans**: A one-to-many relationship where each user can have multiple diet plans.  
3. **DietPlans and DietPlanDetails**: A one-to-many relationship detailing each meal within a plan.  
4. **Users and Notifications**: A one-to-many relationship for sending user-specific alerts and reminders.  
5. **FoodItems**: Acts as a reference table for generating diet plans.  
6. **ActivityLogs**: Captures all API interactions for debugging and analysis.  

### **Potential Enhancements**
- **Indexes**: Add indexes on `Email` (Users table), `UserID` (HealthMetrics, DietPlans), and `Timestamp` (ActivityLogs, AuditLogs) for faster lookups.  
- **Triggers**: Implement triggers for audit logging of sensitive operations.  
- **Partitioning**: Use table partitioning for large tables like `ActivityLogs` to improve query performance.  



#### **1. Patients Table**
Stores core information about patients.  
```sql
CREATE TABLE Patients (
    PatientID SERIAL PRIMARY KEY,
    FullName VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    PhoneNumber VARCHAR(15),
    Gender VARCHAR(10) CHECK (Gender IN ('Male', 'Female', 'Other')),
    DateOfBirth DATE NOT NULL,
    Address TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **2. PatientDetails Table**
Holds additional health and lifestyle information about the patient.  
```sql
CREATE TABLE PatientDetails (
    PatientDetailID SERIAL PRIMARY KEY,
    PatientID INT REFERENCES Patients(PatientID),
    HeightCm FLOAT,
    WeightKg FLOAT,
    BloodType VARCHAR(3),
    Allergies TEXT,
    MedicalHistory TEXT,
    LifestyleNotes TEXT,  -- e.g., dietary preferences, activity levels
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **3. ClinicalVisits Table**
Tracks clinical visits and associated data.  
```sql
CREATE TABLE ClinicalVisits (
    VisitID SERIAL PRIMARY KEY,
    PatientID INT REFERENCES Patients(PatientID),
    VisitDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    ReasonForVisit TEXT,
    Diagnoses TEXT,
    Prescriptions TEXT,
    FollowUpDate DATE,
    Notes TEXT
);
```

#### **4. VisitMetrics Table**
Logs detailed metrics collected during clinical visits (e.g., vitals).  
```sql
CREATE TABLE VisitMetrics (
    MetricID SERIAL PRIMARY KEY,
    VisitID INT REFERENCES ClinicalVisits(VisitID),
    BloodPressure VARCHAR(20),
    PulseRate INT,
    BloodSugarLevel FLOAT,
    CholesterolLevel FLOAT,
    BMI FLOAT,
    Notes TEXT
);
```

#### **5. DieticianNotes Table**
Allows dieticians to add notes or recommendations specific to the patient.  
```sql
CREATE TABLE DieticianNotes (
    NoteID SERIAL PRIMARY KEY,
    PatientID INT REFERENCES Patients(PatientID),
    VisitID INT REFERENCES ClinicalVisits(VisitID),
    DietRecommendations TEXT,
    AdditionalAdvice TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

### **Relationships**
1. **Patients and PatientDetails**: A one-to-one relationship to store additional health and lifestyle details.  
2. **Patients and ClinicalVisits**: A one-to-many relationship where each patient can have multiple clinical visits.  
3. **ClinicalVisits and VisitMetrics**: A one-to-one relationship capturing metrics for each clinical visit.  
4. **Patients and DieticianNotes**: A one-to-many relationship to store dietician-specific advice and observations for each patient.

---

### **Expanded Explanation**
- **Patients**: The core table representing individuals receiving services from the application.  
- **PatientDetails**: A table for storing additional health and lifestyle attributes that are not part of basic demographic data.  
- **ClinicalVisits**: Records each interaction a patient has with the dietician, including diagnoses, prescriptions, and follow-up schedules.  
- **VisitMetrics**: Logs detailed health metrics like vitals, enabling personalized diet and health recommendations.  
- **DieticianNotes**: Facilitates dieticians in documenting recommendations specific to the patient’s visits.

---

### **Integration with Existing Schema**
- Link **Patients** to the **Users** table if the patients are also users of the application.
- Incorporate **DietPlans** and **DietPlanDetails** for managing diet plans assigned to each patient.

---

### **Sample Queries**
1. **Retrieve Patient's Visit History**
   ```sql
   SELECT FullName, VisitDate, ReasonForVisit, Diagnoses 
   FROM Patients
   JOIN ClinicalVisits ON Patients.PatientID = ClinicalVisits.PatientID
   WHERE Patients.PatientID = 123;
   ```

2. **Get All Diet Recommendations for a Patient**
   ```sql
   SELECT FullName, DietRecommendations, AdditionalAdvice
   FROM Patients
   JOIN DieticianNotes ON Patients.PatientID = DieticianNotes.PatientID
   WHERE Patients.PatientID = 123;
   ```

