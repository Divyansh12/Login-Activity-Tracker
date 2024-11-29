# User Login Tracker System

A robust user authentication system with comprehensive login tracking capabilities built with Node.js, Express, and Sequelize ORM. This system provides enterprise-level session management with the ability to track and control multiple concurrent user sessions across different devices.

## About

This system is designed to provide a complete authentication solution with advanced session tracking capabilities. It's particularly useful for applications requiring:

- High security authentication
- Multiple device login support
- Detailed audit trails of user sessions
- Remote session management
- Protection against unauthorized access and token theft

The system uses modern security practices including JWT-based authentication, secure password hashing, and token blacklisting to ensure secure user sessions.

## Features

### Login Tracking
- Tracks all user login sessions with detailed information including:
  - IP address
  - Device information
  - Login timestamp
  - Logout timestamp
  - Token information
  - Session status (active/logged out)

### Session Management
- Multiple concurrent login sessions support
- Ability to view all active sessions
- Options to terminate sessions:
  - Terminate single session
  - Terminate all sessions except current
  - Terminate all sessions including current

### Security Features
- JWT (JSON Web Token) based authentication
- Token blacklisting mechanism
- Automatic session invalidation
- Secure password hashing using bcrypt
- Protection against unauthorized token reuse

## API Documentation

### Authentication Endpoints

#### Register User
- **POST** `/registerUser`
- Register a new user account
- **Body Parameters:**
  - first_name (string)
  - last_name (string) 
  - username (string, required)
  - email (string, required)
  - password (string, required)
- **Responses:**
  - 200: User created successfully
  - 403: Username or email already taken

#### Login User
- **POST** `/loginUser`
- Authenticate and login a user
- **Body Parameters:**
  - username (string, required)
  - password (string, required)
- **Responses:**
  - 200: User logged in successfully
  - 401: Bad username/not found
  - 403: Incorrect password

#### Logout User
- **DELETE** `/logout`
- Logout user and blacklist current token
- **Responses:**
  - 200: User logged out successfully

### User Management

#### Find User
- **GET** `/findUser`
- Get user details
- **Query Parameters:**
  - username (string, required)
- **Requires Authentication:** Yes
- **Responses:**
  - 200: Returns user details
  - 401: User not found
  - 403: JWT token mismatch

#### Update User
- **PUT** `/updateUser`
- Update user profile information
- **Body Parameters:**
  - username (string, required)
  - first_name (string)
  - last_name (string)
  - email (string)
- **Requires Authentication:** Yes
- **Responses:**
  - 200: User updated successfully
  - 401: User not found
  - 403: Unauthorized

#### Delete User
- **DELETE** `/deleteUser`
- Delete user account
- **Query Parameters:**
  - username (string, required)
- **Requires Authentication:** Yes
- **Responses:**
  - 200: User deleted successfully
  - 403: Authentication error
  - 404: User not found

### Password Management

#### Forgot Password
- **POST** `/forgotPassword`
- Request password reset email
- **Body Parameters:**
  - email (string, required)
- **Responses:**
  - 200: Reset email sent
  - 400: Email required
  - 403: Email not found

#### Reset Password
- **GET** `/reset`
- Validate password reset token
- **Query Parameters:**
  - resetPasswordToken (string, required)
- **Responses:**
  - 200: Reset token valid
  - 403: Invalid/expired token

#### Update Password (Logged In)
- **PUT** `/updatePassword`
- Update password while logged in
- **Body Parameters:**
  - username (string, required)
  - password (string, required)
- **Requires Authentication:** Yes
- **Responses:**
  - 200: Password updated
  - 403: Unauthorized
  - 404: User not found

#### Update Password Via Email
- **PUT** `/updatePasswordViaEmail`
- Update password using reset token
- **Body Parameters:**
  - username (string, required)
  - password (string, required)
  - resetPasswordToken (string, required)
- **Responses:**
  - 200: Password updated
  - 401: User not found
  - 403: Invalid/expired token

### Session Management

#### View Active Sessions
- **GET** `/user/logins/show`
- List all active login sessions
- **Requires Authentication:** Yes
- **Responses:**
  - 200: Returns list of active sessions
  - 400: Bad request

#### Delete Single Session
- **GET** `/user/logins/delete/:login_id`
- Terminate specific login session
- **Path Parameters:**
  - login_id: Session ID to terminate
- **Requires Authentication:** Yes
- **Responses:**
  - 200: Session terminated
  - 400: Bad request

#### Delete All Sessions Except Current
- **GET** `/user/logins/delete/all/not-current`
- Terminate all sessions except current
- **Requires Authentication:** Yes
- **Responses:**
  - 200: Sessions terminated
  - 400: Bad request

#### Delete All Sessions
- **GET** `/user/logins/deletes/all`
- Terminate all sessions including current
- **Requires Authentication:** Yes
- **Responses:**
  - 200: All sessions terminated
  - 400: Bad request
