# ClaimSys

Static GitHub Pages frontend with Firebase Authentication and Firestore.

## Staff Management

- Primary and Secondary Managers create Staff accounts using email and a temporary password (minimum 8 characters).
- A separate Firebase Auth instance keeps the manager signed in.
- Company access is stored in `companyAccess/{companyId}_{userId}`.
- Only the Primary Manager or Superadmin may promote/demote employees through `managerIds`.
- `managerId` remains the permanent primary company owner.
- Secondary Managers can add/remove Staff, but cannot change manager roles or remove other managers.
- Removing company access does not delete the employee's Firebase login.
- Existing claims, accounts, reports and company documents are preserved.

## Deployment

**Publish the repository's `firestore.rules` to Firebase project `claimsys-ada3b` before testing Staff Management.** GitHub Pages does not publish rules automatically.

Use Firebase Emulator tests for primary managers, secondary managers, staff, and forbidden cross-company access. The frontend no longer uses `manageCompanyPeople`. Previously deployed Cloud Functions may be retired separately after checking no clients still use them.

Account provisioning creates a Firebase Authentication login, then atomically writes the staff profile and company membership to Firestore. If that write fails, the client attempts to delete the new Authentication account.

## Setup

Firebase web config is public client configuration, not an admin key. Never commit credentials or secrets. An authorized administrator must bootstrap the first Superadmin in Firestore.

