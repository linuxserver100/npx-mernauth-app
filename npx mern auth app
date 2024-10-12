Creating a full-featured user authentication system with a React frontend, a Node.js backend, and MongoDB involves several steps. Below is a high-level overview and basic code snippets to help you get started.

### Step 1: Set Up Your React App

1. **Create a React App:**
   ```bash
   npx create-react-app user-auth
   cd user-auth
   ```

2. **Install Required Packages:**
   ```bash
   npm install axios react-router-dom
   ```

### Step 2: Set Up the Node.js Backend

1. **Create a Backend Directory:**
   ```bash
   mkdir backend
   cd backend
   npm init -y
   ```

2. **Install Required Packages:**
   ```bash
   npm install express mongoose bcryptjs jsonwebtoken nodemailer dotenv cors
   ```

3. **Create Basic Server Structure:**
   - Create a file named `server.js`:
     ```javascript
     const express = require('express');
     const mongoose = require('mongoose');
     const cors = require('cors');
     require('dotenv').config();

     const app = express();
     const PORT = process.env.PORT || 5000;

     app.use(cors());
     app.use(express.json());

     mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
         .then(() => console.log('MongoDB connected'))
         .catch(err => console.log(err));

     app.listen(PORT, () => {
         console.log(`Server running on port ${PORT}`);
     });
     ```

### Step 3: Define User Model

1. **Create a `models` directory and a User model:**
   - `models/User.js`:
     ```javascript
     const mongoose = require('mongoose');

     const UserSchema = new mongoose.Schema({
         username: { type: String, required: true, unique: true },
         email: { type: String, required: true, unique: true },
         password: { type: String, required: true },
         isVerified: { type: Boolean, default: false },
     });

     module.exports = mongoose.model('User', UserSchema);
     ```

### Step 4: Create Authentication Routes

1. **Set Up Routes in `routes/auth.js`:**
   ```javascript
   const express = require('express');
   const bcrypt = require('bcryptjs');
   const jwt = require('jsonwebtoken');
   const nodemailer = require('nodemailer');
   const User = require('../models/User');

   const router = express.Router();

   // Register
   router.post('/register', async (req, res) => {
       const { username, email, password } = req.body;
       const hashedPassword = await bcrypt.hash(password, 10);

       const newUser = new User({ username, email, password: hashedPassword });
       await newUser.save();
       
       // Send verification email logic here

       res.status(201).json({ message: 'User registered' });
   });

   // Login
   router.post('/login', async (req, res) => {
       const { email, password } = req.body;
       const user = await User.findOne({ email });

       if (!user || !(await bcrypt.compare(password, user.password))) {
           return res.status(401).json({ message: 'Invalid credentials' });
       }

       const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET, { expiresIn: '1h' });
       res.json({ token });
   });

   // Forgot Password
   router.post('/forgot-password', async (req, res) => {
       const { email } = req.body;
       // Send password reset link logic here
       res.json({ message: 'Password reset link sent' });
   });

   module.exports = router;
   ```

2. **Import Routes in `server.js`:**
   ```javascript
   const authRoutes = require('./routes/auth');
   app.use('/api/auth', authRoutes);
   ```

### Step 5: Frontend Implementation

1. **Set Up Routing:**
   - In your React app, set up routing in `src/App.js`:
     ```javascript
     import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
     import Login from './components/Login';
     import Signup from './components/Signup';
     import ForgotPassword from './components/ForgotPassword';

     function App() {
         return (
             <Router>
                 <Switch>
                     <Route path="/login" component={Login} />
                     <Route path="/signup" component={Signup} />
                     <Route path="/forgot-password" component={ForgotPassword} />
                 </Switch>
             </Router>
         );
     }

     export default App;
     ```

2. **Create Form Components:**
   - For each component (Login, Signup, ForgotPassword), implement forms and use Axios to make API calls to the backend.

### Step 6: Environment Variables

1. **Create a `.env` File:**
   - In the `backend` directory, create a `.env` file:
     ```
     MONGO_URI=your_mongodb_connection_string
     JWT_SECRET=your_jwt_secret
     ```

### Step 7: Run the Application

1. **Start the Backend Server:**
   ```bash
   node server.js
   ```

2. **Start the React App:**
   ```bash
   npm start
   ```

### Step 8: Implement Email Verification

You can use a service like SendGrid or Nodemailer to implement email verification after user registration.

### Step 9: Additional Enhancements

- Implement password reset functionality.
- Add front-end validation for forms.
- Use context or Redux for managing authentication state.

This setup provides a solid foundation for a user authentication system with React and MongoDB. You can expand upon it by adding features such as user profiles, password reset tokens, etc.
