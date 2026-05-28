Function Refactoring Exercise — Data Validation Function
Selected Function
Data Validation Function with Nested Conditionals (JavaScript)
The original function contained:
multiple nested conditionals
validation logic
formatting checks
error handling
business rules
This made the function:
difficult to read
difficult to debug
difficult to maintain
Step 1 — Identifying Responsibilities
The original function had several responsibilities combined together:
Checking required fields
Validating email format
Validating password rules
Validating age restrictions
Building validation error messages
Returning validation results
This violated the Single Responsibility Principle because one function was handling many different tasks.
Step 2 — Decomposition Plan
The function was broken down into smaller helper functions.
Planned Helper Functions
Helper Function	Responsibility
validateRequiredFields()	Check missing fields
validateEmail()	Validate email format
validatePassword()	Validate password strength
validateAge()	Validate minimum age
buildValidationResult()	Format final response
Step 3 — Extracted Helper Functions
Email Validation
function validateEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
}
Password Validation
function validatePassword(password) {
    return password.length >= 8;
}
Age Validation
function validateAge(age) {
    return age >= 18;
}
Required Fields Validation
function validateRequiredFields(user) {
    return (
        user.name &&
        user.email &&
        user.password &&
        user.age
    );
}
Step 4 — Refactored Main Function
function validateUser(user) {
    const errors = [];
    if (!validateRequiredFields(user)) {
        errors.push("Missing required fields");
    }
    if (!validateEmail(user.email)) {
        errors.push("Invalid email format");
    }
    if (!validatePassword(user.password)) {
        errors.push("Password must be at least 8 characters");
    }
    if (!validateAge(user.age)) {
        errors.push("User must be at least 18 years old");
    }
    return {
        valid: errors.length === 0,
        errors
    };
}
Step 5 — Testing the Refactoring
The tests were run after refactoring to ensure:
the behavior remained unchanged
validation rules still worked
existing functionality was preserved
Example test:
expect(validateEmail("test@email.com")).toBe(true);
The tests confirmed that the refactored version produced the same results as the original function.
Refactoring Benefits
Improved Readability
The main function became:
shorter
easier to scan
easier to understand
Instead of deeply nested conditionals,
the logic is now clearly separated into descriptive helper functions.
Improved Maintainability
Changes can now be made independently.
For example:
email validation can change without affecting password validation
age rules can be updated separately
This reduces the risk of introducing bugs.
Improved Reusability
The helper functions can now be reused in:
registration forms
login systems
profile updates
API validation
Reflection Questions
How did breaking down the function improve its readability and maintainability?
Breaking the function into smaller helper functions made the code easier to understand because each function now has a single responsibility.
The main function became much cleaner and easier to follow, while the helper functions made the validation logic more organized and reusable.
It also became easier to debug and update individual validation rules without affecting the rest of the code.
What was the most challenging part of decomposing the function?
The most challenging part was identifying which logic should stay in the main function and which logic should be extracted into helper functions.
It was also important to ensure that the refactored version still behaved exactly the same as the original function after decomposition.
Which extracted function would be most reusable in other contexts?
The validateEmail() function would likely be the most reusable because email validation is commonly needed in:
registration systems
login forms
profile management
contact forms
APIs
It is generic and independent from the rest of the application logic.
Final Reflection
This exercise demonstrated how refactoring large functions into smaller helper functions improves:
readability
maintainability
reusability
debugging
testing
Using AI helped speed up the decomposition process by identifying responsibilities and suggesting logical function boundaries.
However, manual review was still important to ensure the refactored code remained correct and easy to understand.