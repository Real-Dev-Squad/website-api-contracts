# AWS Access

**Note:**: This API can be only called from the discord slash command and which can be only executed by the super users

## **Requests**

[POST /aws/groups](#aws/groups)  - Creates and adds user to the AWS group    

## POST /aws/groups

Returns all users in the system.

- **Params**
  None
- **Query**
  - Optional: `dev` Passed as a feature to make the feature work
- **Body**
    - groupId,
    - userId
- **Headers**
  Content-Type: application/json
- **Cookie**
  Bot auth token - JWT (Signed by Discord slash commands private key)
- **Success Response:**
  - **Code:** 200
    - **Content:**

    ```
    {
        message: `User ${userId} successfully added to group ${groupId}.`
    }
    ```

  **If user not found given UserId**
  - **Code:** 400
    - **Content:**

    ```
    { 
        error: "User not found" 
    }
    ```

- **If user hasn't set their mail address:**
  - **Code:** 400
    - **Content:**
    ```
       {
        error: `User email is required to create an AWS user. Please update your email by setting up Profile service, url : ${PROFILE_SVC_GITHUB_URL}` 
       }
    ```

