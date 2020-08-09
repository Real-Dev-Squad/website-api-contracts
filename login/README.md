# Login

## Request

```http
POST /login
```

### Parameters

| Param      |  Description  |
|:----------:| :------------ |
| login_type | "github"      |
| token      |  The token we receive fron Github. No fancy salting/hashing required. HTTPS will take care of security |


## Response

### Headers

```http
Set-Cookie: 
    auth-token=<token-value>;
    Domain=realdevsquad.com;
    Same-Site=Strict;
    Max-Age=86400;
    HttpOnly;
    Secure;
```

### Status Codes

```http
200 OK
```

### Body

```json
{
    "msg": "Success"
}

```

| Status Code |  Message      |
|:-----------:| :------------ |
| 200         | Success       |
| 400         | Please make sure request was correct  |
| 403         | Not Allowed   |
