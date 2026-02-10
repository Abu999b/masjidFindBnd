# Masjid Finder - Backend API

Backend API for the Masjid Finder application. Built with Node.js, Express, and MongoDB.

## 🚀 Features

- RESTful API architecture
- JWT-based authentication
- Role-based access control (User, Admin, Main Admin)
- Geospatial queries for nearby masjids
- MongoDB with Mongoose ODM
- CORS enabled for cross-origin requests

## 📋 Prerequisites

- Node.js (v14 or higher)
- MongoDB (local or Atlas)
- npm or yarn

## 🛠️ Installation

1. **Clone the repository**
```bashgit clone <repository-url>
cd backend

2. **Install dependencies**
```bashnpm install

3. **Create environment file**
   
   Create a `.env` file in the backend root directory:
```envPORT=5000
MONGODB_URI=mongodb://localhost:27017/masjid-finder
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
NODE_ENV=development

4. **Start MongoDB**
   
   Local MongoDB:
```bashmongod
   
   Or use MongoDB Atlas (cloud):
   - Create account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Create a cluster
   - Get connection string
   - Update `MONGODB_URI` in `.env`

5. **Run the server**
   
   Development mode (with auto-restart):
```bashnpm run dev
   
   Production mode:
```bashnpm start

## 📁 Project Structurebackend/
├── config/
│   └── db.js                 # Database configuration
├── controllers/
│   ├── authController.js     # Authentication logic
│   └── masjidController.js   # Masjid CRUD operations
├── middleware/
│   └── auth.js               # JWT verification & authorization
├── models/
│   ├── User.js               # User schema
│   └── Masjid.js             # Masjid schema
├── routes/
│   ├── authRoutes.js         # Auth endpoints
│   └── masjidRoutes.js       # Masjid endpoints
├── .env                      # Environment variables (DO NOT COMMIT)
├── .gitignore
├── package.json
├── server.js                 # Application entry point
└── README.md

## 🔌 API Endpoints

### Base URLLocal: http://localhost:5000/api
Production: https://your-domain.onrender.com/api

### Authentication Routes

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/auth/register` | Register new user | No |
| POST | `/auth/login` | Login user | No |
| GET | `/auth/me` | Get current user | Yes |
| GET | `/auth/users` | Get all users | Main Admin |
| PUT | `/auth/users/:id/role` | Update user role | Main Admin |

### Masjid Routes

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/masjids` | Get all masjids | No |
| GET | `/masjids/nearby` | Get nearby masjids | No |
| GET | `/masjids/:id` | Get single masjid | No |
| POST | `/masjids` | Create masjid | Admin |
| PUT | `/masjids/:id` | Update masjid | Admin |
| DELETE | `/masjids/:id` | Delete masjid | Admin |

## 📝 API Usage Examples

### Register User
```bashPOST /api/auth/register
Content-Type: application/json{
"name": "John Doe",
"email": "john@example.com",
"password": "password123"
}

### Login
```bashPOST /api/auth/login
Content-Type: application/json{
"email": "john@example.com",
"password": "password123"
}

### Get Nearby Masjids
```bashGET /api/masjids/nearby?longitude=78.4867&latitude=17.3850&maxDistance=5000

### Create Masjid (Admin only)
```bashPOST /api/masjids
Authorization: Bearer <your-jwt-token>
Content-Type: application/json{
"name": "Al-Masjid An-Nabawi",
"address": "123 Main Street, City",
"latitude": 17.3850,
"longitude": 78.4867,
"prayerTimes": {
"fajr": "05:30",
"dhuhr": "12:30",
"asr": "16:00",
"maghrib": "18:30",
"isha": "20:00",
"jummah": "13:00"
},
"description": "Beautiful mosque in the city center",
"phoneNumber": "+1234567890"
}

## 🔐 Authentication

This API uses JWT (JSON Web Tokens) for authentication.

1. Register or login to receive a token
2. Include token in Authorization header:Authorization: Bearer <your-token>

## 👥 User Roles

- **User**: Default role, can view masjids
- **Admin**: Can create, update, and delete masjids
- **Main Admin**: First registered user, can manage other admins

## 🌍 Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `PORT` | Server port | `5000` |
| `MONGODB_URI` | MongoDB connection string | `mongodb://localhost:27017/masjid-finder` |
| `JWT_SECRET` | Secret key for JWT | `your_secret_key_min_32_characters` |
| `NODE_ENV` | Environment mode | `development` or `production` |

## 🚀 Deployment

### Deploy to Render

1. **Push code to GitHub**
```bashgit init
git add .
git commit -m "Initial commit"
git remote add origin <your-repo-url>
git push -u origin main

2. **Create Web Service on Render**
   - Go to [Render Dashboard](https://dashboard.render.com/)
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Configure:
     - Name: `masjid-finder-api`
     - Environment: `Node`
     - Build Command: `npm install`
     - Start Command: `npm start`
     - Plan: Free

3. **Set Environment Variables**
   - `NODE_ENV` = `production`
   - `MONGODB_URI` = Your MongoDB Atlas connection string
   - `JWT_SECRET` = Your secure secret key
   - `PORT` = `5000` (auto-set by Render)

4. **Deploy**
   - Click "Create Web Service"
   - Wait for deployment to complete

### MongoDB Atlas Setup

1. Create account at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster
3. Database Access → Create database user
4. Network Access → Add IP → Allow from anywhere (`0.0.0.0/0`)
5. Clusters → Connect → Get connection string
6. Replace `<password>` with your database password
7. Use as `MONGODB_URI` in Render

## 🧪 Testing

Test API endpoints using:

- **Postman**: Import collection or test manually
- **cURL**: Command line testing
- **Browser**: For GET requests

Example cURL:
```bashHealth check
curl https://your-api.onrender.com/api/healthGet all masjids
curl https://your-api.onrender.com/api/masjidsRegister user
curl -X POST https://your-api.onrender.com/api/auth/register 
-H "Content-Type: application/json" 
-d '{"name":"Test","email":"test@example.com","password":"test123"}'

## 🐛 Troubleshooting

### MongoDB Connection Error
- Check if MongoDB is running
- Verify `MONGODB_URI` is correct
- For Atlas: Check network access settings

### CORS Error
- Verify frontend URL is in CORS whitelist
- Check `server.js` CORS configuration

### Authentication Failed
- Verify JWT token is valid
- Check if token is included in Authorization header
- Ensure JWT_SECRET matches between environments

## 📚 Dependencies
```json{
"express": "^4.18.2",
"mongoose": "^7.5.0",
"bcryptjs": "^2.4.3",
"jsonwebtoken": "^9.0.2",
"dotenv": "^16.3.1",
"cors": "^2.8.5",
"express-validator": "^7.0.1"
}

## 📄 License

MIT License - feel free to use this project for learning and development.

## 👨‍💻 Author

Your Name - [Your GitHub](https://github.com/yourusername)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request
