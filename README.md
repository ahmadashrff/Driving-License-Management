# 🚗 DVLD — Driving & Vehicle License Department System

A comprehensive **Windows Forms desktop application** built with **C# .NET** for managing driving license operations. The system covers the full lifecycle of driver licensing — from person registration and driving tests to license issuance, renewal, replacement, detention, and international licenses.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Database Setup](#database-setup)
- [Getting Started](#getting-started)
- [Application Modules](#application-modules)
- [License Classes & Test Flow](#license-classes--test-flow)

---

## Overview

The **DVLD (Driving & Vehicle License Department)** system is a desktop application that simulates the operations of a government licensing authority. It manages:

- **People** — citizens registered in the system
- **Drivers** — licensed individuals
- **Applications** — requests submitted for various license services
- **Tests** — vision, written, and street driving tests
- **Local Licenses** — domestic driving licenses per class
- **International Licenses** — internationally recognized licenses
- **Users** — system operators/employees with authentication

The system follows a **3-tier architecture** (Presentation → Business Logic → Data Access) for clean separation of concerns.

---

## Features

### 👤 People Management
- Add, update, and delete person records
- Store full name, national ID, date of birth, gender, address, contact info, nationality, and photo

### 🪪 Driver Management
- Register drivers linked to person records
- View driver license history

### 📝 Application Processing
- Manage multiple application types with configurable fees
- Track application status (New, Cancelled, Completed)

### 🧪 Testing System
- Schedule test appointments (Vision Test, Written Test, Street Drive Test)
- Record test results (Pass/Fail) with notes
- Require all 3 tests to be passed before issuing a license
- Support retake scheduling for failed tests

### 🏷️ Local Driving Licenses
- Issue licenses for multiple vehicle classes
- Automatic expiry date calculation based on license class validity period
- License renewal with fee processing
- Replacement for lost or damaged licenses
- Detain and release detained licenses

### 🌍 International Licenses
- Issue international licenses backed by an active local license
- 1-year validity period
- One active international license per driver

### 👥 User & Authentication
- Secure login with username and password
- User management (add, update, activate/deactivate)
- Change password functionality
- Role-based access via system users

### ⚙️ System Configuration
- Manage application types and fees
- Manage test types
- Manage license classes and validity durations

---

## Architecture

The solution is split into **3 projects** following a clean 3-tier architecture:

```
┌─────────────────────────────────────┐
│         DVLD (Presentation)         │
│     Windows Forms UI Application    │
└──────────────┬──────────────────────┘
               │ references
┌──────────────▼──────────────────────┐
│      DVLD_Business (BLL)            │
│   Business Logic Layer (Classes)    │
└──────────────┬──────────────────────┘
               │ references
┌──────────────▼──────────────────────┐
│      DVLD_DataAccess (DAL)          │
│  Data Access Layer (SQL Server)     │
└─────────────────────────────────────┘
```

| Layer | Project | Responsibility |
|-------|---------|----------------|
| **Presentation** | `DVLD` | Windows Forms UI, forms, controls |
| **Business Logic** | `DVLD_Buisness` | Business rules, entity classes, workflows |
| **Data Access** | `DVLD_DataAccess` | SQL Server queries, stored procedure calls |

---

## Project Structure

```
DVLD/
├── DVLD/                              # Presentation Layer (WinForms)
│   ├── Applications/
│   │   ├── Application Types/         # Manage application type forms
│   │   ├── International License/     # International license application forms
│   │   ├── Local Driving License/     # Local license application forms
│   │   ├── Renew Local License/       # License renewal forms
│   │   ├── ReplaceLostOrDamagedLicense/
│   │   └── Rlease Detained License/   # Detained license release forms
│   ├── Drivers/                       # Driver list & management forms
│   ├── Global Classes/                # Shared utilities (formatting, validation)
│   ├── Licenses/
│   │   ├── Detain License/
│   │   ├── International Licenses/
│   │   └── Local Licenses/
│   ├── Login/                         # Authentication forms
│   ├── People/                        # People management forms
│   ├── Tests/                         # Test type management forms
│   ├── User/                          # User management forms
│   ├── Database File/                 # DVLD.back (SQL Server backup)
│   ├── frmMain.cs                     # Main application form
│   └── Program.cs                     # Application entry point
│
├── DVLD_Buisness/                     # Business Logic Layer
│   ├── clsApplication.cs             # Base application class
│   ├── clsApplicationType.cs         # Application type & fees
│   ├── clsCountry.cs                 # Country/nationality info
│   ├── clsDetainedLicense.cs         # License detention management
│   ├── clsDriver.cs                  # Driver entity
│   ├── clsInternationalLicense.cs    # International license logic
│   ├── clsLicense.cs                 # Local license logic
│   ├── clsLicenseClass.cs            # License class (A, B, C, D...)
│   ├── clsLocalDrivingLicenseApplication.cs
│   ├── clsPerson.cs                  # Person entity
│   ├── clsTest.cs                    # Test result entity
│   ├── clsTestAppointment.cs         # Test scheduling
│   ├── clsTestType.cs                # Test type definitions
│   └── clsUser.cs                    # User/employee entity
│
└── DVLD_DataAccess/                   # Data Access Layer
    ├── clsApplication.cs
    ├── clsCountryData.cs
    ├── clsDataAccessSettings.cs      # DB connection string
    ├── clsDetainedLicense.cs
    ├── clsDriver.cs
    ├── clsInternationalLicense.cs
    ├── clsLicense.cs
    ├── clsLocalDrivingLicenseApplicationData.cs
    ├── clsPersonData.cs
    ├── clsTest.cs
    ├── clsTestAppointment.cs
    ├── clsUserData.cs
    └── LicenseClass.cs
```

---

## Technologies Used

| Technology | Details |
|------------|---------|
| **Language** | C# (.NET Framework) |
| **UI Framework** | Windows Forms (WinForms) |
| **Database** | Microsoft SQL Server |
| **Data Access** | ADO.NET (`SqlConnection`, `SqlCommand`, `SqlDataReader`) |
| **IDE** | Visual Studio |
| **Architecture** | 3-Tier (Presentation / BLL / DAL) |

---

## Database Setup

1. Open **SQL Server Management Studio (SSMS)**
2. Restore the database from the backup file located at:
   ```
   DVLD/Database File/DVLD.back
   ```
3. Right-click **Databases** → **Restore Database** → select the `.back` file
4. Ensure the restored database name is `DVLD`

### Connection String

The default connection string is set in `DVLD_DataAccess/clsDataAccessSettings.cs`:

```csharp
public static string ConnectionString = "Server=. ; Database=DVLD ; User Id=sa ; Password=sa123456 ;";
```

> **⚠️ Important:** Update `Server`, `User Id`, and `Password` to match your SQL Server instance before running.

---

## Getting Started

### Prerequisites

- Windows OS
- [Visual Studio 2019 or later](https://visualstudio.microsoft.com/)
- [.NET Framework 4.x](https://dotnet.microsoft.com/download/dotnet-framework)
- [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) (Express or higher)

### Setup Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/DVLD.git
   ```

2. **Restore the database**
   - Open SSMS and restore `DVLD/Database File/DVLD.back`

3. **Configure the connection string**
   - Open `DVLD_DataAccess/clsDataAccessSettings.cs`
   - Update the `ConnectionString` to match your SQL Server credentials

4. **Open the solution**
   - Open `DVLD/DVLD.sln` in Visual Studio

5. **Build and run**
   - Set `DVLD` as the startup project
   - Press `F5` to build and run

6. **Login**
   - Use the default credentials configured in your database

---

## Application Modules

| Module | Description |
|--------|-------------|
| **People** | Register and manage citizen records |
| **Drivers** | View and manage licensed drivers |
| **Local License Applications** | Apply for a new local driving license |
| **International License** | Issue international driving licenses |
| **Renew License** | Process license renewals |
| **Replace License** | Issue replacements for lost or damaged licenses |
| **Detain License** | Detain a driver's license with fine |
| **Release Detained License** | Release a detained license after payment |
| **Manage Tests** | Schedule appointments and record test results |
| **Users / Employees** | Manage system user accounts |
| **Application Types** | Configure service types and fees |
| **Test Types** | Configure available driving test types |

---

## License Classes & Test Flow

To obtain a **Local Driving License**, an applicant must:

1. Submit a **Local Driving License Application**
2. Pass **3 sequential tests**:
   - 👁️ **Vision Test**
   - 📝 **Written Theory Test**
   - 🚗 **Street Driving Test**
3. Upon passing all tests → a **License** is issued automatically

> Failed tests can be retaken by scheduling a new appointment. Each retake requires a new application fee.

---

## Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## License

This project is open-source and available under the [MIT License](LICENSE).

