Here’s a **GitHub README.md** version of your **Mercedes-Benz SQL Guides**, formatted for easy readability and engagement! 🚀  

---

# **Mercedes-Benz Dealership SQL Database Guide** 🚗💨  

This repository contains **SQL scripts** for creating a **Mercedes-Benz dealership database**, including inventory, sales, customer management, and **historical vehicle tracking**.  

## **📌 Features of This Guide**  
✅ **Full database setup** – Tables for vehicles, customers, sales, and services  
✅ **Historical tracking** – Price changes, ownership history, and service records  
✅ **SQL queries** – Retrieve insights on dealership performance  
✅ **Automated triggers** – Keep history tables updated  

---

## **📂 Database Structure**  

### **1️⃣ Core Tables**  
- `Inventory` – Stores vehicle details  
- `Customers` – Holds customer information  
- `Sales` – Tracks vehicle sales  
- `ServiceRecords` – Logs service history  

### **2️⃣ History Tables**  
- `VehiclePriceHistory` – Tracks price changes over time  
- `OwnershipHistory` – Stores previous vehicle owners  
- `ServiceHistory` – Records past maintenance  

---

## **🔹 Step 1: Creating the Database**  

```sql
CREATE DATABASE MercedesDealership;
USE MercedesDealership;
```

---

## **🔹 Step 2: Creating Core Tables**  

```sql
CREATE TABLE Inventory (
    VehicleID INT PRIMARY KEY IDENTITY(1,1),
    Make VARCHAR(50),
    Model VARCHAR(50),
    Year INT,
    Price DECIMAL(10,2)
);

CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY IDENTITY(1,1),
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Email VARCHAR(100)
);

CREATE TABLE Sales (
    SaleID INT PRIMARY KEY IDENTITY(1,1),
    VehicleID INT FOREIGN KEY REFERENCES Inventory(VehicleID),
    CustomerID INT FOREIGN KEY REFERENCES Customers(CustomerID),
    SaleDate DATETIME DEFAULT GETDATE(),
    SalePrice DECIMAL(10,2)
);
```

---

## **🔹 Step 3: Creating History Tables**  

### **Vehicle Price History**  
```sql
CREATE TABLE VehiclePriceHistory (
    HistoryID INT PRIMARY KEY IDENTITY(1,1),
    VehicleID INT FOREIGN KEY REFERENCES Inventory(VehicleID),
    OldPrice DECIMAL(10,2),
    NewPrice DECIMAL(10,2),
    ChangeDate DATETIME DEFAULT GETDATE()
);
```

### **Ownership History**  
```sql
CREATE TABLE OwnershipHistory (
    HistoryID INT PRIMARY KEY IDENTITY(1,1),
    VehicleID INT FOREIGN KEY REFERENCES Inventory(VehicleID),
    PreviousOwnerID INT FOREIGN KEY REFERENCES Customers(CustomerID),
    SaleDate DATETIME DEFAULT GETDATE(),
    SalePrice DECIMAL(10,2)
);
```

### **Service History**  
```sql
CREATE TABLE ServiceHistory (
    HistoryID INT PRIMARY KEY IDENTITY(1,1),
    VehicleID INT FOREIGN KEY REFERENCES Inventory(VehicleID),
    ServiceDate DATETIME DEFAULT GETDATE(),
    ServiceType VARCHAR(100),
    Cost DECIMAL(10,2),
    PerformedBy VARCHAR(100)
);
```

---

## **🔹 Step 4: Inserting Sample Data**  

```sql
INSERT INTO VehiclePriceHistory (VehicleID, OldPrice, NewPrice, ChangeDate)
VALUES 
(1, 45000.00, 43000.00, '2024-12-01'),
(2, 55000.00, 53000.00, '2025-01-15');

INSERT INTO OwnershipHistory (VehicleID, PreviousOwnerID, SaleDate, SalePrice)
VALUES 
(1, 2, '2024-11-10', 42000.00),
(2, 1, '2025-02-20', 52000.00);

INSERT INTO ServiceHistory (VehicleID, ServiceDate, ServiceType, Cost, PerformedBy)
VALUES 
(1, '2024-10-01', 'Brake Replacement', 800.00, 'Mercedes-Benz Service Center'),
(2, '2025-03-01', 'Oil Change', 150.00, 'Luxury Auto Care');
```

---

## **🔹 Step 5: Querying Data**  

### **1. Get Vehicle Price Change History**  
```sql
SELECT I.Make, I.Model, I.Year, VPH.OldPrice, VPH.NewPrice, VPH.ChangeDate
FROM VehiclePriceHistory VPH
JOIN Inventory I ON VPH.VehicleID = I.VehicleID
ORDER BY VPH.ChangeDate DESC;
```

### **2. View Past Owners of a Vehicle**  
```sql
SELECT I.Make, I.Model, I.Year, C.FirstName, C.LastName, OH.SaleDate, OH.SalePrice
FROM OwnershipHistory OH
JOIN Inventory I ON OH.VehicleID = I.VehicleID
JOIN Customers C ON OH.PreviousOwnerID = C.CustomerID
ORDER BY OH.SaleDate DESC;
```

### **3. Retrieve Full Service History for a Vehicle**  
```sql
SELECT I.Make, I.Model, I.Year, SH.ServiceDate, SH.ServiceType, SH.Cost, SH.PerformedBy
FROM ServiceHistory SH
JOIN Inventory I ON SH.VehicleID = I.VehicleID
ORDER BY SH.ServiceDate DESC;
```

---

## **🔹 Step 6: Automating History Tracking with Triggers**  

### **Track Price Changes Automatically**  
```sql
CREATE TRIGGER trg_PriceChange
ON Inventory
AFTER UPDATE
AS
BEGIN
    INSERT INTO VehiclePriceHistory (VehicleID, OldPrice, NewPrice, ChangeDate)
    SELECT i.VehicleID, d.Price, i.Price, GETDATE()
    FROM inserted i
    JOIN deleted d ON i.VehicleID = d.VehicleID
    WHERE i.Price <> d.Price;
END;
```

### **Track Ownership Changes Automatically**  
```sql
CREATE TRIGGER trg_OwnershipChange
ON Sales
AFTER INSERT
AS
BEGIN
    INSERT INTO OwnershipHistory (VehicleID, PreviousOwnerID, SaleDate, SalePrice)
    SELECT i.VehicleID, i.CustomerID, GETDATE(), i.SalePrice
    FROM inserted i;
END;
```

---

## **🚀 Conclusion**  
This **Mercedes-Benz Dealership SQL Guide** provides:  
✅ **A structured SQL database** for vehicle sales & service  
✅ **Tracking for historical data** (price, ownership, service)  
✅ **Ready-to-use SQL queries** for dealership insights  
✅ **Automated triggers** to maintain data integrity  

💡 **How can you improve this database?** Fork the repo and contribute! 🔥  

---

## **📌 Next Steps**  
1️⃣ **Fork this repo** and try out the SQL scripts.  
2️⃣ Modify the schema to fit real dealership needs.  
3️⃣ Share your feedback & improvements!  

📢 **Follow me for more SQL & AI-driven projects!**  

#SQL #DataAnalytics #MercedesBenz #DatabaseDesign #GitHub #BusinessIntelligence 🚗💨  

---

This **README.md** is **GitHub-ready**—just **copy and paste** it into your **README.md** file. Let me know if you need any tweaks! 🚀🔥
