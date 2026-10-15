# 🚀 Full Next.js CRUD App with MongoDB

This guide covers the **complete CRUD (Create, Read, Update, Delete) implementation** in a **Next.js full-stack application**, integrating **MongoDB for the database** and **React frontend with Axios** for fetching and managing data.

---

## 📌 **Project Setup**

### 1️⃣ **Clone the Repository**
```bash
git clone https://github.com/yourusername/nextjs-crud-app.git
cd nextjs-crud-app
```

### 2️⃣ **Install Dependencies**
```bash
npm install
```

### 3️⃣ **Setup Environment Variables**
Create a `.env.local` file in the root directory and add:
```env
MONGODB_URI=mongodb+srv://yourusername:yourpassword@cluster.mongodb.net/nextjscrud
```

---

## 🏗️ **Database Connection**

We use **Mongoose** to connect to MongoDB.
📌 File: `utils/db.js`
```javascript
import mongoose from "mongoose";

const MONGODB_URI = process.env.MONGODB_URI;

if (!MONGODB_URI) {
  throw new Error("Please define the MONGODB_URI in your .env file");
}

let cached = global.mongoose || { conn: null, promise: null };

async function connectDB() {
  if (cached.conn) return cached.conn;

  if (!cached.promise) {
    cached.promise = mongoose.connect(MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    }).then((mongoose) => mongoose);
  }
  cached.conn = await cached.promise;
  return cached.conn;
}

export default connectDB;
```

---

## 📁 **Task Schema (Model)**
📌 File: `models/Task.js`
```javascript
import mongoose from "mongoose";

const TaskSchema = new mongoose.Schema(
  {
    title: { type: String, required: true },
    description: { type: String },
  },
  { timestamps: true }
);

export default mongoose.models.Task || mongoose.model("Task", TaskSchema);
```

---

## 🌐 **Backend API Routes (CRUD Operations)**

### **1️⃣ Get All Tasks (READ)**
📌 File: `pages/api/tasks/index.js`
```javascript
import connectDB from "../../../utils/db";
import Task from "../../../models/Task";

export default async function handler(req, res) {
  await connectDB();
  if (req.method === "GET") {
    const tasks = await Task.find();
    return res.status(200).json(tasks);
  }
}
```

### **2️⃣ Create a Task (POST)**
📌 File: `pages/api/tasks/index.js`
```javascript
export default async function handler(req, res) {
  await connectDB();
  if (req.method === "POST") {
    try {
      const { title, description } = req.body;
      const newTask = await Task.create({ title, description });
      return res.status(201).json(newTask);
    } catch (error) {
      return res.status(500).json({ error: "Task creation failed" });
    }
  }
}
```

### **3️⃣ Update a Task (PUT/PATCH)**
📌 File: `pages/api/tasks/[id].js`
```javascript
export default async function handler(req, res) {
  await connectDB();
  const { id } = req.query;
  if (req.method === "PUT" || req.method === "PATCH") {
    try {
      const { title, description } = req.body;
      const updatedTask = await Task.findByIdAndUpdate(
        id,
        { title, description },
        { new: true }
      );
      return res.status(200).json(updatedTask);
    } catch (error) {
      return res.status(500).json({ error: "Update failed" });
    }
  }
}
```

---

## 🎨 **Frontend Implementation (CRUD with Axios & useState)**
📌 File: `pages/index.js`
```javascript
import { useState, useEffect } from "react";
import axios from "axios";

export default function Home() {
  const [tasks, setTasks] = useState([]);
  const [title, setTitle] = useState("");
  const [editTask, setEditTask] = useState(null);

  useEffect(() => {
    fetchTasks();
  }, []);

  const fetchTasks = async () => {
    const { data } = await axios.get("/api/tasks");
    setTasks(data);
  };

  const addTask = async () => {
    await axios.post("/api/tasks", { title });
    setTitle("");
    fetchTasks();
  };

  const updateTask = async (id) => {
    await axios.patch(`/api/tasks/${id}`, { title });
    setEditTask(null);
    fetchTasks();
  };

  const deleteTask = async (id) => {
    await axios.delete(`/api/tasks/${id}`);
    fetchTasks();
  };

  return (
    <div style={{ textAlign: "center", padding: "20px" }}>
      <h1>Task Manager</h1>
      <input
        type="text"
        placeholder="New Task"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <button onClick={addTask}>Add</button>
      <ul>
        {tasks.map((task) => (
          <li key={task._id}>
            {task.title}
            <button onClick={() => setEditTask(task._id)}>Edit</button>
            <button onClick={() => updateTask(task._id)}>Save</button>
            <button onClick={() => deleteTask(task._id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 📂 **Project Structure**
```
nextjs-crud-app/
│── models/
│   ├── Task.js
│── pages/
│   ├── api/
│   │   ├── tasks/
│   │   │   ├── index.js
│   │   │   ├── [id].js
│   ├── index.js
│── utils/
│   ├── db.js
│── .env.local
│── package.json
│── README.md
```

---

 **Author:** *Abdur Rashid Sawom* 

