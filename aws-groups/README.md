# AWS Access

**Note:**: This API can be only called from the discord slash command and which can be only executed by the super users

## **Requests**

[POST /aws/groups](#aws/groups)  - Creates and adds user to the AWS group    

## POST /aws/groups/access

This endpoint aims to add the user into AWS group specified, it also checks if the user is already part of the AWS account if not it creates the user by itself, this flow requires user's email, so if the email is not present it throws an error stating users to set their email

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
- **Error Response handling**
    - **If user not found given UserId**
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

