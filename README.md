# Daily Rojmel Accounting — Version 3

An installable bilingual (English / Gujarati) accounting PWA using Firebase Authentication and Cloud Firestore.

## Included modules
- Email/password sign-in and admin-approved registration
- Purchase entry with Cash, Bank, Credit Purchase and optional product/quantity
- Sales entry with Sales Invoice or Delivery Challan print/PDF workflow
- Expense entry with user-creatable, reusable expense heads and expense-head reports
- Product Master with unit and cost price
- Production entry and product-wise stock (production minus sales)
- Combined Cash + Bank Rojmel and separate Cash Rojmel / Bank Rojmel
- Daily, monthly, quarterly, half-yearly, yearly and custom report periods
- Combined business profit summary (not item-wise profit): sales less estimated sold-goods cost and expenses
- Cash and Bank balance summaries with admin-managed opening balances
- Delete requests that require administrator approval
- Responsive PWA with language preference saved on the device

## Important accounting notes
- Profit is shown as one combined business total. The current estimate uses each sold product's cost price from Product Master multiplied by sold quantity, then subtracts expenses. Keep product cost prices current; this is not FIFO/weighted-average inventory valuation.
- Stock is calculated as production quantity minus sales quantity. Opening stock and purchase-to-stock integration are not part of this first module release.
- Rojmel includes cash/bank sales as receipts and cash/bank purchases and expenses as payments. Credit sales/purchases do not change cash or bank until a receipt/payment module is added.
- Sales document print opens the browser print dialog; choose “Save as PDF” to download. Add your legal business details and tax configuration before using invoices as statutory tax invoices.
- Test with sample data before relying on the reports for statutory accounting or tax filing.

## Firebase setup
The client Firebase web configuration is in `app.js`. Firebase web config is intended to be present in browser code; protect the project with Authentication, Firestore Security Rules, and appropriate API-key restrictions. Never put service-account credentials in this repository.

1. In Firebase Console, open project **daily-rojmel-accounting**.
2. Enable **Authentication → Sign-in method → Email/Password**.
3. Create a **Cloud Firestore** database.
4. Review and publish the contents of `firestore.rules` in Firestore → Rules. New `products`, `expenseHeads`, and `settings` collections require these matching rules.
5. Deploy with GitHub Pages (Settings → Pages → Deploy from a branch → `main` → `/ (root)`).

## First administrator bootstrap
For safety, self-registration creates a pending user with role `user`. After the first account is registered:
1. In Firebase Console → Firestore Database → `users`, open the document whose ID is the new account's Firebase Auth UID.
2. Set `status` to `approved` and `role` to `admin` (both strings).
3. Sign out and sign in again.

Only trusted administrators should receive the admin role. Do not share Firebase passwords, OTPs, or service-account keys.

## Firestore collections
- `users/{uid}`: email, role, status, createdAt
- `transactions/{id}`: purchase, sale, expense, production and other transaction records
- `products/{id}`: name, unit, costPrice
- `expenseHeads/{id}`: reusable expense-head names
- `settings/balances`: opening cash and bank balances
- `deleteRequests/{id}`: transactionId, requestedBy, status, createdAt, reviewedBy, reviewedAt
