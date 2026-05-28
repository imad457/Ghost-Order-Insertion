# Ghost-Order-Insertion
Full-disclosure penetration testing report focusing on a critical Business Logic Bypass and Input Validation vulnerability (Ghost Order creation) on a WooCommerce platform.
### 📝 Vulnerability Summary: What Happened?

During a black-box assessment of the checkout endpoint (`/checkout/`), a critical business logic flaw was discovered in how input lengths are processed. 

* **The Vector:** The `billing_phone` parameter failed to enforce strict length restrictions on the server side.
* **The Trigger:** Sending an oversized payload of exactly **309 numeric characters** directly into the phone field caused a silent backend processing exception or database truncation error.
* **The Logic Collapse:** Instead of throwing a validation error and blocking the request, the application crashed silently, dropped all accompanying user metadata (Name, City, and Phone), but continued executing the routing logic.
* **The Outcome:** The server returned an HTTP `302 Found` redirect, successfully committing a **"Ghost Order"** into the database—a valid transaction record containing completely blank customer details.
* **The Risk:** This flaw allows malicious actors to automate checkout requests, flooding the database with corrupt logs and artificially exhausting product inventory metrics (causing legitimate items to show as "Out of Stock").
