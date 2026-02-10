OVERALL FLOW 
Login
  ↓ (valid user)
Dashboard / Books Page
  ↓
Add / Edit / Delete / Select Books

1. CREATE PROJECT
npm create vite@latest book-app
cd book-app
npm install
npm run dev

2. INSTALL REQUIRED PACKAGES
npm install react-router-dom bootstrap

3. PROJECT STRUCTURE (FOLLOW EXACTLY)
src/
│── components/
│   ├── Header.jsx
│   ├── Footer.jsx
│
│── pages/
│   ├── Login.jsx
│   ├── Books.jsx
│
│── data/
│   └── books.js
│
│── App.jsx
│── main.jsx


4. BOOTSTRAP SETUP
main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

import "bootstrap/dist/css/bootstrap.min.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

5.FIXED USERS (LOGIN CHECK)
We’ll hardcode users (for now):
username: admin
password: admin123

6 LOGIN PAGE
pages/Login.jsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";

function Login() {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");
  const navigate = useNavigate();

  function handleLogin(e) {
    e.preventDefault();

    if (username === "admin" && password === "admin123") {
      localStorage.setItem("loggedIn", "true");
      navigate("/books");
    } else {
      setError("Invalid Credentials");
    }
  }

  return (
    <div className="container mt-5 col-md-4">
      <h3 className="text-center">Login</h3>

      {error && <p className="text-danger">{error}</p>}

      <form onSubmit={handleLogin}>
        <input
          className="form-control mb-2"
          placeholder="Username"
          onChange={(e) => setUsername(e.target.value)}
        />

        <input
          type="password"
          className="form-control mb-3"
          placeholder="Password"
          onChange={(e) => setPassword(e.target.value)}
        />

        <button className="btn btn-primary w-100">
          Login
        </button>
      </form>
    </div>
  );
}

export default Login;

7 PROTECTED ROUTE CHECK
App.jsx
import { BrowserRouter, Routes, Route, Navigate } from "react-router-dom";
import Login from "./pages/Login";
import Books from "./pages/Books";
import Header from "./components/Header";
import Footer from "./components/Footer";

function ProtectedRoute({ children }) {
  const isAuth = localStorage.getItem("loggedIn");
  return isAuth ? children : <Navigate to="/" />;
}

function App() {
  return (
    <BrowserRouter>
      <Header />

      <div className="container mt-4">
        <Routes>
          <Route path="/" element={<Login />} />

          <Route
            path="/books"
            element={
              <ProtectedRoute>
                <Books />
              </ProtectedRoute>
            }
          />
        </Routes>
      </div>

      <Footer />
    </BrowserRouter>
  );
}

export default App;

8. HEADER & FOOTER
components/Header.jsx
function Header() {
  function logout() {
    localStorage.removeItem("loggedIn");
    window.location.href = "/";
  }

  return (
    <nav className="navbar navbar-dark bg-dark px-3">
      <span className="navbar-brand">Book App</span>
      <button className="btn btn-danger" onClick={logout}>
        Logout
      </button>
    </nav>
  );
}

export default Header;
components/Footer.jsx
function Footer() {
  return (
    <footer className="bg-dark text-white text-center p-2 mt-4">
      © 2026 Book Management
    </footer>
  );
}

export default Footer;

9.  BOOK DATA
data/books.js
export const initialBooks = [
  { id: 1, name: "React Basics", price: 500, publisher: "O'Reilly" },
  { id: 2, name: "JavaScript Guide", price: 650, publisher: "Packt" },
  { id: 3, name: "Node.js", price: 700, publisher: "Pearson" },
  { id: 4, name: "HTML & CSS", price: 400, publisher: "Wiley" },
  { id: 5, name: "TypeScript", price: 550, publisher: "Apress" },
  { id: 6, name: "Redux", price: 600, publisher: "Manning" }
];




10 BOOK LIST + PAGINATION + ADD + EDIT + DELETE + SELECT
pages/Books.jsx
import { useState } from "react";
import { initialBooks } from "../data/books";

function Books() {
  const [books, setBooks] = useState(initialBooks);
  const [currentPage, setCurrentPage] = useState(1);
  const [bookName, setBookName] = useState("");

  const booksPerPage = 5;
  const start = (currentPage - 1) * booksPerPage;
  const paginatedBooks = books.slice(start, start + booksPerPage);

  function addBook() {
    setBooks([
      ...books,
      {
        id: books.length + 1,
        name: bookName,
        price: 500,
        publisher: "New Publisher"
      }
    ]);
    setBookName("");
  }

  function deleteBook(id) {
    setBooks(books.filter(b => b.id !== id));
  }

  return (
    <>
      <h3>Book List</h3>

      <input
        className="form-control mb-2"
        placeholder="New Book Name"
        value={bookName}
        onChange={(e) => setBookName(e.target.value)}
      />

      <button className="btn btn-success mb-3" onClick={addBook}>
        Add Book
      </button>

      <table className="table table-bordered">
        <thead className="table-dark">
          <tr>
            <th>Select</th>
            <th>Name</th>
            <th>Price</th>
            <th>Publisher</th>
            <th>Action</th>
          </tr>
        </thead>

        <tbody>
          {paginatedBooks.map(book => (
            <tr key={book.id}>
              <td><input type="checkbox" /></td>
              <td>{book.name}</td>
              <td>{book.price}</td>
              <td>{book.publisher}</td>
              <td>
                <button
                  className="btn btn-danger btn-sm"
                  onClick={() => deleteBook(book.id)}
                >
                  Delete
                </button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>

      <button
        className="btn btn-secondary me-2"
        onClick={() => setCurrentPage(currentPage - 1)}
        disabled={currentPage === 1}
      >
        Prev
      </button>

      <button
        className="btn btn-secondary"
        onClick={() => setCurrentPage(currentPage + 1)}
        disabled={start + booksPerPage >= books.length}
      >
        Next
      </button>
    </>
  );
}

export default Books;



TEST Cases
Action	Expected
Wrong login	Rejected
Correct login	Books page
Refresh	Still logged in
Add book	Appears
Delete book	Removed
Pagination	5 per page
Logout	Redirect to login

