body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

.navbar {
  background-color: #2c3e50;
  color: white;
  padding: 0.75em 1em;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.navbar a {
  color: white;
  text-decoration: none;
  margin-right: 1em;
}
.navbar .navbar-brand {
  font-size: 1.5em;
  font-weight: bold;
}
.navbar-nav {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
}
.navbar-nav li a {
  font-size: 1em;
}

.container {
  max-width: 900px;
  margin: 2em auto;
  padding: 0 1em;
}

h1 {
  margin-bottom: 1em;
  color: #34495e;
}

.pub-table {
  width: 100%;
  border-collapse: collapse;
}
.pub-table th,
.pub-table td {
  border: 1px solid #ccc;
  padding: 0.5em;
  text-align: left;
}
.pub-table th {
  background-color: #ecf0f1;
}
.pub-table tbody tr:hover {
  background-color: #f9f9f9;
}

.form-group {
  margin-bottom: 1.25em;
}
.form-group label {
  font-weight: bold;
}
.form-group input[type="text"],
.form-group input[type="number"],
.form-group textarea {
  width: 100%;
  padding: 0.5em;
  border: 1px solid #bbb;
  border-radius: 4px;
  font-size: 1em;
}
.btn-primary {
  background-color: #2980b9;
  color: white;
  padding: 0.5em 1em;
  border: none;
  border-radius: 4px;
  font-size: 1em;
  cursor: pointer;
}
.btn-small {
  background-color: #3498db;
  color: white;
  padding: 0.25em 0.5em;
  border: none;
  border-radius: 4px;
  font-size: 0.85em;
  cursor: pointer;
}

.alert {
  padding: 0.75em 1em;
  margin-bottom: 1em;
  border-radius: 4px;
}
.alert-success {
  background-color: #2ecc71;
  color: white;
}
.alert-danger {
  background-color: #e74c3c;
  color: white;
}

/* Abstract popup styling */
.abstract-popup {
  position: fixed;
  top: 50%;
  left: 50%;
  width: 60%;
  max-height: 60%;
  overflow-y: auto;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 1em;
  border: 1px solid #ccc;
  border-radius: 6px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  z-index: 1000;
}
.hidden {
  display: none;
}

/* Flash container */
.flash-container {
  margin-bottom: 1em;
}
.text-error {
  color: #e74c3c;
  font-size: 0.9em;
}

.footer {
  text-align: center;
  padding: 1em 0;
  background-color: #ecf0f1;
  margin-top: 2em;
  font-size: 0.9em;
  color: #7f8c8d;
}
