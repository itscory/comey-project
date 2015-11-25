# The Comey Project
Named after FBI Director James Comey, the Comey Project is dedicated to encryption of user data on the cloud, a project I'm sure he would approve of /s.

I have worked alot on this project in PHP. Mid-project, I figured that I should not continue on using PHP and just rewrite it in Java.

#Account Object
When an account is created, it will be made into an object for easy coding. When account data needs to be saved, the Account object will be stripped apart by the client and saved in the SQL database. Here is the structure of the Account object:

| Variable    | Type  |  Description        |
| ----------  | ----- | ---------- |
| identifier  | byte[16]  | A 16-byte unique account identifier. |
| userID      | String    | Unique user-chosen username.  |
| salt        | byte[16]  | A 16-byte account-specific salt used for encryption and authentication. |
| pswdHash    | String    | The hash of a user-chosen password, combined with the account salt and static salt. |

#Algorithm
It is important for this project's security to have a good foundation. To make it easier for contributors to understand and improve it, the algorithm and the crypto-techniques used should be described here.
##Salty Password
Each account is going to have its own multi-purpose secure salt. Its password will be hashed with it. The salt should not be considered private to the user. However, since we want the ability to audit client-server authentication, we will take away the client's ability to authenticate itself. After authentication, the salt will be shared freely with the client so it can use it to perform other cryptic functions. We also have our own static salt that will be used in combination with the account-specific salt.
Pseudo: `user.pswd_hash = hash( password + user.salt + ourStaticSalt )`
