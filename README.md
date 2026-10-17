
## DESTINY 

Destiny is a MERN stack web application designed to help people connect and collaborate. Users can create profiles, explore other people, send connection requests, and manage their matches.

Live demo: https://destiny-gauri.duckdns.org 
Demo login: demo@example.com / <password> (create a test account and put it here)

## 🛠️ Tech Stack
  **Frontend Framework**:React.js Backend Framework: Node.js + Express.js
  **Database**: MongoDB + Mongoose 
  **Authentication**: JWT (JSON Web Tokens) + Cookies
  **Encryption**: bcryptjs for password hashing 
  **API Testing**: Postman 
  **Environment Variables Management**: dotenv 
  **Package Manager**: npm

---

## 🔑 Features Implemented

### **1. Authentication System**
✅ User Signup, Login, and Logout  
✅ JWT-based authentication with secure cookies  
✅ Password encryption using **bcryptjs**  
✅ Authentication middleware to protect routes  

### **2. User Profile Management**
✅ View user profile  
✅ Edit profile details (restricted fields for security)  
✅ Update password with validation  

### **3. Connection Request System**
✅ Send connection requests (`Interested` or `Ignored`)  
✅ Accept or reject received requests  
✅ Prevent duplicate requests using MongoDB validation  
✅ Prevent self-requests using Mongoose `.pre` middleware  

### **4. Feed API & Pagination**
✅ Fetch suggested developers while excluding:  
   - Logged-in user  
   - Existing connections  
   - Ignored users  
   - Users with pending requests  
✅ Implemented **pagination** using `skip` & `limit`  
✅ Optimized query using **MongoDB $nin and $ne operators**  

### **5. Database Design**
✅ **User Schema**:
   - Sanitized input fields (`trim`, `lowercase`, validation)
   - Unique constraints on email and username  

✅ **ConnectionRequest Schema**:
   - `fromUserId`, `toUserId`, `status` with **enum validation**
   - Indexed fields for optimized queries  
   - Prevents multiple requests between the same users  

### **6. Advanced Query Optimization**
✅ **Indexes & Compound Indexes**:
   - Used `index: true` for faster queries  
   - Implemented compound indexes to optimize search  

### **7. Middleware Implementation**
✅ **Authentication Middleware**: Protects private routes  
✅ **Error Handling Middleware**: Centralized error response  
✅ **Mongoose `.pre` Middleware**: Prevents self-requests  

### **8. Express Router Structure**
✅ Modular route organization for maintainability  
✅ APIs structured into separate routers (`auth`, `profile`, `connections`, `users`)  

---

## 🚀 API Endpoints

### **1️⃣ Authentication Routes**
| Method | Endpoint      | Description          | Auth Required |
|--------|--------------|----------------------|--------------|
| PATCH   | `/auth/signup` | Register a new user | ❌ |
| PATCH  | `/auth/login` | Authenticate user & issue JWT | ❌ |
| PATCH   | `/auth/logout` | Logout user by clearing JWT cookie | ✅ |

---

### **2️⃣ User Profile Routes**
| Method | Endpoint           | Description              | Auth Required |
|--------|-------------------|------------------------|--------------|
| GET    | `/profile/view`   | Get logged-in user profile | ✅ |
| PATCH  | `/profile/edit`   | Update allowed profile fields | ✅ |
| PATCH  | `/profile/password` | Update user password | ✅ |

---

### **3️⃣ Connection Request Routes**
| Method | Endpoint                                    | Description                 | Auth Required |
|--------|--------------------------------------------|-----------------------------|--------------|
| PATCH   | `/request/send/:status/:toUserId`         | Send a connection request (Interested/Ignored) | ✅ |
| PATCH   | `/request/review/:status/:requestId`      | Accept/Reject a request | ✅ |
| GET    | `/user/requests/received`                 | Fetch pending connection requests | ✅ |
| GET    | `/user/connections`                       | Fetch accepted connections | ✅ |

---

### **4️⃣ Feed API & Pagination**
| Method | Endpoint      | Description                              | Auth Required |
|--------|--------------|----------------------------------------|--------------|
| GET    | `/user/feed?page=1&limit=10` | Get suggested developer profiles with pagination | ✅ |

---

### **Server**
 1️⃣ Clone the Repository git clone
 https://github.com/Gauri1799/destiny.git 
 cd Destiny
 2️⃣ Set Up Environment Variables

Create a backend/.env file and add:

DATABASE_URL=mongodb+srv://:@cluster0.mongodb.net/tinderdb 
JWT_SECRET=your_jwt_secret 
PORT=3000 
CLIENT_URL=http://localhost:5173

Create a frontend/.env.development file and add: VITE_API_URL=http://localhost:3000

3️⃣ Start the Backend Server
 npm start 
 Server runs at: http://localhost:3000/

## CI/CD pipeline

Pull request → lint (frontend + backend) Push to main → lint → build Docker images → push to Docker Hub → SSH deploy to EC2

ESLint runs on both projects. A lint failure blocks the build and deploy.
Images are tagged with latest and the commit SHA, so any release can be rolled back.
Secrets live in GitHub Secrets. The server-side .env is generated at deploy time.

## Deployment

**Required GitHub secrets:** DOCKER_HUB_TOKEN, EC2_HOST, EC2_USER, SSH_PRIVATE_KEY, MONGO_URI, JWT_SECRET.

One-time EC2 setup: install Docker, open ports 22/80/443, allow the EC2 IP in MongoDB Atlas, point the domain to the Elastic IP. After that, every push to main deploys automatically.

## 📌 Future Enhancements
🔹 Real-time notifications using WebSockets  
🔹 Messaging System for better user interaction  
🔹 Profile Search & Filtering