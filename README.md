npx create-react-app chatgpt-app
cd chatgpt-app
npm install react-router-dom axios
import React from "react";
import { BrowserRouter as Router, Routes, Route, Navigate } from "react-router-dom";
import Login from "./components/Login";
import ChatPage from "./components/ChatPage";

function App() {
  const [isLoggedIn, setIsLoggedIn] = React.useState(false);

  return (
    <Router>
      <Routes>
        <Route
          path="/"
          element={isLoggedIn ? <Navigate to="/chat" /> : <Login setIsLoggedIn={setIsLoggedIn} />}
        />
        <Route
          path="/chat"
          element={isLoggedIn ? <ChatPage setIsLoggedIn={setIsLoggedIn} /> : <Navigate to="/" />}
        />
      </Routes>
    </Router>
  );
}

export default App;
import React, { useState } from "react";

function Login({ setIsLoggedIn }) {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    // Call API for authentication
    if (username === "admin" && password === "1234") { // Simple mock validation
      setIsLoggedIn(true);
    } else {
      alert("Invalid credentials!");
    }
  };

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>ChatGPT Login</h1>
      <form onSubmit={handleSubmit}>
        <div>
          <input
            type="text"
            placeholder="Username"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            required
          />
        </div>
        <div>
          <input
            type="password"
            placeholder="Password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            required
          />
        </div>
        <button type="submit">Login</button>
      </form>
    </div>
  );
}

export default Login;
[5:03 PM, 12/3/2024] Samiullah Canva Wala: import React, { useState } from "react";

function Login({ setIsLoggedIn }) {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    // Call API for authentication
    if (username === "admin" && password === "1234") { // Simple mock validation
      setIsLoggedIn(true);
    } else {
      alert("Invalid credentials!");
    }
  };

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>ChatGPT Login</h1>
      <form onSubmit={handleSubmit}>
        <div>
          <input
            type="text"
            placeholder="Username"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            required
          />
        </div>
        <div>
          <input
            type="password"
            placeholder="Password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            required
          />
        </div>
        <button type="submit">Login</button>
      </form>
    </div>
  );
}

export default Login;
[5:03 PM, 12/3/2024] Samiullah Canva Wala: import React, { useState } from "react";
import axios from "axios";

function ChatPage({ setIsLoggedIn }) {
  const [input, setInput] = useState("");
  const [messages, setMessages] = useState([]);

  const handleSend = async () => {
    if (input.trim() === "") return;

    const userMessage = { sender: "user", text: input };
    setMessages((prev) => [...prev, userMessage]);

    try {
      const response = await axios.post("http://localhost:5000/chat", { message: input });
      setMessages((prev) => [...prev, { sender: "bot", text: response.data.reply }]);
    } catch (error) {
      setMessages((prev) => [...prev, { sender: "bot", text: "Error connecting to server!" }]);
    }

    setInput("");
  };

  return (
    <div style={{ padding: "20px" }}>
      <h2>ChatGPT</h2>
      <button onClick={() => setIsLoggedIn(false)} style={{ float: "right" }}>Logout</button>
      <div style={{ border: "1px solid #ccc", padding: "10px", height: "300px", overflowY: "scroll" }}>
        {messages.map((msg, index) => (
          <div key={index} style={{ textAlign: msg.sender === "user" ? "right" : "left" }}>
            <p><strong>{msg.sender === "user" ? "You" : "Bot"}:</strong> {msg.text}</p>
          </div>
        ))}
      </div>
      <div style={{ marginTop: "10px" }}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Type your message..."
        />
        <button onClick={handleSend}>Send</button>
      </div>
    </div>
  );
}

export default ChatPage;
mkdir backend
cd backend
npm init -y
npm install express body-parser cors
const express = require("express");
const bodyParser = require("body-parser");
const cors = require("cors");

const app = express();
app.use(bodyParser.json());
app.use(cors());

app.post("/chat", (req, res) => {
  const userMessage = req.body.message;
  // Mock reply logic
  const botReply = You said: "${userMessage}";
  res.json({ reply: botReply });
});

const PORT = 5000;
app.listen(PORT, () => {
  console.log(Server running on http://localhost:${PORT});
});
node server.js
npm start
<img src="/logo.png" alt="ChatGPT Logo" style={{ width: "150px" }} />
