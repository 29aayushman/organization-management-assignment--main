Organization Management Service – Multi-Tenant Backend (FastAPI + MongoDB)

This project implements a multi-tenant backend system that allows creating and managing organizations, where each organization receives its own dynamically generated MongoDB collection. All organization metadata and admin user details are stored in a dedicated Master Database to maintain scalability, isolation, and structured data management.

The backend is developed using FastAPI with JWT-based authentication and bcrypt-based password hashing.

Features
1. Multi-Tenant Architecture

Each organization receives a separate collection following the pattern:

org_<organization_name>

2. Master Database

The master database stores:

Organization metadata

Admin user credentials (hashed)

Collection and configuration references

3. Secure Authentication

Passwords hashed using bcrypt

JWT tokens generated using python-jose

4. REST API Endpoints

Create Organization

Get Organization Details

Update Organization

Delete Organization

Admin Login

Architecture Diagram

The project includes a high-level architecture diagram located in the repository root:

architecture_diagram.png


This diagram illustrates:
Client/Admin → FastAPI Backend → Master Database → Dynamic Organization Collections

Project Structure
app/
│── main.py
│── db.py
│── schemas.py
│── routes/
│     ├── org_routes.py
│     ├── admin_routes.py
│── utils/
│     └── security.py
.env
requirements.txt
architecture_diagram.png
README.md

Tech Stack

FastAPI – Python Web Framework

MongoDB – NoSQL Database

Motor – Asynchronous MongoDB Client

bcrypt – Password Hashing

python-jose – JWT Authentication

Uvicorn – ASGI Server

Setup Instructions
1. Clone the Repository
git clone <your-repo-url>
cd organization-management-service

2. Create the .env File

Create a .env file in the project root with the following configuration:

MONGO_URL=mongodb://localhost:27017
MASTER_DB=organization_master
JWT_SECRET=your_super_secret_key_here
JWT_EXPIRES_IN=3600

3. Install Dependencies
pip install -r requirements.txt

4. Run the Server
uvicorn app.main:app --reload


The application will be available at:

http://127.0.0.1:8000

5. API Documentation

Swagger UI:

http://127.0.0.1:8000/docs

API Endpoints
1. Create Organization

POST /org/create
Creates a new organization, inserts metadata into the Master Database, and generates a dedicated collection for the organization.

2. Get Organization

GET /org/get
Retrieves metadata for an existing organization.

3. Update Organization

PUT /org/update
Updates organization details and synchronizes data to a newly generated collection.

4. Delete Organization

DELETE /org/delete
Deletes organization metadata and its dynamic collection. Requires authentication.

5. Admin Login

POST /admin/login
Validates administrator credentials and returns a JWT token containing admin and organization identifiers.

Design Choices
Multi-Tenant Isolation

Each organization is isolated through a dedicated collection, eliminating cross-tenant data conflicts and simplifying schema management.

Master Database for Metadata

Centralized storage for organizational metadata improves lookup performance and simplifies administration.

JWT-Based Authentication

Stateless authentication using signed tokens removes the need for maintaining session state on the server.

Asynchronous I/O Model

FastAPI and Motor allow efficient handling of concurrent requests and database operations.

Future Enhancements

Role-based access control

Refresh token system

Centralized logging and monitoring

Rate limiting for endpoints

Organization listing endpoint

Conversion to class-based service architecture
