# Community Database Management System

A comprehensive Relational Database Management System (RDBMS) designed to manage university student communities. This project specifically focuses on **membership tracking**, **board game inventory management**, and **lending transactions** with automated penalty calculations.

## Project Overview

This project was created to digitize the administrative workflow of a student club. It moves away from manual spreadsheets to a structured SQL Server database, ensuring data integrity and efficient querying for reporting.

### Key Features

* **Membership Management:**
    * Tracks member details (Student ID, Phone, Active Status).
    * Categorizes members by types (e.g., Board Member, General Member) and manages dues.
* **Inventory & Archive:**
    * Categorized inventory system for board games (Strategy, Party, Abstract, etc.).
    * **Archiving System:** A dedicated `OyunArsiv` table to store history of deleted or removed inventory items.
* **Lending & Circulation:**
    * Tracks which member borrowed which game, including issue dates and return dates.
* **Automated Logic (Functions):**
    * Includes a custom Scalar-Valued Function **`fn_CezaHesapla`**.
    * *Logic:* Automatically calculates a fine (5.00 units per day) if a borrowed item is returned after the 7-day limit.

##  Technical Details

* **Database Engine:** Microsoft SQL Server (MSSQL)
* **Scripting:** T-SQL
* **Database Objects:**
    * **Tables:** 10+ Related Tables (Normalised schema)
    * **Functions:** `fn_CezaHesapla` (Date difference and conditional logic)
    * **Relationships:** Primary Keys (PK) and Foreign Keys (FK) enforced for data integrity.

##  Database Schema

Below is the Entity-Relationship (ER) Diagram of the system:

![ER Diagram](https://placehold.co/600x400?text=Please+Upload+Your+ER+Diagram+Image)

## 🚀 Installation & Usage

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/yourusername/Community-Database-Management-System.git](https://github.com/yourusername/Community-Database-Management-System.git)
    ```
2.  **Open SQL Server Management Studio (SSMS).**
3.  **Run the Script:**
    * Open `FullDatabaseScript.sql`.
    * Execute the script. It will automatically create the database `ToplulukYonetimDB`, create all tables, establish relationships, and insert sample seed data.

## 📝 SQL Query Examples

**1. Calculate Fines for Overdue Returns:**
```sql
SELECT 
    u.AdSoyad AS MemberName, 
    o.OyunAdi AS GameName, 
    i.TeslimTarihi AS ReturnDate,
    dbo.fn_CezaHesapla(i.AlisTarihi, i.TeslimTarihi) AS FineAmount
FROM OduncIslemleri i
JOIN Uyeler u ON i.UyeId = u.Id
JOIN Oyunlar o ON i.OyunId = o.Id
WHERE dbo.fn_CezaHesapla(i.AlisTarihi, i.TeslimTarihi) > 0;
