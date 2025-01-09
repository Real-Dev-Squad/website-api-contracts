# Auth

## Endpoints

| Route | Method | Description |
|-------|--------|-------------|
| [/auth/google/login](#get-authgooglelogin) | GET | Initiates the Google OAuth authentication |
| [/auth/google/callback](#get-authgooglecallback) | GET | Handles the callback from Google after the user authenticates |
| [/auth/github/login](#get-authgithublogin) | GET | Initiates the GitHub OAuth authentication |
| [/auth/github/callback](#get-authgithubcallback) | GET | Handles the callback from GitHub after the user authenticates |

## GET /auth/google/login

Initiates the Google OAuth authentication process by redirecting the user to Google's consent screen.

- **Params**
  None

- **Query**
  - Required: `dev=[boolean]` (Must be set to true for this feature to work.)
  - Optional: `redirectURL=[string]` (The URL to redirect the user to after authentication is successful. It should be a valid URL.)

### Response

- **Success Response**:
  - **Code:** 302  

  - **Content:** Redirects to Google's OAuth 2.0 consent screen for user authentication.

    ```text
    Location: https://accounts.google.com/o/oauth2/v2/auth?client_id={CLIENT_ID}&redirect_uri={REDIRECT_URI}&response_type=code&scope=email&state={redirectURL}
    ```

- **Error Response:**
  - **Code:** 401
  - **Content:**

    ```json
    { 
      "statusCode": 401, 
      "error": "Unauthorized", 
      "message": "User cannot be authenticated" 
    }
    ```
  - **Code:** 500

  - **Content:**

    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```

## GET /auth/google/callback

Handles the callback from Google after the user authenticates, exchanges the authorization code for an access token, and completes the user login process.

- **Params**
  None

- **Query**
  - Required: `code=[string]` (The authorization code returned by Google after the user grants consent.)
  - Required: `state=[string]` (The state parameter returned by Google, used to verify the request’s legitimacy and ensure security.)

### Response

- **Success Response**:
  - **Code**: 302
  - **Content**: Redirects to the specified redirectURL or https://my.realdevsquad.com/new-signup if user details are incomplete.

    ```
    Location: {redirectURL}
    ```
  - **Cookie**: A secure JWT authentication token (rds-session) is set as a cookie to maintain the user's session.
    ```
    Set-Cookie: rds-session=<jwt_token>; Domain={realdevsqual.com}; Expires={expirationTime}; HttpOnly; Secure; SameSite=Lax
    ```

- **Error Response:**
  - **Code:** 401
  - **Content:**

    ```json
    { 
      "statusCode": 401, 
      "error": "Unauthorized", 
      "message": "User cannot be authenticated" 
    }
    ```
  - **Code:** 500

  - **Content:**

    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```
## GET /auth/github/login

Initiates the GitHub OAuth authentication process by redirecting the user to GitHub's consent screen.

- **Params**
  None

- **Query**
  - Optional: `redirectURL=[string]` (The URL to redirect the user to after authentication is successful. It should be a valid URL.)

### Response

- **Success Response**:
  - **Code:** 302  

  - **Content:** Redirects to GitHub's OAuth 2.0 consent screen for user authentication.

    ```text
    Location: Location: https://github.com/login/oauth/authorize?client_id={CLIENT_ID}&redirect_uri={REDIRECT_URI}&response_type=code&scope=user:email&state={state}
    ```

- **Error Response:**
  - **Code:** 401
  - **Content:**

    ```json
    { 
      "statusCode": 401, 
      "error": "Unauthorized", 
      "message": "User cannot be authenticated" 
    }
    ```
  - **Code:** 500

  - **Content:**

    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```

## GET /auth/github/callback

Handles the callback from GitHub after the user authenticates, exchanges the authorization code for an access token, and completes the user login process.

- **Params**
  None

- **Query**
  - Required: `code=[string]` (The authorization code returned by GitHub after the user grants consent.)
  - Required: `state=[string]` (The state parameter returned by GitHub, used to verify the request’s legitimacy and ensure security.)

### Response

- **Success Response**:
  - **Code**: 302
  - **Content**: Redirects to the specified redirectURL or https://my.realdevsquad.com/new-signup if user details are incomplete.

    ```
    Location: {redirectURL}
    ```
  - **Cookie**: A secure JWT authentication token (rds-session) is set as a cookie to maintain the user's session.
    ```
    Set-Cookie: rds-session=<jwt_token>; Domain={realdevsqual.com}; Expires={expirationTime}; HttpOnly; Secure; SameSite=Lax
    ```

- **Error Response:**
  - **Code:** 401
  - **Content:**

    ```json
    { 
      "statusCode": 401, 
      "error": "Unauthorized", 
      "message": "User cannot be authenticated" 
    }
    ```
  - **Code:** 500

  - **Content:**

    ```json
    {
      "statusCode": 500,
      "error": "Internal Server Error",
      "message": "An internal server error occurred"
    }
    ```
