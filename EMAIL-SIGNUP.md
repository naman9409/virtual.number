# Email signup

Sign up now accepts full name, email, password (minimum 10 characters) and confirm password. A new customer account is saved in PostgreSQL with zero balance, no PIN and unverified document status. Customer sets their own PIN after signup. Admin → Clients and login activity show the account. Existing demo and Google sign-in remain available. Email ownership is not verified by this form; email OTP has not been added. Use a new Nexora password, never your Gmail password.

Update: stop the app, extract this project, copy your previous working server/.env to its server folder, then run npm.cmd install, npm.cmd run db:setup, npm.cmd run dev. Setup preserves previous records and applies the Google/PIN columns if needed. Keep database port 5433 for your existing PostgreSQL 14 connection. Google client ID is optional for email signup.
