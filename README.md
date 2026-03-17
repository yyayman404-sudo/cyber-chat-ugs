# Cyber Chat UGS

## Project Overview
Cyber Chat UGS is a user-friendly chat application designed to facilitate seamless communication among users. This project is built using modern web technologies, providing a fast and responsive experience.

## Features
- **Real-time messaging**: Send and receive messages instantly.
- **User authentication**: Secure login and registration system.
- **Group chats**: Create groups for specific discussions.
- **Media sharing**: Send images, videos, and files effortlessly.

## Installation
### Prerequisites
- Node.js (v14 or later)
- MongoDB

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/yyayman404-sudo/cyber-chat-ugs.git
   cd cyber-chat-ugs
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the application:
   ```bash
   npm start
   ```

## Usage
- Navigate to `http://localhost:3000` in your browser to access the chat application.
- Create an account or log in to start chatting.

## API Documentation
### Authentication
- **POST** `/api/auth/register`
  - Description: Register a new user
  - Body: `{ "username": String, "password": String }`

- **POST** `/api/auth/login`
  - Description: Log in an existing user
  - Body: `{ "username": String, "password": String }`

### Messaging
- **GET** `/api/messages`
  - Description: Retrieve all messages

- **POST** `/api/messages`
  - Description: Send a new message
  - Body: `{ "content": String }`

## Contributing Guidelines
We welcome contributions! To contribute:
1. Fork the repo.
2. Create a new branch: `git checkout -b feature/YourFeature`
3. Make your changes and commit them: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin feature/YourFeature`
5. Open a Pull Request.

## License
This project is licensed under the MIT License.