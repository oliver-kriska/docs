This document describes all the resources that make up the Semaphore 2.0 API version `v1alpha` . If you have any problems or requests please [contact support](mailto:support@semaphoreci.com).

The root of the API is `https://{org_name}.semaphoreci.com/api/v1alpha`.

- [Overview](#overview)
  - [Constraints](#constraints)
  - [Authentication](#authentication)
  - [Errors](#errors)
  - [Pagination](#pagination)
  - [Stability](#stability)
  - [Changelog](#changelog)
- [Workflows](#workflows)
  - [Describe Workflow](#describe-workflow)
  - [List Workflows](#list-workflows)
  - [Rerun Workflow](#rerun-workflow)
  - [Stop Workflow](#stop-workflow)
- [Promotions](#promotions)
  - [List Promotions](#list-promotions)
  - [Trigger Promotion](#trigger-promotion)
- [Pipelines](#pipelines)
  - [Describe Pipeline](#describe-pipeline)
  - [List Pipelines](#list-pipelines)
  - [Stop Pipeline](#stop-pipeline)
- [Jobs](#jobs)
  - [Describe Job](#describe-job)
  - [Stop Job](#stop-job)
  - [Get Log](#get-log)

## Overview

### Constraints
Every API request and response satisfies the following constraints:

- All requests must use HTTPS.
- All data is sent and received as JSON.
- Blank fields are included as `null` instead of being omitted.
- Timestamps are in different formats due to historical circumstances how these public APIs were appearing. In next release of API these will be standardized. Currently there are following formats:
  - Unixtime Epoch time: `"create_time": "1571083003"`
  - Unixtime Epoch time with nanoseconds: `"created_at": {"seconds": 1571063401, "nanos": 559492000}`
  - Custom format: `YYYY-MM-DD HH:MM:SS.ffffffZ` e.g.`"2019-10-14 12:11:47.824128Z"`

### Authentication

All API requests require authentication. To authenticate, you need an
authentication token. You can find your authentication token by visiting your
[account settings](https://me.semaphoreci.com/account).

Authentication Token is sent as a HTTP header

``` bash
curl -H "Authorization: Token <token>" https://{org_name}.semaphoreci.com/api/v1alpha
```

### Errors

There are several errors that you can receive as a response to an API request:

#### Failing to authenticate

```
HTTP/1.1 401 Unauthorized
```

#### Requesting non existing resources

```
HTTP/1.1 404 Not Found
```

#### Requesting resources that are not visible to the user

```
HTTP/1.1 404 Not Found
```

### Pagination

Every request that that returns more than 30 items will be paginated.
Form calls with `link` header values instead of constructing your own URLs.

The `link` header includes information about pagination:

```
link: <http://{org_name}.semaphoreci.com/api/v1alpha/?PAGE_PARAMS>; rel="first",
      <http://{org_name}.semaphoreci.com/api/orgs?PAGE_PARAMS>; rel="nest"
```

The possible `rel` values are:

- **next** - The link for the next page of results.
- **prev** - The link previous page of results.
- **first** -  The link for the first page of results.

### Stability

- Compatible and emergency changes may be made with no advance notice
- Disruptive changes may not occur, instead a new major version is developed

#### Types of changes

##### Compatible change

Small in scope and unlikely to break or change semantics of existing methods.

- Adding nested resources, methods and attributes
- Change of documentation
- Change of undocumented behavior

##### Disruptive change

May have larger impact and effort will be made to provide migration paths as needed.

- Change semantics of existing methods
- Remove resources, methods and attributes

##### Emergency change

- May have larger impact, but are unavoidable due to legal compliance, security vulnerabilities or violation of specification.

### Changelog

No changes.

## Workflows

### Describe Workflow

```
GET {org_name}.semaphoreci.com/api/v1alpha/plumber-workflows/:workflow_id
```

**Params**

- `workflow_id` - ID of a workflow

**Response**

```json
HTTP status: 200

{
  "workflow": {
    "wf_id": "72c434c4-6589-493d-97cd-22f46681c893",
    "requester_id": "d32141ca-1552-4370-b0d4-4030aa9cf524",
    "project_id": "adaede30-9de5-471f-9f95-b7d437170f10",
    "initial_ppl_id": "f86b3b5e-c3de-4f77-849f-39e080374ce4",
    "hook_id": "6d7ed9d3-3047-4d5e-9b27-f0b68b228409",
    "created_at": {
      "seconds": 1571063401,
      "nanos": 559492000
    },
    "commit_sha": "6fe03f118b7aa7b8ea1a983c3faee4f8b54213a5",
    "branch_name": "master",
    "branch_id": "e8a4ad3b-4951-4520-aed7-6292ebd70076"
  }
}
```

**Example**

```
curl -i -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/api/v1alpha/plumber-workflows/:workflow_id
```

### List Workflows

```
GET https://{org_name}.semaphoreci.com/api/v1alpha/plumber-workflows?project_id=:project_id
```

**Params**

- `project_id` (**required**) - ID of a project
- `branch_name` (*optional*) - Name of branch, used as filter

**Response**

```json
HTTP status: 200

[
  {
    "wf_id": "a99a75c3-f921-4fa9-a43f-69b2cede6274",
    "requester_id": "fcd7fe34-f73f-4686-821b-ce02cb970b22",
    "project_id": "c394d20b-b3c6-4c90-b743-a9a65fa95a78",
    "initial_ppl_id": "e1a678ba-ed2d-412f-b350-7333579bb0d3",
    "hook_id": "4a1d3cf7-c3d5-42ec-aa22-c31dffa9f05d",
    "created_at": {
      "seconds": 1570792145,
      "nanos": 544028000
    },
    "commit_sha": "cac345d0a7d425e23e18f7be33e9b441f95c65f5",
    "branch_name": "gallery",
    "branch_id": "70f52bdd-2dab-427a-81b1-d2999bc8c2a8"
  },
  {
    "wf_id": "e08a7a60-413c-4224-a208-9c67302d3ba1",
    "requester_id": "fcd7fe34-f73f-4686-821b-ce02cb970b22",
    "project_id": "c394d20b-b3c6-4c90-b743-a9a65fa95a78",
    "initial_ppl_id": "64dc1837-aaad-4907-a7db-aedfe091a987",
    "hook_id": "84de6482-8f5b-4f31-996f-528b6d8fa771",
    "created_at": {
      "seconds": 1570715702,
      "nanos": 345824000
    },
    "commit_sha": "aa292a7a8de08bc6246de697b84d2531fc64a43b",
    "branch_name": "gallery",
    "branch_id": "70f52bdd-2dab-427a-81b1-d2999bc8c2a8"
  }
]
```

**Example**

```
curl -i -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/api/v1alpha/plumber-workflows\?project_id\=:project_id
```

### Rerun Workflow

```
POST {org_name}.semaphoreci.com/api/v1alpha/plumber-workflows/:workflow_id/reschedule?request_token=:request_token
```

**Params**

- `workflow_id` - ID of a workflow that you want to rerun
- `request_token` - Idempotency token, can be any string

**Response**

```json
HTTP status: 200

{
  "wf_id": "965d3c3d-bbe6-4ff7-b62a-1ff51a92bdc0",
  "ppl_id": "6cf4569c-f76c-4dea-b293-3e4282ba1153"
}
```

**Example**

```
curl -i -X POST -H "Authorization: Token {api_token}" \
        https://{org_name}.semaphoreci.com/api/v1alpha/plumber-workflows/:workflow_id/reschedule\?request_token\=:request_token
```

### Stop Workflow

```
POST {org_name}.semaphoreci.com/api/v1alpha/plumber-workflows/:workflow_id/terminate
```

**Params**

- `workflow_id` - ID of a workflow that you want to stop

**Response**

```
HTTP status: 200
```

**Example**

```
curl -i -X POST -H "Authorization: Token {api_token}" \
        https://{org_name}.semaphoreci.com/api/v1alpha/plumber-workflows/:workflow_id/terminate
```

## Pipelines

### Describe Pipeline

```
GET {org_name}.semaphoreci.com/api/v1alpha/pipelines/:pipeline_id
```

**Params**

- `pipeline_id` (**required**) - ID of a pipeline
- `detailed` (*optional*) - Default: `false`, include all information about all blocks, and jobs. Much more expensive you are only interested in the status of pipeline, don't set detailed to `true`.

**Response**

```json
HTTP status: 200

{
  "pipeline": {
    "yaml_file_name": "semaphore.yml",
    "working_directory": ".semaphore",
    "wf_id": "80b8bae6-bc1d-44bc-a233-2bf9ba78ae5c",
    "terminated_by": "",
    "terminate_request": "",
    "switch_id": "bf2652a4-56c6-4b4c-92ff-b2c623ae90b7",
    "stopping_at": "1970-01-01 00:00:00.000000Z",
    "state": "done",
    "snapshot_id": "",
    "running_at": "2019-10-14 12:11:47.824128Z",
    "result_reason": "test",
    "result": "failed",
    "queuing_at": "2019-10-14 12:10:43.783017Z",
    "project_id": "c394d20b-b3c6-4c90-b743-a9a65fa95a78",
    "ppl_id": "de4e1476-11ad-481c-afe1 -620bb263887d",
    "pending_at": "2019-10-14 12:10:43.769695Z",
    "name": "Pipeline",
    "hook_id": "dcdf2aad-9221-4987-ae7e-50c978675935",
    "error_description": "",
    "done_at": "2019-10-14  12:15:31.090206Z",
    "created_at": "2019-10-14 12:10:43.460665Z",
    "commit_sha": "b602f0594c8d8ad50ec8e848eb6ea0399692117c",
    "branch_name": "mixpanel-plug",
    "branch_id": "591b2951 -5c30-4955-ba05-9426a04d5c4d"
  },
  "blocks": []
}
```

Response with `datailed` param set to `true`:

```json
{
  "pipeline": {
    "yaml_file_name": "semaphore.yml",
    "working_directory": ".semaphore",
    "wf_id": "80b8bae6-bc1d-44bc-a233-2bf9ba78ae5c",
    "terminated_by": "",
    "terminate_request": "",
    " switch_id": "bf2652a4-56c6-4b4c-92ff-b2c623ae90b7",
    "stopping_at": "1970-01-01 00:00:00.000000Z",
    "state": "done",
    "snapshot_id": "",
    "running_at": "2019-10-14 12:11:47.824128Z",
    "result_reason": "test",
    "result": "failed",
    "queuing_at": "2019-10-14 12:10:43.783017Z",
    "project_id": "c394d20b-b3c6-4c90-b743-a9a65fa95a78",
    "ppl_id": "de4e1476-11ad-481c-afe1 -620bb263887d",
    "pending_at": "2019-10-14 12:10:43.769695Z",
    "name": "Pipeline",
    "hook_id": "dcdf2aad-9221-4987-ae7e-50c978675935",
    "error_description": "",
    "done_at": "2019-10-14  12:15:31.090206Z",
    "created_at": "2019-10-14 12:10:43.460665Z",
    "commit_sha": "b602f0594c8d8ad50ec8e848eb6ea0399692117c",
    "branch_name": "mixpanel-plug",
    "branch_id": "591b2951 -5c30-4955-ba05-9426a04d5c4d"
  },
  "blocks": [
    {
      "state": "done",
      "result_reason": "test",
      "result": "passed",
      "name": "Lint",
      "jobs": [
        {
          "status": "",
          "result": "",
          "name": "",
          "job_id": "",
          "i ndex": 0
        },
        {
          "status": "",
          "result": "",
          "name": "",
          "job_id": "",
          "index": 0
        }
      ],
      "error_description": "",
      "build_req_id": "3ebcf42b-798e-472c-8683-a9bfde2ed069",
      "block_id": "188e843e-820 3-4b6f-89e6-53e9d36879b2"
    },
    {
      "state": "done",
      "result_reason": "test",
      "result": "failed",
      "name": "Test",
      "jobs": [
        {
          "status": "",
          "result": "",
          "name": "",
          "job_id": "",
          "index": 0
        },
        {
          "sta tus": "",
          "result": "",
          "name": "",
          "job_id": "",
          "index": 0
        },
        {
          "status": "",
          "result": "",
          "name": "",
          "job_id": "",
          "index": 0
        }
      ],
      "error_description": "",
      "build_req_id": "91fedd38-82ba-41c2 -bc8b-a2ec6911e8aa",
      "block_id": "44153495-4502-406f-8cab-b920879f7f84"
    },
    {
      "state": "done",
      "result_reason": "test",
      "result": "passed",
      "name": "Build",
      "jobs": [
        {
          "status": "",
          "resu lt": "",
          "name": "",
          "job_id": "",
          "index": 0
        }
      ],
      "error_description": "",
      "build_req_id": "823ab9ae-9f30-41a7-8537-9ef53ca88f6f",
      "block_id": "34b27e46-28a0-4fdc-a8e3-242c02228b9a"
    }
  ]
}
```

**Example**

```
curl -i -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/api/v1alpha/pipelines/:pipeline_id
```


### List Pipelines

```
GET {org_name}.semaphoreci.com/api/v1alpha/pipelines?project_id=:project_id
```

**Params**

- `project_id` (**required**) - ID of a project
- `wf_id` - (*optional*) - ID of a workflow
- `branch_name` (*optional*) - name of a branch
- `yml_file_path` (*optional*) - YML file that contains pipeline definition


**Response**

```json
HTTP status: 200

[
  {
    "yaml_file_name": "semaphore.yml",
    "working_directory": ".semaphore",
    "wf_id": "484e263a-424a-4820-bff0-bba436c54042",
    "terminated_by": "",
    "terminate_request": "",
    "switch_id": "c3e752e9-74ab-4207-bda9-4a9ce4ef17a0",
    "stopping_at": {
      "seconds": 0,
      "nanos": 0
    },
    "state": "DONE",
    "snapshot_id": "",
    "running_at": {
      "seconds": 1571076845,
      "nanos": 810862000
    },
    "queuing_at": {
      "seconds": 1571076843,
      "nanos": 878741000
    },
    "project_id": "c394d20b-b3c6-4c90-b743-a9a65fa95a78",
    "ppl_id": "0a9563f9-09a3-4450-a9bf-75b75373881a",
    "pending_at": {
      "seconds": 1571076843,
      "nanos": 868054000
    },
    "name": "Pipeline",
    "hook_id": "cd7f6162-9b6e-435a-89a7-3968b542e9c7",
    "error_description": "",
    "done_at": {
      "seconds": 1571076991,
      "nanos": 166159000
    },
    "created_at": {
      "seconds": 1571076843,
      "nanos": 537730000
    },
    "commit_sha": "ac3f9796df42db976814e3fee670e11e3fd4b98a",
    "branch_name": "ms\/another-test-branch",
    "branch_id": "a79557f2-dc4e-4807-ba89-601401eb3b1e"
  }
]
```

**Example**

```
curl -i -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/api/v1alpha/pipelines\?project_id\=:project_id
```

### Stop Pipeline

```
POST {org_name}.semaphoreci.com/api/v1alpha/pipelines?project_id=:project_id
```

**Params**

- `pipeline_id` - ID of a pipeline
- `terminate_request` - Must be set to `true`

**Response**

```
HTTP status: 200
```

**Example**

```
curl -i -X PATCH  -H "Authorization: Token {api_token}" \
     --header "Accept: application/json"  --header "Content-Type: application/json" \
     --data '{"terminate_request": true}' \
     https://semaphore.semaphoreci.com/api/v1alpha/pipelines/:pipeline_id
```

## Promotions

### List Promotions

```
GET {org_name}.semaphoreci.com/api/v1alpha/promotions?pipeline_id=:pipeline_id
```

**Params**

- `pipeline_id` - ID of a pipeline

**Response**

```json
HTTP status: 200

[
  {
    "triggered_by": "Pipeline Done request",
    "triggered_at": {
      "seconds": 1571065763,
      "nanos": 817290000
    },
    "status": "passed",
    "scheduled_pipeline_id": "d605c1ed-5664-4ce3-8419-14d3d7337c35",
    "scheduled_at": {
      "seconds": 1571065764,
      "nanos": 900999000
    },
    "override": false,
    "name": "production"
  }
]
```

**Example**

```
curl -i -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/api/v1alpha/promotions\?pipeline_id\=:pipeline_id
```

### Trigger Promotion

```
POST {org_name}.semaphoreci.com/api/v1alpha/promotions
```

**Params**

- `pipeline_id` - ID of a pipeline
- `name` - Name of promotion

**Response**

```
HTTP status: 200
```

**Example**

```
curl -H "Authorization: Token {api_token}"  \
     -d "name=:promotion_name&pipeline_id=:pipeline_id"  \
     -X POST  https://semaphore.semaphoreci.com/api/v1alpha/promotions
```

## Jobs

### Describe Job

```
GET {org_name}.semaphoreci.com/api/v1alpha/jobs/:job_id
```

**Response**

```json
HTTP status: 200

{
  "metadata": {
    "name": "Job #1",
    "id": "bc8826bd-dbb2-4d28-8c90-7f370ce478fe",
    "create_time": "1571083003",
    "update_time": "1571083507",
    "start_time": "1571083006",
    "finish_time": "1571083507"
  },
  "spec": {
    "project_id": "162987ba-bda7-4e54-9c45-977a8cc6087d",
    "agent": {
      "machine": {
        "type": "e1-standard-2",
        "os_image": "ubuntu1804"
      }
    },
    "env_vars": [
      {
        "name": "SEMAPHORE_WORKFLOW_ID",
        "value": "59b32e16-3c4a-4940-899e-348c28396884"
      },
      {
        "name": "SEMAPHORE_WORKFLOW_NUMBER",
        "value": "2"
      },
      {
        "name": "SEMAPHORE_PIPELINE_ARTEFACT_ID",
        "value": "abb4fb87-309d-490a-bf0d-84972641b130"
      },
      {
        "name": "SEMAPHORE_PIPELINE_ID",
        "value": "abb4fb87-309d-490a-bf0d-84972641b130"
      },
      {
        "name": "SEMAPHORE_PIPELINE_0_ARTEFACT_ID",
        "value": "abb4fb87-309d-490a-bf0d-84972641b130"
      }
    ],
    "commands": [
      "sleep 3600"
    ]
  },
  "status": {
    "result": "STOPPED",
    "state": "FINISHED",
    "agent": {
      "ip": "88.99.26.221",
      "ports": [
        {
          "name": "ssh",
          "number": 30000
        }
      ]
    }
  }
}
```

**Example**

```
curl -i -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/api/v1alpha/jobs/:job_id
```

### Stop Job

```
POST {org_name}.semaphoreci.com/api/v1alpha/jobs/:job_id/stop
```

**Response**

```json
HTTP status: 200
```

**Example**

```
curl -i -X POST -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/api/v1alpha/jobs/:job_id/stop
```

### Get Log

```
GET https://{org_name}.semaphoreci.com/jobs/:job_id/plain_logs.json
```

**Response**

```
HTTP status: 200

BranchPage.Models.ProjectTest
  * test .find when the response is succesfull => it returns a project model instance (1.1ms)
  * test .find when the response is unsuccesfull => it returns nil (1.1ms)

BranchPage.Models.BranchTest
  * test .find when the response is succesfull => it returns a branch model instance (1.1ms)
  * test .find when the response is unsuccesfull => it returns nil (1.4ms)
```

**Example**

```
curl -i -H "Authorization: Token {api_token}" \
     https://{org_name}.semaphoreci.com/jobs/:job_id/plain_logs.json
```


