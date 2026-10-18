# RL-INSTITUTE — Updated Portal Package

## Included updates
- Premium login gate with Institute / Student / Parent role selector.
- Local preview is separate from real sign-in; passwords are not stored or checked in the browser.
- Fee Management has a **fee month** selector (`YYYY-MM`). Payments are assigned to the month they cover, not just the date received.
- Monthly due is calculated separately for each student and calendar month.
- Fully paid students show **Closed** for the selected month and no payment button; the next month is a separate cycle.
- Partial payments remain due until the monthly fee is covered.
- Daily admin-marked attendance, marks, report cards, CSV export, backup/restore and WhatsApp reminder composer remain available in local preview.
- SQL starter includes roles, class/course fee defaults, student-linked accounts, attendance, monthly fee payments, marks and row-level policies.

## Critical limitation — not production-ready yet
The login page is a prototype only. It **does not authenticate users**, and browser local storage is not private. The local admin preview is for testing with fictional data only. Do not enter real student names, phone numbers, passwords, fee details or marks until backend integration and security tests are complete.

## Secure backend setup required before live use
1. Create a Supabase project.
2. In Supabase SQL Editor, run `SUPABASE_STARTER.sql` in a new test project and review every policy.
3. Create user accounts through Supabase Auth. Never put passwords in HTML, JavaScript, SQL files or GitHub.
4. Add a `profiles` row for each Auth user. Use `role='admin'` for the institute account; for a student use `role='student'` and set `student_id` to the student's record ID.
5. The requested Admin ID is `RL25001`. ID-based login needs a server-side Edge Function that maps login ID to Auth user, or use a registered email. Do not expose a Supabase service-role key in the browser.
6. Parent access requires a verified parent-to-student relationship table and dedicated RLS policies. This starter intentionally does not grant generic parent access.
7. Connect the existing forms/views to Supabase Auth and queries. The current localStorage data layer is not yet connected to Supabase; this is required for shared data, secure sessions and actual student-only views.
8. Test with separate admin and student accounts: students must not edit data or access other students' records. Test logout/session expiry and monthly fee rules.
9. Only after policies pass security tests should real student data be entered.

## Monthly fee rule
Calendar-month billing is selected. Each student has a monthly fee, and the fee month is stored separately from payment date. A payment received in November can cover October if October is selected. Once October is fully paid, it shows closed; November is a separate cycle.

## Class/course fee structure
Each student can have an individual monthly fee in the current form. The SQL starter also includes `class_fee_rates` for class/course defaults. The workflow for managing class defaults and applying them automatically to new admissions must be connected during backend integration.

## Admin account
The requested Admin ID is `RL25001`. The password is intentionally not included in this package. Configure credentials through Supabase Auth/secure server-side setup. If the password shared in chat is an active password, change it before using a live account.

## GitHub Pages
GitHub Pages can host the static preview, but it does not make local data private. Upload only website code and setup files. Never upload real student data or secret keys.
