# Sample Test Workflow: Authentication

This is an example of a complete test workflow file that demonstrates the expected format and level of detail.

## Overview

This workflow tests all authentication-related functionality including login, logout, signup, and password management.

## Pre-conditions

- [ ] Application is running on localhost:3000
- [ ] Test user exists: test@example.com / TestPass123!
- [ ] User is logged out at start of testing
- [ ] Database has been reset to known state

---

## Test Cases

### TC-001: Successful Login
**Priority**: Critical
**Type**: Functional

**Steps**:
1. Navigate to http://localhost:3000/login
2. Enter email: test@example.com
3. Enter password: TestPass123!
4. Click the "Sign In" button
5. Wait for page navigation

**Expected Result**:
- User is redirected to /dashboard
- Welcome message displays username
- Navigation shows authenticated state (logout button visible)

**Pass Criteria**:
- [ ] URL is /dashboard after login
- [ ] User name appears in header
- [ ] Logout option is visible

---

### TC-002: Login with Invalid Credentials
**Priority**: Critical
**Type**: Functional

**Steps**:
1. Navigate to http://localhost:3000/login
2. Enter email: wrong@example.com
3. Enter password: wrongpassword
4. Click the "Sign In" button

**Expected Result**:
- User stays on login page
- Error message appears: "Invalid email or password"
- Form is not cleared (email remains)

**Pass Criteria**:
- [ ] URL is still /login
- [ ] Error message is visible
- [ ] Email field retains value

---

### TC-003: Login Form Validation
**Priority**: High
**Type**: Functional

**Steps**:
1. Navigate to http://localhost:3000/login
2. Leave email field empty
3. Leave password field empty
4. Click the "Sign In" button

**Expected Result**:
- Form does not submit
- Validation errors appear for both fields

**Pass Criteria**:
- [ ] Email field shows required error
- [ ] Password field shows required error
- [ ] No network request is made

---

### TC-004: Logout Flow
**Priority**: Critical
**Type**: Functional

**Pre-condition**: User must be logged in first

**Steps**:
1. From dashboard, click user menu/avatar
2. Click "Logout" option
3. Confirm logout if prompted

**Expected Result**:
- User is redirected to login page or home
- Session is cleared
- Protected routes are no longer accessible

**Pass Criteria**:
- [ ] URL is /login or /
- [ ] Navigating to /dashboard redirects to login
- [ ] User menu no longer shows username

---

### TC-005: Remember Me Functionality
**Priority**: Medium
**Type**: Functional

**Steps**:
1. Navigate to login page
2. Enter valid credentials
3. Check "Remember me" checkbox (if exists)
4. Click login
5. Close browser tab
6. Open new tab and navigate to app

**Expected Result**:
- User should still be authenticated
- No login required on return

**Pass Criteria**:
- [ ] Session persists after tab close
- [ ] Dashboard loads without login prompt

---

### TC-006: Password Visibility Toggle
**Priority**: Low
**Type**: Visual

**Steps**:
1. Navigate to login page
2. Enter text in password field
3. Click the show/hide password icon (if exists)

**Expected Result**:
- Password text becomes visible/hidden
- Icon changes to indicate state

**Pass Criteria**:
- [ ] Password field type toggles between password/text
- [ ] Icon updates appropriately

---

## Edge Cases

### TC-007: Login During Network Failure
**Priority**: Medium
**Type**: Edge Case

**Steps**:
1. Disconnect network or block API calls
2. Attempt to login with valid credentials

**Expected Result**:
- Appropriate error message (network error)
- Form remains usable
- No infinite loading state

---

### TC-008: Rapid Login Attempts
**Priority**: Low
**Type**: Edge Case

**Steps**:
1. Submit login form rapidly 5+ times

**Expected Result**:
- Only one request is processed
- No duplicate sessions created
- Rate limiting message if implemented

---

## Post-Testing Cleanup

- [ ] Ensure test user is logged out
- [ ] Clear any test data created
- [ ] Reset database if modified
