# Daily Rojmel Accounting — Version 3

A lightweight installable PWA starter using Firebase Authentication and Cloud Firestore.

## Included in this starter
- Email/password registration and sign-in
- New accounts start as `pending` and cannot access entries until approved
- Purchase entry supports Cash, Bank, and Credit Purchase
- Opening balance starts at ₹0
- Users submit deletion requests; an administrator must approve before the transaction is deleted
- Responsive PWA shell

## Firebase setup
The client Firebase web configuration is in `app.js`. Firebase web config is intended to be present in browser code; protect the project with Authentication, Firestore Security Rules, and appropriate API-key restrictions. Never put service-account credentials in this repository.

1. In Firebase Console, open project **daily-rojmel-accounting**.
2. Enable **Authentication → Sign-in method → Email/Password**.
3. Create a **Cloud Firestore** database.
4. Publish the contents of `firestore.rules` in Firestore → Rules. Review before publishing.
5. Deploy this repository with GitHub Pages (Settings → Pages → Deploy from a branch → `main` → `/ (root)`).

## First administrator bootstrap
For safety, self-registration only creates a pending user with role `user`. After the first account is registered:
1. In Firebase Console → Firestore Database → `users`, open the document whose ID is the new account's Firebase Auth UID.
2. Change `status` to `approved` and `role` to `admin` (both strings).
3. Sign out and sign in again. The app will show the administrator label and delete-approval queue.

Only trusted administrators should receive the admin role. Do not share Firebase passwords, OTPs, or service-account keys.

## Data collections
- `users/{uid}`: email, role, status, createdAt
- `transactions/{id}`: type, date, party, amount, paymentType, description, createdBy, createdAt, status
- `deleteRequests/{id}`: transactionId, requestedBy, status, createdAt, reviewedBy, reviewedAt

## Important
This is the initial Version 3 foundation, not a complete accounting system yet. Reports, receipts, payment entries, party ledgers, edit history, and richer admin/user management can be added in the next stages. Test with non-production data before using it for real accounts.