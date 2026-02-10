1.	npm create vite@latest book-app
2.	cd book-app
3.	npm install
4.	npm run dev
5.	npm install bootstrap
6.	open the project in VisualStudio Code
7.	In src/main.jsx add:
import 'bootstrap/dist/css/bootstrap.min.css';
8.	Create Data folder under src
Create a file books.js
export const initialBooks = [
  { id: 1, name: "React Basics", price: 500, publisher: "O'Reilly" },
  { id: 2, name: "JavaScript Guide", price: 650, publisher: "Packt" },
  { id: 3, name: "Node.js", price: 700, publisher: "Pearson" },
  { id: 4, name: "HTML & CSS", price: 400, publisher: "Wiley" },
  { id: 5, name: "TypeScript", price: 550, publisher: "Apress" },
  { id: 6, name: "Redux", price: 600, publisher: "Manning" }
];

9.	Create folder called pages
10.	Create a file called Books.jsx
import { useState } from "react";
import { initialBooks } from "../data/books";

function Books() {
  const [books, setBooks] = useState(initialBooks);
  const [currentPage, setCurrentPage] = useState(1);
  const [bookName, setBookName] = useState("");

  const booksPerPage = 5;
  const start = (currentPage - 1) * booksPerPage;
  const pageBooks = books.slice(start, start + booksPerPage);

  function addBook() {
    if (!bookName) return;
    setBooks([...books, {
      id: books.length + 1,
      name: bookName,
      price: 500,
      publisher: "New Publisher"
    }]);
    setBookName("");
  }

  function deleteBook(id) {
    setBooks(books.filter(b => b.id !== id));
  }

  return (
    <>
      <h3>Books</h3>

      <div className="mb-3">
        <input className="form-control mb-2"
          placeholder="Book Name"
          value={bookName}
          onChange={(e) => setBookName(e.target.value)} />
        <button className="btn btn-success" onClick={addBook}>AddBook</button>
    </div>

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
          {pageBooks.map(book => (
            <tr key={book.id}>
              <td><input type="checkbox" /></td>
              <td>{book.name}</td>
              <td>{book.price}</td>
              <td>{book.publisher}</td>
              <td>
                <button className="btn btn-danger btn-sm"
                  onClick={() => deleteBook(book.id)}>Delete</button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>

      <button className="btn btn-secondary me-2"
        disabled={currentPage === 1}
        onClick={() => setCurrentPage(currentPage - 1)}>Prev</button>

      <button className="btn btn-secondary"
        disabled={start + booksPerPage >= books.length}
        onClick={() => setCurrentPage(currentPage + 1)}>Next</button>
    </>
  );
}

export default Books;
11.	Create Login.jsx under pages folder 

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
      setError("Invalid credentials");
    }
  }

  return (
    <div className="container mt-5 col-md-4">
      <h3 className="text-center">Login</h3>
      {error && <p className="text-danger">{error}</p>}
      <form onSubmit={handleLogin}>
        <input className="form-control mb-2" placeholder="Username"
          onChange={(e) => setUsername(e.target.value)} />
         <input type="password" className="form-control mb-3" placeholder="Password"
          onChange={(e) => setPassword(e.target.value)} />
        <button className="btn btn-primary w-100">Login</button>
      </form>
    </div>
  );
}

export default Login;


12.	Replace the code in App.jsx
13.	 import { BrowserRouter, Routes, Route, Navigate } from "react-router-dom";
14.	import Login from "./pages/Login";
15.	import Books from "./pages/Books";
16.	import Header from "./components/Header";
17.	import Footer from "./components/Footer";
18.	
19.	function ProtectedRoute({ children }) {
20.	  const isAuth = localStorage.getItem("loggedIn");
21.	  return isAuth ? children : <Navigate to="/" />;
22.	}
23.	
24.	function App() {
25.	  return (
26.	    <BrowserRouter>
27.	      <Header />
28.	      <div className="container mt-4">
29.	        <Routes>
30.	          <Route path="/" element={<Login />} />
31.	          <Route
32.	            path="/books"
33.	            element={
34.	              <ProtectedRoute>
35.	                <Books />
36.	              </ProtectedRoute>
37.	            }
38.	          />
39.	        </Routes>
40.	      </div>
41.	      <Footer />
42.	    </BrowserRouter>
43.	  );
44.	}
45.	
46.	export default App;
47.	
13. in index.html
No change

14. in main.jsx

Include
// Bootstrap CSS
import "bootstrap/dist/css/bootstrap.min.css";






