# API Connect 2018 - REST Management API

IBM API Connect 2018 provide complete access to the capability of the platform via REST APIs. It can be used to automate administration of the platform; for scripts and tools to support a continuous integration environment for API development and publishing; & much more. The operations provided in the REST API also correspond directly with commands in the toolkit CLI. 

Check the [API Connect 2018 API Explorer](https://openwhisk.ng.bluemix.net/api/v1/web/API%20Connect%20Native_apic-on-prem/default/index.http?cm_mc_uid=14353927892615385066501&cm_mc_sid_50200000=87881371554243875584) for the complete list of APIs.

# Steps to use REST Management API

## 1. Create a registration object using toolkit

login using toolkit
```cmd
apic login -s <servername> -u <user_id> -p <password> --realm <context/idp>
eg:
apic login -s cloud-apic.domain.com -u abhinab -p password --realm admin/default-idp-1
```

Register an app using the below command
```cmd
apic registrations:create --server <servername> <json_file>
Eg:
apic registrations:create --server manager-apic.domain.com app1.json
```
where app1.json will look like
```json
{
    "name": "app1",
    "client_id": "myapp1id", 
    "client_secret": "myapp1secret",
    "client_type": "toolkit"
}  
```

## 2. Get the Bearer token

Send a request to the OAuth endpoint

```json
POST /api/token
Accept: application/json
Content-Type: application/json
host: apimserver.example.com
Body
{
  "client_id": "myapp1id",
  "client_secret": "myapp1secret",
  "grant_type": "password",
  "realm": "provider/default-idp-2",
  "username": "abhinab",
  "password": "password",
}
```

sample curl request
```curl
curl --request POST \
    --url https://apimserver.example.com/api/token \
    --header 'Content-Type: application/json' \
    --header 'accept: application/json' \
    --data '{"username": "abhinab", "password": "password" "realm": "provider/default-idp-2", "client_id": "myapp1id", "client_secret": "myapp1secret", "grant_type": "password"}' 
```

## 3. Using the bearer taken 
Once obtained, the bearer token may be used to make authenticated API calls. The bearer token is sent as the value of the Authorization header, prefixed by the word bearer: (including the colon). Here's an example of an authenticated call to the /me resource using the bearer token obtained above:

```code
GET /me
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI4ODY2ZjE0Mi...sdjhks
Accept: application/json
Content-Type: application/json
host: apimserver.example.com
```