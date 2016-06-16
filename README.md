
## A Simple Transaction / Accounting System.

Built to be used mainly as an internal service.

```bash
# Install modules
npm i

# Setup database
mysql -u root -p <your root pass> < ./sql/db.sql

# Set the following environment variables, e.g.
# in ~/.profile or ~/.bashrc

# Export everything
set -a

TRANSACT_HOST="dev.api.com"
TRANSACT_PORT=3991

TRANSACT_API_SECRET="change-secret-to-something-better-68000"

# Note that TLS is a requirement in production !
TRANSACT_TLS_CERT="path/to/domain.com.crt"
TRANSACT_TLS_PRIVKEY="path/to/domain.com.key"

TRANSACT_MYSQL_DB="transact_dev"
TRANSACT_MYSQL_USER="transact"
TRANSACT_MYSQL_PASS="transact"

set +a

# Finally start the server
npm start
```

## Usage:

### 1. Create an account.

```bash
# Curl FTW.

# Create user `Ace` with email `ace@base.se`
curl -X POST -d 'name=Ace&email=ace@base.se' http://dev.api.com:3991/api/user

# Response
{
  "user":{
    "id":"17",
    "name":"Ace",
    "email":"ace@base.se",
    "created":"2016-06-11T07:24:15.000Z",
    "password":"ryHD1BYN" # automatically generated password
  }
}
```

### 2. Authenticate and get an access token.

```bash
# Authenticate user using email and password
curl -X POST -d 'email=ace@base.se&password=ryHD1BYN' http://dev.api.com:3991/api/auth

# Response
{ "token":"eyJhbGci..." }
```

### 3. Create an account for a given currency.

```bash
# NOTE: Requires an access token.
# Choose a currency like USD, SEK etc.
curl -H 'Authorization: eyJhbGci...' -X POST -d 'currency=USD' http://dev.api.com:3991/api/account

# Response
{
  "account":{
    "id":"10092",
    "userId":"17",
    "name":"USD_1",
    "type":1,
    "isInternal":0,
    "created":"2016-06-11T09:54:52.000Z",
    "balance":"0",
    "currency":"USD"
  }
}
```
