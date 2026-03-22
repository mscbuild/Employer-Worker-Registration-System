# Employer-Worker-Registration-System
 
 ![](https://komarev.com/ghpvc/?username=mscbuild) 
 [![Author](https://img.shields.io/badge/Author-Yuri%20Dev-blue.svg)](http://mscbuild.github.io/)
 ![](https://img.shields.io/github/license/mscbuild/e-learning) 
 ![](https://img.shields.io/github/repo-size/mscbuild/e-learning)
![](https://img.shields.io/badge/PRs-Welcome-green)
![](https://img.shields.io/badge/code%20style-java-green)
![](https://img.shields.io/github/stars/mscbuild)
![](https://img.shields.io/badge/Topic-Github-lighred)
![](https://img.shields.io/website?url=https%3A%2F%2Fgithub.com%2Fmscbuild)

✅ Project Overview.

🎯 Goal:
A system where:

Employers and Workers can register and log in.

Employers can post jobs.

Workers can view jobs and apply.

📁 Project Structure
```ruby
EmployerWorkerSystem/
├── 📁 src/
│   └── 📁 com.system/
│       ├── 📁 controllers/
│       │   ├── LoginServlet.java
│       │   ├── LogoutServlet.java
│       │   ├── RegisterServlet.java
│       │   ├── ApplyJobServlet.java
│       │   ├── PostJobServlet.java
│       │   ├── UpdateStatusServlet.java
│       │   └── UploadResumeServlet.java (optional)
│       ├── 📁 utils/
│       │   └── DBUtil.java
│       │   └── EmailUtility.java
│       └── 📁 models/
│           └── User.java
│           └── Job.java
│           └── Application.java
│
├── 📁 WebContent/  (or webapp/)
│   ├── 📁 css/
│   │   └── style.css
│   ├── 📁 js/
│   │   └── app.js
│   ├── 📁 uploads/        <-- Uploaded images/resumes go here
│   ├── 📁 jsp/
│   │   ├── login.jsp
│   │   ├── register.jsp
│   │   ├── employer_dashboard.jsp
│   │   ├── worker_dashboard.jsp
│   │   ├── post_job.jsp
│   │   ├── available_jobs.jsp
│   │   ├── manage_applicants.jsp
│   │   ├── worker_applied_jobs.jsp
│   │   ├── profile.jsp
│   │   └── error.jsp
│   └── index.jsp
│
├── 📁 META-INF/
├── 📁 WEB-INF/
│   ├── web.xml
│   └── lib/ (if not using Maven: put JDBC, JavaMail JARs here)
│
├── 📄 pom.xml (if using Maven)
└── 📄 README.md

 
```
🧾 Database Tables Example (MySQL/PostgreSQL).

```ruby
-- User Table (Employer or Worker)
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(100),
    email VARCHAR(100),
    password VARCHAR(100),
    role ENUM('employer', 'worker')
);

-- Jobs (Posted by Employers)
CREATE TABLE jobs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    description TEXT,
    employer_id INT,
    FOREIGN KEY (employer_id) REFERENCES users(id)
);

-- Applications (Submitted by Workers)
CREATE TABLE applications (
    id INT AUTO_INCREMENT PRIMARY KEY,
    job_id INT,
    worker_id INT,
    FOREIGN KEY (job_id) REFERENCES jobs(id),
    FOREIGN KEY (worker_id) REFERENCES users(id)
);
```
🔧 Core Servlets.

`RegisterServlet.java`  – Handles user registration.

`LoginServlet.java` – Handles authentication and session.

`PostJobServlet.java` – Employer posts a job.

`ApplyJobServlet.java` – Worker applies for job.

🌐 Example: Registration Form (HTML/JSP)

```ruby
<form action="RegisterServlet" method="post">
  <input type="text" name="username" placeholder="Username" required><br>
  <input type="email" name="email" placeholder="Email" required><br>
  <input type="password" name="password" placeholder="Password" required><br>
  <select name="role">
    <option value="employer">Employer</option>
    <option value="worker">Worker</option>
  </select><br>
  <button type="submit">Register</button>
</form>
```
📌 Tips.

Store passwords hashed (e.g., using SHA-256).

Use sessions to manage login state.

Validate inputs both on client (JS) and server (Servlet).

Show different dashboards for employer vs. worker.

🔄 After Form Submission.

The form will send data to your RegisterServlet, where you'll handle:

Validating input

Inserting user into the database

Redirecting to login or dashboard

🧩 Next Steps.

Make sure your `web.xml` or annotations register this servlet.

Test the form and ensure the data is inserted into the database.

Add a `login.jsp` and LoginServlet.java next for authentication.

🔐 Optional: Secure with Session Filters.

🧩 How to Use.

In your dashboard pages (e.g. employer_dashboard.jsp, worker_dashboard.jsp), include a logout link:

```ruby
<a href="../LogoutServlet">Logout</a>
```

🛡️ Optional: Session Check Snippet in JSP.

To protect pages from unauthorized access, add this at the top of protected JSPs:
```ruby
<%
  String user = (String) session.getAttribute("username");
  if (user == null) {
      response.sendRedirect("../login.jsp");
  }
%>
```
🧩 Let's implement the Job Posting feature for employers in your system.

This feature will include:

A JSP form `(post_job.jsp)` to enter job details

A Servlet `(PostJobServlet.java)` to handle form submission

A MySQL table to store job posts.

✅ 1. Create the jobs Table.

Make sure your database has this table:
```ruby
CREATE TABLE jobs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    employer_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (employer_id) REFERENCES users(id)
);
```
✅ 2. JSP Form:
✅ 3. Servlet:

🔧 IMPORTANT – Track userId on Login.
Update your LoginServlet to store the user's ID:
```ruby
int userId = rs.getInt("id");
session.setAttribute("userId", userId);
```

🧩 Overview,

This will include:

A job listing JSP for workers

A button/form to apply for a job

A new database table to store applications

An `ApplyJobServlet` to handle applications

✅ 1. Create job_applications Table.

In your MySQL database:
```ruby
CREATE TABLE job_applications (
    id INT AUTO_INCREMENT PRIMARY KEY,
    job_id INT NOT NULL,
    worker_id INT NOT NULL,
    applied_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (job_id) REFERENCES jobs(id),
    FOREIGN KEY (worker_id) REFERENCES users(id)
);
```
✅ 2. JSP Page: `job_list.jsp`.

This lists jobs and shows an “Apply” button for each job. Add a check so only workers can access it.

✅ 3. Servlet: `ApplyJobServlet.java`

✅ 4. Add Link to Job List (from dashboard).

On `worker_dashboard.jsp`, link to the job listing:
```ruby
<a href="job_list.jsp">Browse Jobs</a>
```
🧩 Feature Overview.

JSP page: `view_applicants.jsp`

Fetch applicants for jobs posted by the logged-in employer

Display job title and list of workers who applied

✅ 1. JSP Page.

✅ 2. Add Link in `employer_dashboard.jsp`:
```ruby
<a href="view_applicants.jsp">View Applicants</a>
```

🧩 What's Included:

Update the `job_applications` table to include a `status` field

Modify the ApplyJobServlet to set default status

Create a JSP page for employers to update application status

Create a JSP page for workers to view their applied jobs with status

✅ 1. Update job_applications Table.

Run this SQL to add a status column:
```ruby
ALTER TABLE job_applications ADD COLUMN status VARCHAR(20) DEFAULT 'Pending';
```

✅ 2. Modify `ApplyJobServlet.java`

Add this line when inserting application:
```ruby
String sql = "INSERT INTO job_applications (job_id, worker_id, status) VALUES (?, ?, 'Pending')";
```
✅ 3. JSP: `manage_applicants.jsp` (Employer View).

✅ 4. `UpdateStatusServlet.java`.
 
✅ 5. `worker_applied_jobs.jsp` – Worker View,
 
✅ 6. Add Links in Dashboards.

In `employer_dashboard.jsp` :
```ruby
<a href="manage_applicants.jsp">Manage Application Status</a>
```

*In `worker_dashboard.jsp` :
```ruby
<a href="worker_applied_jobs.jsp">My Applications</a>
```

🧩 Goal.

When an employer changes the status of an application, the worker gets an automated email notification.

✅ Steps:

Configure JavaMail in your project

Update `UpdateStatusServlet` to send an email after updating

Create an `EmailUtility` class for reusable mail sending

✅ 1. Add JavaMail Dependency.

If using Maven, add this to `pom.xml`:

If not using Maven, download `javax.mail` jar and add it to your project’s `lib` folder.

✅ 2. `EmailUtility.java` (Utility Class).

✅ 3. Modify `UpdateStatusServlet.java` .

After updating status in DB, fetch worker email and send email:

✅ Key Features Included.

User Registration & Login: Separate flows for employers and workers.

Job Posting: Employers can post job listings.

Job Application: Workers can apply to jobs.

Application Status Management: Employers can update application statuses; workers can view them.

Image Upload: Users can upload profile images during registration.

Email Notifications: Workers receive emails upon status updates.

🚀 Getting Started.

Database Setup:

Create a MySQL database named `employer_worker_system`.

Execute the provided `schema.sql` script to create necessary tables.

Configure Database Connection:

Update `DBUtil.java` with your database URL, username, and password.

Build and Deploy:

If using Maven, run `mvn clean install`.

Deploy the WAR file to your servlet container (e.g., Apache Tomcat).

Access the Application:

Navigate to `http://localhost:8080/EmployerWorkerSystem/` in your browser.

📦 The Project Template
