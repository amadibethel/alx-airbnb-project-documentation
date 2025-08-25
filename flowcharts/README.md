# Airbnb Clone Backend – System Process Flowcharts

This directory contains flowcharts representing workflows and processes for the Airbnb Clone backend.  
Flowcharts provide a clear step-by-step visualization of system processes, aiding in development and debugging.

## 1. Example Process: User Registration

### Steps

1. **Guest submits registration form**
   - Inputs: first name, last name, email, password, role
2. **System validates data**
   - Checks for required fields, valid email format, password strength
3. **Check for existing user**
   - Ensures email is unique
4. **Hash password**
   - Securely store password using bcrypt
5. **Save user to database**
   - Insert record into Users table
6. **Return success response**
   - Provide confirmation or error message

### Flowchart Diagram

The flowchart below visualizes this process:

![User Registration Flowchart](user-registration-flow.png)

> Note: Created using Draw.io / Diagrams.net and exported as PNG for repository inclusion.
