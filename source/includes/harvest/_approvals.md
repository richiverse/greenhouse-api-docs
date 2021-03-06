# Approvals

An organization's approvals

## The approval flow object

```json
{
  "id": 49394,
  "offer_id": 45545,
  "sequential": true,
  "version": 2,
  "approval_type": "offer_candidate",
  "approval_status": "pending",
  "job_id": 12321,
  "requested_by_user_id": 543,
  "approver_groups": [
    {
      "id": 2011242,
      "approvals_required": 1,
      "created_at": "2018-03-30T19:32:04.295Z",
      "resolved_at": null,
      "priority": 1,
      "job_id": 12321,
      "offer_id": null
    },
    {
      "id": 2011241,
      "approvals_required": 1,
      "created_at": "2018-03-30T19:32:00.150Z",
      "resolved_at": "2018-03-30T19:34:17.374Z",
      "priority": 0,
      "job_id": 12321,
      "offer_id": null
    }
  ]
}
```

### Noteworthy Attributes

| Attribute | Description |
|-----------|-------------|
| id | The approval's unique identifier |
| offer_id | The specific offer for this approval. This will be empty for job approvals. |
| sequential | true or false based on whether the groups in this flow must approve sequentially |
| version | The specific version of this flow. |
| approval_type | one of 'offer_job', 'open_job', or 'offer_candidate' |
| approval_status | one of 'pending', 'approved', or 'rejected' |
| job_id | The job for which approval is configured. This always has a value. |
| requested_by_user_id | The user who requested this approval be started. |

## GET: List Approvals For Job

```shell
curl 'https://harvest.greenhouse.io/v1/jobs/{job_id}/approval_flows'
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

```json
[
    {
        "id": 49394,
        "offer_id": 45545,
        "sequential": true,
        "version": 2,
        "approval_type": "offer_candidate",
        "approval_status": "pending",
        "job_id": 12321,
        "requested_by_user_id": 543,
        "approver_groups": [
            {
                "id": 2011242,
                "approvals_required": 1,
                "created_at": "2018-03-30T19:32:04.295Z",
                "resolved_at": null,
                "priority": 1,
                "job_id": 12321,
                "offer_id": null
            },
            {
                "id": 2011241,
                "approvals_required": 1,
                "created_at": "2018-03-30T19:32:00.150Z",
                "resolved_at": "2018-03-30T19:34:17.374Z",
                "priority": 0,
                "job_id": 12321,
                "offer_id": null
            }
        ]
    },
    {
        "id": 49395,
        "offer_id": nil,
        "sequential": true,
        "version": 1,
        "approval_type": "offer_job",
        "approval_status": "pending",
        "job_id": 12321,
        "requested_by_user_id": 545,
        "approver_groups": [
            {
                "id": 2011247,
                "approvals_required": 1,
                "created_at": "2018-03-30T19:32:04.295Z",
                "resolved_at": null,
                "priority": 1,
                "job_id": 12321,
                "offer_id": null
            },
        ]
    }
]
```

List all of a job's approval flows

### HTTP Request

`GET https://harvest.greenhouse.io/v1/jobs/{id}/approval_flow`

### Querystring parameters

| Parameter | Description |
|-----------|-------------|
| id | The id of the job for which you'd like to see approvals |

