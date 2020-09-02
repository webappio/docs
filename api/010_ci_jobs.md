# CI Jobs

## Layerfile runner objects
Each time you look at the dashboard for a run, you're looking at a collection of layerfile runners.
Each layerfile runner is created by a specific Layerfile, or by a Layerfile being retried.

*CI jobs are a collection of Layerfile runners!*

## Example schema

```json
{
    "repository_name": "layerci-example",
    "id": 1,
    "status": "SEE_RUNNERS",
    "status_change_time": "2000-01-01 01:23:45.123456 +0000 UTC",
    "calculated_status": "SUCCEEDED",
    "commit_sha": "9abc2ac68d52afe1a5a3fbc724d031af5a397204",
    "branch": "master",
    "merge_branch": "master",
    "run_start_reason": "Manual run created by API",
    "run_start_reason_url": ""
}
```

## status vs. calculated_status

CI jobs themselves maintain an internal status which is either:

- `NEW` for pending jobs
- `ERROR` for jobs that have had some error while initializing (e.g., invalid layerfiles)
- `STARTING_RUNNERS` for jobs which are in the process of starting runners  
- `SEE_RUNNERS` for jobs which have started runners. This status means you must look at the runners themselves to understand the status.


CI jobs also contain a status which is the calculated status of the whole job, looking at the runners.
The calculated status is usually what you'll use to determine the status of an entire run.

The `calculated_status` of a CI job is one of the following:

- `NEW` for pending jobs
- `JOB_ERROR` for jobs that have had an internal while initializing (e.g., internal errors in Layer occurred when starting this run)
- `BAD_LAYERFILE` for jobs that had an invalid Layerfile
- `STARTING_RUNNERS` means that runners are in the process of being started for this job.
- `FAILURE` means that at least one runner failed (some instruction for that runner is red) - again, there can be multiple Layerfiles per repository and if either fails.
- `RUNNER_CANCELLED` means that a user manually cancelled at least one runner
- `RUNNER_ERROR` means that at least one runner had an internal error while running
- `RUNNING` means that the runner itself is running
- `SUCCEEDED` means that the runner succeeded


## Get most recent CI job for a repository matching the given filters

It's often useful to get the most recent CI job given specific filters. This API endpoint allows for that.

Possible filters are:

- `calculated_status=SUCCEEDED` (calculated status is success, any other value is also usable here)
- `branch=master` (branch is only set for commits actually made to the branch, not for merge requests opened against that branch)
- `status=ERROR` (see status vs calculated status section above)
- `done=true` (the calculated status is neither NEW, STARTING_RUNNERS, nor RUNNING)
- `failed=true` (the calculated status is JOB_ERROR, BAD_LAYERFILE, FAILURE, or RUNNER_ERROR)

You can include multiple filters to require multiple conditions to be true.

<language-tabs>

```ruby
require 'faraday'
require 'json'

# get the most recent failing jobs
res = Faraday.get 'https://layerci.com/api/v1/run/repo_name/latest?calculated_status=FAILURE&layertoken=(your api key)'

res = JSON.parse(res.body)
```

```python
import requests

# get the most recent failing job
my_token="(your API key)"
res = requests.get(
    f'https://layerci.com/api/v1/jobs/github/repo_owner/repo_name/latest?done=true&layertoken={my_token}', 
).json()
```

```shell
# get the most recent failing job
my_token="(your API key)"
curl "https://layerci.com/api/v1/run/repo_name/latest?calculated_status=FAILURE&layertoken=${my_token}"
```

```javascript
let apiKey="(your API key)"
fetch(
    'https://layerci.com/api/v1/run/repo_name/latest?calculated_status=FAILURE&layertoken='+apiKey,
).then(res => res.json()).then(json => console.log(json))
```

</language-tabs>

Output:


```json
{
    "status": "ok",
    "job": {
        "repository_name": "layerci-example",
        "id": 3,
        "status": "SEE_RUNNERS",
        "status_change_time": "2000-01-01 01:23:45.123456 +0000 UTC",
        "calculated_status": "SUCCEEDED",
        "commit_sha": "9abc2ac68d52afe1a5a3fbc724d031af5a397204",
        "branch": "master",
        "merge_branch": "master",
        "run_start_reason": "Manual run created by API",
        "run_start_reason_url": ""
    }
}
```

`status` can be one of `"ok"` or `"error"`. If the latter exists, an `"error"` value will be included with an explanation.

## Create a CI job for a given repository

Sometimes it's useful to manually start LayerCI runs (for, e.g., deploying)
This endpoint lets you do that.

The input to the POST is optional, but can contain:

- `branch=master` to check out the "master" branch. If omitted, we use "master"
- `ref=9abc2ac68d52afe1a5a3fbc724d031af5a397204` to check out a specific commit. If omitted, we use "origin/master"
- `accept_buttons=true` to accept any BUTTON instructions in the job (i.e., to deploy automatically)
- `skip_no_further_than=(CHECKPOINT name)` to avoid skipping certain actions (e.g., deployment) - this lets you use Layer as a cheap lambda.

<language-tabs>
```ruby
require 'faraday'
require 'json'

res = Faraday.post(
    'https://layerci.com/api/v1/run/repo_name?layertoken=(your api key)',
    {
        branch: 'master',
        ref: '9abc2ac68d52afe1a5a3fbc724d031af5a397204',
        accept_buttons: 'true',
        skip_no_further_than: 'run-and-deploy',
    }.to_json
)

res = JSON.parse(res.body)
```

```python
import requests

my_token="(your API key)"
res = requests.post(
    f'https://layerci.com/api/v1/run/repo_name?layertoken={my_token}',
    json={
        'branch': 'master',
        'ref': '9abc2ac68d52afe1a5a3fbc724d031af5a397204',
        'accept_buttons': 'true',
        'skip_no_further_than': 'run-and-deploy',
    },
).json()
```

```shell
my_token="(your API key)"
curl -X POST \
    -H 'Content-Type: application/json' \
    -d '{"branch": "master", \
         "ref": "9abc2ac68d52afe1a5a3fbc724d031af5a397204", \
         "accept_buttons": "true", \
         "skip_no_further_than": "run-and-deploy"
        }' \
    "https://layerci.com/api/v1/run/repo_name?layertoken=${my_token}"
```

```javascript
let apiKey="(your API key)"
fetch(
    'https://layerci.com/api/v1/run/repo_name?layertoken='+apiKey,
    {
        method: "POST",
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            'branch': 'master',
            'ref': '9abc2ac68d52afe1a5a3fbc724d031af5a397204',
            'accept_buttons': 'true',
            'skip_no_further_than': 'run-and-deploy',
        }),
    }
).then(res => res.json()).then(json => console.log(json))
```
</language-tabs>

Output:

```json
{
    "status": "ok",
    "job": {
        "repository_name": "layerci-example",
        "id": 1,
        "status": "NEW",
        "status_change_time": "2020-05-05 05:11:44.17955 +0000 UTC",
        "calculated_status": "NEW",
        "commit_sha": "9abc2ac68d52afe1a5a3fbc724d031af5a397204",
        "branch": "master",
        "merge_branch": "master",
        "run_start_reason": "Manual run created by API",
        "run_start_reason_url": ""
    },
    "url": "http://layerci.com/jobs/github/distributed-containers-inc/layerci-example/1"
}

```

`status` can be one of `"ok"` or `"error"`. If the latter exists, an `"error"` value will be included with an explanation.
