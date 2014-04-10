Login with Namecoin
===================

A protocol for decentralized authentication using Namecoin


## Example

Let's suppose our example service is **Facebook**. The user's Namecoin identity is `id/johnsmith`.


### Step 0: User Visits facebook.com

Suppose our user opens www.facebook.com, and sees that **Login with Namecoin** is an example. On the Facebook login page, it is specified that Facebook's Namecoin domain identity is `d/facebook`.



### Step 1: Temporary Password Registration


1. Client looks up the name key `d/facebook` in the Namecoin blockchain. This key includes new values:

```clojure
{
  :register-url "https://www.facebook.com/register.php"
  :register-timeout 86400
}
```

2. Client generates a random passphrase.

Client's random passphrase is `correct horse battery staple`.

3. Client POSTs a temporary registration message to `register_url`:

```json
{
  "registration": {
    "nonce": "119ce833204d494499688dc6e30e1a74",
    "identity": "id/johnsmith",
    "password_hash: "p5k2$3e9$XXXX$DIwqKlLGRDL.B2KjvDYdKjmH2/U/iee4"
  }
  "signature": "GzMaHVvY7MCM6IGezMkdk+s4Fc/qAXkn80ettBdJbkyjmbNYB20uSwuwSJL5BuvhhE/oFgcd2KjtEI9vCzQtUZs="
}```

4. The server verifies the temporary registration:
  - Check that the nonce has not been seen yet
  - Look up `id/johnsmith` in the Namecoin blockchain
  - Verify that `signature` is `id/johnsmith`'s signature of the text value of the `registration` key.

If verification succeeds, the server updates its database, setting the login password hash of the user `id/johnsmith` to `p5k2$3e9$XXXX$DIwqKlLGRDL.B2KjvDYdKjmH2/U/iee4`, which is the PBKDF2 hash of `correct horse battery staple` (with a random salt).

5. The server responds with success to the client

6. The user can log in with the username `id/johnsmith` and password `correct horse battery staple`. This password is no longer valid after the time period specified by the service's `register_timeout` record.


