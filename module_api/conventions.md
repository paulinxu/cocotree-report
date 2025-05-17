# Conventions

Code | Meaning
-----|----------------------------------------------
0    | Backend well working
1    | Bad Request (failed to auth, page not found)
2    | Server Error (Unexpected Error occur)

Status | Meaning
-------|----------------------------------------------
200    | ok
201    | User created successfully with JWT token
400    | Bad Request
401    | Unauthorized
403    | Forbidden
404    | Not found
405    | Method Not Allowed
409    | Conflict
500    | Internal server error 