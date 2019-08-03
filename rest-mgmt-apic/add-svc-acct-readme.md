# REST API - Add service account to a space

The below steps are for adding a LDAP based service/autmation account to a space. For portal delegated user registry, step # 4 will be different.

## 1. Get the bearer token
Follow the steps mentioned [here](/rest-mgmt-apic/readme.md)

## 2. Get the space & role urls
```code
GET /api/spaces/<org_name>/<catalog_name>/<space_name>/roles/<role_name>
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI4ODY2ZjE0Mi...sdjhks
Accept: application/json
Content-Type: application/json
host: apimserver.example.com
Eg:
Sample Endpoint: https://apimserver.example.com/api/spaces/myorg/mycat/myspace/roles/developer
```
Extract the space url & the role url from the response
```code
Sample response
{
    "type": "role",
    "api_version": "2.0.0",
    "id": "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxx",
    "name": "developer",
    "title": "Developer",
    "summary": "Authors API and product definitions",
    "inherited_permission_urls": [
        :
    ],
    "scope": "space",
    "org_url": "",
    "catalog_url": "",
    "space_url": "",
    "url": ""
}
```

## 3. Create member invitation scoped at Space
Use the space url and role url fetched from step 2 in the body of the request.
```json
POST /api/spaces/<org_name>/<catalog_name>/<space_name>/member-invitations
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI4ODY2ZjE0Mi...sdjhks
Accept: application/json
Content-Type: application/json
host: apimserver.example.com
Body
{
  "name": "<invitation_name>",
  "notify": false, 
  "space_url": "<Fetched from step 2 - space_url>",
  "role_urls": [
    "<Fetched from step 2 - url>"
    ]
}
```
Response will be 201 Created.

The JSON response body will have activation link as one of the name-value pair. 
```json
"activation_link": "https://apimserver.example.com/auth/manager/register?activation=ZXlKSd....1Mms=" 
```
Extract the base64 encoded token from the link (everything after "activation=" in the link) and decode it to get the JWT.

## 4. Accept member invitation
Use the base64 decoded token (JWT) and send it along with authorization header as the bearer token
```json
POST /api/spaces/<org_name>/<catalog_name>/<space_name>/member-invitations/<invitation_name>/accept
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJm...s2k
Accept: application/json
Content-Type: application/json
host: apimserver.example.com
Body
{
	"client_id": "myapp1id",
	"client_secret": "myapp1secret",	
	"realm":"provider/<ldap-registry>",
	"username": "<svc_acct>",
	"password": "<password>"
}
```
Response will be 201 Created. The service account will be added to the space with the defined role.