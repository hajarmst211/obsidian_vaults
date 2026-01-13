Adding user authentication to a database-driven application is a significant milestone. Since you are new to this, the most important thing to understand is that **security is about never storing raw passwords** and **isolating data** so one user cannot see another's records.

Here are the dependencies and the critical steps to implement authentication for your income tracking project.

### 1. New Dependencies
You will need a library to handle password security. You should never write your own encryption logic.
*   **`bcrypt`**: This is the industry standard for Python. it handles "salting" and "hashing" (scrambling) passwords so that even if your database is stolen, the passwords remain secure.
*   **`getpass`**: This is a standard Python library (no installation needed). It allows users to type their password in the terminal without the characters appearing on the screen.

### 2. Critical Steps

#### Step A: Update the Database Schema
Currently, your database is "global"—anyone using the app sees the same data. You need to link every piece of data to a specific person.
1.  **Create a `users` table**: This table should store a unique `user_id`, a `username` (unique), and a `password_hash` (a long string of scrambled characters).
2.  **Add Foreign Keys**: Add a `user_id` column to your existing tables: `bank_account`, `Transactions`, `expense_categories`, and `monthly_expenses`. 
3.  **Establish Ownership**: This ensures that when you run a `SELECT` query, you can add a `WHERE user_id = current_user` clause.

#### Step B: Implement the "Sign Up" Logic
Before a user can log in, they must register.
1.  **Capture Input**: Ask for a username and a password.
2.  **Hash the Password**: Use `bcrypt` to turn the plain-text password into a hash.
3.  **Store the Hash**: Insert the username and the *hash* (not the password) into the `users` table.

#### Step C: Implement the "Login" Logic
This is where you verify who the user is.
1.  **Fetch the User**: Take the entered username and look for it in the database.
2.  **Verify**: Use `bcrypt` to compare the entered password with the hash stored in the database.
3.  **Capture the Session**: If the password matches, retrieve that user's `user_id` and store it in a variable in your Python script. This ID will be used for the rest of the session.

#### Step D: Modify the Application Flow (`main.py`)
Your CLI needs a "Gatekeeper" before the main menu.
1.  **Initial Prompt**: When the script starts, instead of showing the main menu, show a "Welcome" menu with two options: `[1] Login` or `[2] Sign Up`.
2.  **Restrict Access**: Loop this menu until a user successfully logs in or exits the program.
3.  **Pass the User ID**: Update your functions (like `add_bank_account` or `display_table`) to accept `user_id` as an argument.

#### Step E: Update Database Queries (Filtering)
Every SQL query in your `user_input.py` (or `db_operations`) must be updated.
1.  **Filtering**: Change `SELECT * FROM bank_account` to `SELECT * FROM bank_account WHERE user_id = %s`.
2.  **Insertion**: When adding a transaction or bank account, you must now include the `user_id` in the `INSERT` statement so the record is "owned" by the person logged in.

### 3. Summary of the Security Workflow
1.  **User types password** $\rightarrow$ **Bcrypt hashes it** $\rightarrow$ **Database stores hash**.
2.  **User logs in** $\rightarrow$ **Bcrypt compares typed password to stored hash** $\rightarrow$ **Access granted**.

By following these steps, you will transform your project from a local tool into a multi-user application where everyone's financial data is private and secure.

---
#### C. Session Management (src/services/session.py)

Since you added a users table, most of your queries (like SELECT * FROM bank_account) now need a WHERE username = %s clause.

- Create a global session object to store the logged_in_user.
    
- Pass this username to all repository functions.

#### D. The Handler Layer (src/ui/handlers.py)

Move functions like create_bank_account here.

1. This file handles input().
    
2. Validates the data.
    
3. Calls the repository.py function.
    
4. Prints the PrettyTable result.

---
# what is flask blueprint