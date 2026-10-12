# AcxiomCRM — Customer Relationship Management System

A full-stack ASP.NET Core MVC CRM starter built to the supplied AcxiomCRM assignment specification. Includes Identity login/register/logout, role-aware navigation, customer/lead/opportunity/follow-up/activity management, dashboard analytics, reports, audit logging and REST APIs.

## Tech stack
- ASP.NET Core MVC (.NET 8), C# and Razor Views
- ASP.NET Core Identity (adaptive password hashing, cookie authentication, lockout and roles)
- Entity Framework Core + SQL Server
- Bootstrap 5, Bootstrap Icons, Chart.js
- REST endpoints using DTO-style responses and validation

## Prerequisites
1. .NET 8 SDK
2. SQL Server Express / LocalDB / SQL Server Developer edition
3. Visual Studio 2022 or VS Code (C# Dev Kit)

## Run locally
1. Open `appsettings.json` and set `ConnectionStrings:DefaultConnection` to your SQL Server instance. The included default is Windows LocalDB:
   `Server=(localdb)\MSSQLLocalDB;Database=AcxiomCRMDb;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=True`
2. From the `AcxiomCRM` folder, run:
   ```bash
   dotnet restore
   dotnet tool install --global dotnet-ef --version 8.0.11
   dotnet ef migrations add InitialCreate
   dotnet ef database update
   dotnet run
   ```
   The application attempts to apply migrations and seed roles/admin on startup. Run the migration commands once first for a fresh checkout.
3. Open the local URL shown in the terminal.

## First login (development only)
- Email: `admin@acxiomcrm.local`
- Password: `Admin@12345!`

Change this password immediately after first login. The seeded account is for local evaluation only; do not use these credentials in production.

## Main modules
- Dashboard with customer, lead, opportunity and pipeline KPIs; Chart.js lead-status and opportunity-stage charts
- Customers: create, search, details, edit and deactivate; unique email/phone checks
- Leads: create, search/filter, edit statuses and convert qualified leads into customers
- Opportunities: amount, probability, stage, expected close date and weighted value
- Follow-ups: schedule, filter, complete and mark missed
- Activities: calls, meetings, emails and tasks
- Users & roles: Admin-only user creation, role assignment and account activation
- Audit log: login success/failure and business actions
- Reports: pipeline, lead conversion, follow-up summaries and customer CSV export
- API: `/api/customers`, `/api/customers/{id}`, `/api/leads`, `/api/opportunities`

## Role model
- **Admin:** user administration, all CRM modules and audit log.
- **Manager:** team-level CRM work and reporting; can deactivate/delete records where supported.
- **Sales Executive:** CRM workspace access; use ownership scoping as a next hardening step before production.

## API notes
API routes require an authenticated Identity cookie. Try endpoints after signing in, or use browser developer tools / an API client with the same cookie. Swagger UI is available at `/swagger` in Development. `POST /api/customers` and `PUT /api/customers/{id}` accept JSON like:
```json
{
  "customerName": "Aarav Sharma",
  "email": "aarav@example.com",
  "phone": "9876543210",
  "companyName": "Example Ltd"
}
```

## Important evaluation/security notes
- Client-side Razor validation and server-side model/business validation are enabled for forms.
- Identity hashes passwords; no custom password table or plain-text password storage is used.
- Lockout after five failed attempts for 15 minutes, anti-forgery tokens on MVC state-changing forms, and role-protected user/audit endpoints are configured.
- Opportunity amount must be greater than zero, probability 0–100, active expected close date cannot be in the past, and new follow-up dates cannot be in the past.
- Audit records are append-oriented through the app UI; production database permissions should also prevent audit tampering.
- Before production, add automated tests, ownership/team-level query filters across every MVC/API action, HTTPS-only deployment configuration, secret management, rate limiting and a controlled admin-password bootstrap. The assignment requires resource-level authorization, so verify and extend per-record ownership policies for your evaluator's exact expectations.

## Repository structure
```
Controllers/       MVC and REST API controllers
Data/              EF Core DbContext and seed data
Models/            Identity user and CRM entities
Services/          Audit logging service
ViewModels/        View helpers / form view models
Views/             Razor pages for all main modules
wwwroot/           Custom CSS and JavaScript
```

## GitHub submission
1. Create a public GitHub repository named `AcxiomCRM`.
2. Extract this ZIP, open the extracted folder, and run `git init` if needed.
3. Commit the project and push it to GitHub.
4. Paste the repository URL into the assignment Google Form.

Do not upload database credentials, personal data or production secrets. This is a project baseline intended for local setup and review; run it and verify the acceptance checklist before submission.
