# User Account Management

This project is backend built in nest js framework for user sign-in/sign-up and features like **email verification**, **forgotten password**, **reset password**, **update profile** and **settings**.

# setting up environment
Install `nodejs` and `mongodb` in your machine.

Install dependencies with npm and run the application:
``` 
npm install
npm run start
```

# Deploy using Docker
⚠️ Before deploy the app in a container set the right **configuration** as explained in the section below, and then you can run:
``` 
docker-compose up -d
```
# API
Server will listen on port `3000`, and it expose the following APIs:


- **POST** - `/auth/email/register` - Register a new user
  - **email** - *string*
  - **password** - *string*
  - **name** - *string (optional)*
  - **surname** - *string (optional)*

- **POST** - `/auth/email/login` - Login user
  - **email** - *string*
  - **password** - *string*

- **GET** - `/auth/email/verify/:token` - Validates the token sent in the email and activates the user's account

- **GET** - `/auth/email/resend-verification/:email` - Resend verification email

- **GET** - `/auth/email/forgot-password/:email` - Send a token via email to reset the password 

- **POST** - `/auth/email/reset-password` - Change user password
  - **newPassword** - *string*
  - **newPasswordToken** - *string (token received by forgot-password api)*

- **GET** - `/auth/users` - Returns all users (must be logged in)

- **GET** - `/users/user/:email` - Returns selected user info (must be logged in)

- **POST** - `/users/profile/update` - Update user info
  - **name** - *string*
  - **surname** - *string*
  - **phone** - *string*
  - **email** - *string*
  - **birthdaydate** - *Date*
  - **profilepicture** - *string (base64)*

- **POST** - `/users/gallery/update` -  Add/Remove user photos
  - **email** - *string*
  - **action** - *string ('add' or 'remove')*
  - **newPhoto** - *object* (only for case 'add')
    - **imageData** - *string (base64)*
    - **description** - *string*
  - **photoId** - *string (base64)* (only for case 'remove')

- **POST** - `settings/update` - Update user settings
  - **email** - *string*
  - **settingsKey1** - *string (Value1)*
  - **settingsKey2** - *string (Value2)*
  - **...**
  

# Passport JWT strategy
This project use JSON Web Token ([JWT](https://www.npmjs.com/package/passport-jwt)) Bearer Token as authentication strategy for Passport. 
The login API returns an access_token that you have to use to send a correct authorization header in calls that require authentication. 

Authorization header example:
```
 Authorization → Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...._DkYJJh4s
```

# Security
  - In the main.ts you can set a limit of requests in a time window (default is 100 requests in 15 minutes for all endpoints, and 3 requests in a 1 hour for sign up endpoint)
