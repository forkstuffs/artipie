# Artifactory API Support

Artipie supports artifactory API to manage repositories, users and repository permissions.

## Create Repository

Creates new docker repository with `default` storage alias and provided name.

> **PUT** /api/repositories/{repoKey}

Consumes json with the following fields (any other fields are ignored): 

Field name | Type | Meaning | Required
------------ | ------------- | ------------ | ---------
key | string | New repository name | Y
rclass | string | Repository class, only `local` type is supported | Y
packageType | string | Artifact type, only `docker` type is supported | Y
dockerApiVersion | string | Docker API version, we only support `V2` | Y

Possible responses:
- `200 OK` when new repository was successfully created
- `400 BAD REQUEST` in the cases when repository with such name already exists or invalid json was sent
- `500 INTERNAL ERROR` in the case of unexpected server error

Artifactory documentation can be found [here](https://www.jfrog.com/confluence/display/rtf/artifactory+rest+api#ArtifactoryRESTAPI-CreateRepository). 

## Users

### Get Users

Endpoint to obtain [list](https://www.jfrog.com/confluence/display/rtf/artifactory+rest+api#ArtifactoryRESTAPI-GetUsers) of the existing users.

> **GET** /api/security/users

Returns json array of the following form:
```json
[
  {
    "name": "davids",
    "uri" : "http://localhost:8081/artifactory/api/security/users/davids",
    "realm" : "Internal"
  }, {
    "name": "danl",
    "uri" : "http://localhost:8081/artifactory/api/security/users/danl",
    "realm" : "Internal"
  }
]
```
Where `name` field contains user name, `uri` - URI to obtain user details, user `realm` is always `Internal`.

### Get User Details

Endpoint to obtain [user details](https://www.jfrog.com/confluence/display/rtf/artifactory+rest+api#ArtifactoryRESTAPI-GetUserDetails).

> **GET** /api/security/users/{userName}

Returns json with the following fields:

Field name | Type | Meaning | Required
------ | ------ | ------ | ------
name | string | User name | Y
email | string | User email, always constructed as {userName}@artipie.com | Y
lastLoggedIn | string | Default `2020-01-01T01:01:01.000+01:00` | Y
realm | string | User realm, value `Internal` is always returned | Y

If user is not found `404 NOT FOUND` status is returned.