<br>
[See noteworthy response attributes.] (#the-approval-flow-object)

## GET: Retrieve Approval Flow

```shell
curl 'https://harvest.greenhouse.io/v1/approval_flows/{id}'
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

```json
{
  "id": 49394,
  "offer_id": 45545,
  "sequential": true,
  "version": 2,
  "approval_type": "offer_candidate",
  "approval_status": "pending",
  "job_id": 12321,
  "requested_by_user_id": 543,
  "approver_groups": [
    {
      "id": 2011242,
      "approvals_required": 1,
      "created_at": "2018-03-30T19:32:04.295Z",
      "resolved_at": null,
      "priority": 1,
      "job_id": 12321,
      "offer_id": null
    },
    {
      "id": 2011241,
      "approvals_required": 1,
      "created_at": "2018-03-30T19:32:00.150Z",
      "resolved_at": "2018-03-30T19:34:17.374Z",
      "priority": 0,
      "job_id": 12321,
      "offer_id": null
    }
  ]
}
```

### HTTP Request

`GET https://harvest.greenhouse.io/v1/approval_flows/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the approval flow to retrieve

<br>
[See noteworthy response attributes.] (#the-approval-flow-object)

## POST: Request Approvals

```shell
curl -X POST 'https://harvest.greenhouse.io/v1/approval_flows/{id}/request_approvals'
-H "On-Behalf-Of: {greenhouse user ID}"
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

Given an approval flow, request it be started. This will change the state of the first approver group from "waiting" to "due" and mark the On-Behalf-Of user as the requesting user. This endpoint will fail with a 403 response if the On-Behalf-Of user can't see the hiring plan or can't request approvals. It will fail with a 422 response if the approval flow can't be started.

### HTTP Request

`POST https://harvest.greenhouse.io/v1/approval_flows/{id}/request_approvals`

## GET: Pending Approvals For User

```shell
curl -X GET 'https://harvest.greenhouse.io/v1/users/{id}/pending_approvals'
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

> The above command returns a JSON response, structured like this:

```
[
    {
        "id": 34564,
        "status": "waiting",
        "created_at": "2017-03-23T18:58:27.796Z",
        "resolved_at": nil,
        "request_sent_at": "2017-03-23T18:58:27.796Z",
        "reminder_sent_at": "2017-03-25T18:58:27.796Z",
        "reminders_sent": 2,
        "approver_group_id": 3432,
        "reminder_sent_by_user_id": 343,
        "hiring_plan_id": 4567,
        "offer_id": null
    },
    {
        "id": 34568,
        "status": "due",
        "created_at": "2017-04-23T18:58:27.796Z",
        "resolved_at": nil,
        "request_sent_at": "2017-04-23T18:58:27.796Z",
        "reminder_sent_at": "2017-04-25T18:58:27.796Z",
        "reminders_sent": 1,
        "approver_group_id": 3436,
        "reminder_sent_by_user_id": 343,
        "hiring_plan_id": 4568,
        "offer_id": 4534
    }
]
```

Returns all pending approvals for this user. Pending approvals are defined as an approval chain that is not approved or rejected, that this user has not already approved or rejected, in a group that has not yet determined approval or rejection.

### HTTP Request

`GET https://harvest.greenhouse.io/v1/users/{user_id}/pending_approvals`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the user whose pending approvals we want.

### Querystring Parameters

Parameter | Description
--------- | -----------
type | 'job' or 'offer', where 'offer' returns only approvals which are for a specific offer and 'job' specifically excludes approvals for specific offers. Leaving this blank will return all approvals.


### Noteworthy Attributes

| Attribute | Description |
|-----------|-------------|
| status | This is either "waiting" or "due" for pending approvals |
| approver_group_id | This is Approver Group ID used in post requests to replace this user in approval steps. |

## PUT: Replace an approver in an approver group

```shell
curl -X PUT 'https://harvest.greenhouse.io/v1/approver_groups/{id}/replace_approvers'
  -H "On-Behalf-Of: {greenhouse user ID}"
  -H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

> The above command takes a JSON request, structured like this:

```json
{
  "remove_user_id": 1443,
  "add_user_id": 5432
}
```

> The above returns a JSON success message with the following information.

```
{
  "success": "true",
  "message": "Approver group updated.",
  "approval_flow_id": 454,
  "approver_group_id": 343,
  "removed_user_id": 1443,
  "added_user_id": 5432
}
```

Removes the approver with user id given in remove_user_id and adds a new approver with the user id in add_user_id. Only approvers in unresolved groups may be replaced with this endpoint. If a group is currently "due", the new user will be notified that action is required.

### HTTP Request

`PUT https://harvest.greenhouse.io/v1/approver_groups/{approver_group_id}/replace_approvers`

### Headers

Header | Description
--------- | -----------
On-Behalf-Of | ID of the user issuing this request. Required for auditing purposes. Must have access to the approval flow.

### URL Parameters

Parameter | Description
--------- | -----------
approver_group_id | The approver group in which we are replacing an approver.

### Input JSON Parameters

Parameter | Description
--------- | -----------
remove_user_id | The user id of the approver to be removed. |
add_user_id | The user id of the new approver. Must have access to this approval flow.

### Noteworthy Output JSON Parameters

Parameter | Description
--------- | -----------
approval_flow_id | The approval flow that was changed. |
