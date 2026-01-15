
# Exam (Backend)

**Date:** Jan 15, 2026  
**Topic:** Node.js + Express + MongoDB (Mongoose) + MVC Pattern + Postman + Gemini API endpoint  

## Rules
- **No vibe coding** (no blind copy/paste). Every student is expected to **review and understand** every line they submit.
- You may use your previous session repos.
- If you use AI for help, you must be able to explain your code.
- The exam session **will be recorded** in your laptop so **please remove any personal** items from Desktop

## Goal
Build a REST API using **Express + Mongoose** following the **MVC pattern** exactly as we practiced in class.

You will implement:
1) A **User** model + login/signup/getme endpoints
2) A **Car** model + CRUD endpoints
3) An **AI endpoint** that calls **Gemini** and accepts/returns **JSON in a structured format** (same idea as our in-class demo)
4) Full Postman testing + export results to JSON

---

## Project Setup Requirements

### Must use
- Node.js + Express
- MongoDB + Mongoose
- `dotenv`
- `morgan`


---

## Required Folder Structure (MVC)
Your structure must match the **same approach we used in class** (routes → controller → model; plus server + config).

Example (you may add other folders if you need):

```
config/
	db.js
models/
	User.js
	Car.js
controllers/
	userController.js
	carController.js
	aiController.js
routes/
	userRoutes.js
	carRoutes.js
	aiRoutes.js
middlewares/
	errorHandler.js
	notFound.js
app.js
server.js
.env
package.json
```

### Server requirements
- Must start with: `npm run dev`
- Must run on: `http://localhost:4003`
- Must connect to MongoDB using `MONGODB_URI` in `.env`

---

## Models (Mongoose)

### 1) User Model (`User`)
Create a `User` schema with at least:
- `name`: String, required, minimum length 2  
- `email`: String, required, unique, lowercase, trimmed  
- `age`: Number, required, minimum 16  
- `role`: String, enum: `"student" | "admin"`, default `"student"`  
- `password`: String, required  
- `companyName`: String, required  
- `companyAddress`: String, required  
- `githubUsername`: String, required  
- `phoneNumber`: String, required  
- `jobTitle`: String  
- timestamps enabled  
- versionKey disabled  
- validation required with meaningful error messages 

Validation is required. Return meaningful errors.

### 2) Car Model (`Car`)
Create a `Car` schema with at least:
- `brand`: String, required  
- `model`: String, required  
- `make`: String, required  
- `vin`: String, required  
- `manufacturer`: String, required  
- `year`: Number, required, minimum 1990, maximum current year + 1  
- `price`: Number, required, minimum 0  
- `type`: String, required (e.g., Sedan, SUV)  
- `availability.isAvailable`: Boolean, required  
- `availability.dueDate`: Date  
- `availability.renter`: String  
- `user_id`: ObjectId reference to `User`, required  
- `owner`: ObjectId reference to `User` 
- timestamps enabled  
- validation required with meaningful errors  

---

## Routers (2 main routers)
You must have exactly **two routers**:
1) **Users router** (`/api/users`)
2) **Cars router** (`/api/cars`)

The **Gemini endpoint** must be added as an extra endpoint inside **one of these two routers** (example: `POST /api/users/ai`).

---

## Auth Requirements (for protected routes)
Protected routes must use the **same JWT approach we used in class**:
- Passwords must be hashed using `bcrypt` (never store plain text passwords)
- Login/Signup returns a JWT token
- `GET /api/users/getme` (and protected car routes) requires:
	- header: `Authorization: Bearer <token>`
- Token payload should include at least the user id
- API responses must never return the password

Required `.env` variables:
- `JWT_SECRET=`
- `JWT_EXPIRES_IN=` (optional)

---

## Required Endpoints

### Users Endpoints (`/api/users`)
Implement all endpoints below:

1) `POST /api/users/login`: not protected

2) `POST /api/users/signup`: not protected

3) `GET /api/users/getme`:  protected route


### Cars Endpoints (`/api/cars`)
Implement all endpoints below:

1) `POST /api/cars`: protected route
- Creates a car

2) `GET /api/cars`: not protected
- Returns list of cars
- Add optional query support (choose at least one):
	- `?brand=Toyota`
	- `?minYear=2015`
	- `?available=true`

3) `GET /api/cars/:id`: not protected
- Returns one car

4) `PATCH /api/cars/:id`: protected route
- Updates allowed fields (brand, model, year, price, isAvailable, owner)

5) `DELETE /api/cars/:id`: protected route
- Deletes the car

---

## AI Requirement (Gemini Endpoint)

Add **one endpoint** that calls **Gemini** using the **same pattern as the in-class demo**.

### Endpoint spec
- Method: `POST`
- Path (choose one):
	- `/api/users/ai` OR `/api/cars/ai` OR `/api/ai/gemini`

### Input (JSON request)
Your endpoint must accept a structured JSON body like:

```json
{
	"task": "summarize" ,
	"data": {
		"text": "...",
		"language": "en"
	}
}
```

You may define your own structure, but it must be:
- JSON only
- validated (reject missing fields with `400`)
- consistent (same keys always)

### Output (JSON response)
Must return a structured JSON response, for example:

```json
{
	"success": true,
	"input": { "task": "summarize" },
	"result": {
		"text": "...",
		"meta": { "model": "gemini-..." }
	}
}
```

### Gemini requirements
- API key must come from `.env` (example: `GEMINI_API_KEY`)
- Do not hardcode keys
- Must handle Gemini errors and return:
	- `502` for upstream/AI failure
	- readable message + safe error details

---

## Error Handling (Required)
Implement:
- `notFound` middleware (returns `404` JSON)
- centralized `errorHandler` middleware
- consistent error response format, e.g.:

```json
{
	"success": false,
	"error": {
		"message": "Validation failed",
		"details": []
	}
}
```

---

## Postman Requirement
You must:
1) Test **every endpoint** (Users + Cars + AI)
2) Export:
	 - The Postman **collection** as JSON
	 - The Postman **environment** as JSON

Name them clearly, for example:
- `backend-exam-collection.json`
- `backend-exam-environment.json`

Place them in a folder in your repo:
```
postman/
	backend-exam-collection.json
	backend-exam-environment.json
```

---

## Submission
Submit a GitHub repository link containing:
- Working code with the required structure
- `.env.example` (NOT your real `.env`) containing:
	- `MONGODB_URI=`
	- `PORT=4003`
	- `JWT_SECRET=mock-secret-not-real`
	- `JWT_EXPIRES_IN=1d`
	- `GEMINI_API_KEY=mock-key-not-real-key-here`
- `README.md` that explains:
	- how to install (`npm i`)
	- how to run (`npm run dev`)
	- endpoints list
	- how to import Postman collection/environment

---

## Grading
- MVC structure + clean routing/controller/model separation (20%)
- User endpoints + validation + status codes (10%)
- Car endpoints + validation + relations (owner) (10%)
- Gemini endpoint (JSON in/out + error handling + env key) (10%)
- Postman testing + correct JSON export (10%)
- Code quality + readability (10%)
- students can explain their code (30%)

---

## Notes
- Use correct HTTP status codes: `201`, `200`, `400`, `404`, `500`, `502`.
- Keep responses in JSON only.
- Keep controllers thin: no DB logic inside routes.

