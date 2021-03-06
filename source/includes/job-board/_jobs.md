# Jobs

## List jobs

```json
{
  "jobs": [
    {
      "id":127817,
      "internal_job_id":144381,
      "title":"Bad Cop",
      "updated_at":"2016-01-14T10:55:28-05:00",
      "location":{
        "name":"NYC"
      },
      "absolute_url":"https://boards.greenhouse.io/vaulttec/jobs/127817",
      "metadata":null
    }
  ],
  "meta": {
    "total": 1
  }
}
```

> When `?content=true`:

```json
{
  "jobs": [
    {
      "id":127817,
      "internal_job_id":144381,
      "title":"Bad Cop",
      "updated_at":"2016-01-14T10:55:28-05:00",
      "location":{
        "name":"NYC"
      },
      "absolute_url":"https://boards.greenhouse.io/vaulttec/jobs/127817",
      "metadata":null,
      "content":"&lt;p&gt;The Rule Enforcement team is dedicated to keeping all employees in line. &amp;nbsp;Rule enforcers use&amp;nbsp;various tactics such as physical intimidation, awkward pauses, and dramatic coffee sips in order to make sure everyone does what they&#39;re told.",
      "departments":[
        {
          "id":13583,
          "name":"Department of Departments",
          "parent_id":null,
          "child_ids":[
            13585
          ]
        },
        {
          "id":13585,
          "name":"Rule Enforcement",
          "parent_id":13583,
          "child_ids":[
          ]
        }
      ],
      "offices":[
        {
          "id":8304,
          "name":"East Coast",
          "location":"United States",
          "parent_id":null,
          "child_ids":[
            8787
          ]
        },
        {
          "id":8787,
          "name":"New York City",
          "location":"New York, NY, United States",
          "parent_id":8304,
          "child_ids":[
          ]
        }
      ]
    }
  ],
  "meta": {
    "total": 1
  }
}
```

Returns the list of all job posts. The `id` field contains the unique identifier for the job post, while `internal_job_id` contains the unique identifier for the job itself. Any job custom fields you have selected to be exposed in the job board API will be shown in the `metadata` attribute.

<aside class="warning">
When submitting a job application, you will use the <code>id</code> field to specify the application's target job post.
</aside>


### HTTP Request

`GET https://boards-api.greenhouse.io/v1/boards/{board_token}/jobs`

### URL Parameters

Parameter | Description
--------- | -----------
board_token | Job Board URL token

### Optional Querystring Parameters

Parameter | Description
--------- | -----------
content | If set to `true`, include the description, department and office of each job post.

## Retrieve a job

```json
{
  "id":44444,
  "title":"Product Engineer",
  "updated_at":"2013-07-02T19:39:23Z",
  "location":{
    "name":"San Francisco, CA"
  },
  "content":"This is the job description. &amp;lt;p&amp;gt;Any HTML included through the hosted job application editor will be automatically converted into corresponding HTML entitites.&amp;lt;/p&amp;gt;",
  "absolute_url":"http://your.co/careers?gh_jid=444444",
  "internal_job_id":55555,
  "location_questions": [
    {
      "label": "Location",
      "fields": [
        {
          "name": "location",
          "type": "input_text",
          "values": []
        }
      ],
      "required": true
    },
    {
      "label": "Latitude",
      "fields": [
        {
          "name": "latitude",
          "type": "input_hidden",
          "values": []
        }
      ],
      "required": true
    },
    {
      "label": "Longitude",
      "fields": [
        {
          "name": "longitude",
          "type": "input_hidden",
          "values": []
        }
      ],
      "required": true
    },
  ],
  "questions":[
    {
      "required":true,
      "label":"First Name",
      "fields":[
        {
          "name":"first_name", 
          "type":"input_text"
        }
      ]
    },
    {
      "required":true,
      "label":"Resume",
      "fields":[
        {
          "name":"resume",
          "type":"input_file"
        },
        {
          "name":"resume_text",
          "type":"textarea"
        }
      ]
    },
    {
      "required":false,
      "label":"Do you like apples?",
      "fields":[
        {
          "name":"question_2222",
          "type":"multi_value_single_select",
          "values":[
            {
              "value":0,
              "label":"No"
            },
            {
              "value":1,
              "label":"Yes"
            }
          ]
        }
      ]
    }
  ],
  "metadata":[
    {
      "id":12345,
      "name":"Field Name",
      "value_type":"text",
      "value":"Some value"
    }
  ]
}
```

> When demographic questions are enabled:

```json
{
  "id": 44444,
  "title": "Product Engineer",
  ...
  "demographic_questions": {
    "header": "Diversity and Inclusion at Acme Corp.",
    "description": "<p>Acme Corp. is dedicated to...</p>",
    "questions": [
      {
        "id": 1,
        "label": "Favorite Color",
        "required": false,
        "answer_options": [
          {
            "id": 100,
            "label": "Red",
            "free_form": false
          },
          {
            "id": 101,
            "label": "Green",
            "free_form": false
          },
          {
            "id": 102,
            "label": "Blue",
            "free_form": false
          },
          {
            "id": 102,
            "label": "Prefer to Type My Own",
            "free_form": true
          }
        ]
      }
    ]
  }
}
```

Returns a job post. Setting the questions querystring parameter to
`"true"` will include the list of job application fields; these fields
can be used to dynamically construct your own job application form.

Any job custom fields you have selected to be exposed in the job board API will be shown in the `metadata` attribute.

### HTTP Request

`GET https://boards-api.greenhouse.io/v1/boards/{board_token}/jobs/{job_id}`

### URL Parameters

Parameter | Description
--------- | -----------
board_token | Job Board URL token
job_id | ID of the job to retrieve

### Querystring Parameters

Parameter | Description
--------- | -----------
*questions | If set to `true`, include additional fields in the response:<br><br>- `questions`: An array of custom questions defined for this job post<br>- `location_questions`: An array of questions used to capture the applicant's location (included only if the job post has the location configured as "optional" or "required")<br>- `compliance`: An array of questions used by government contractors to capture applicant information to comply with EEOC regulations (included only if the job post has EEOC questions enabled)<br>- `demographic_questions`: An object containing demographic questions and related information (included only if your organization has Greenhouse Inclusion, and the job post has demographic questions enabled)

### Questions / Location Questions / Compliance

Possible field types:

| Type | How to represent |
|------|------------------|
| input_file | Represent with an input of type file |
| input_text | Represent with an input of type text |
| input_hidden | Represent with an input of type hidden |
| textarea | Represent with a textarea |
| multi_value_single_select | Can be represented as either a set of radio buttons or a select
| multi_value_multi_select | Can be represented as either a set of checkboxes or a multi-select

Please note that it is possible for multiple fields to be aggregated beneath a single question. The "Resume" field is a prime example, with both an input_file and textarea type accepted. If marked as required, then we expect at least one of these fields to contain a valid value when your form is submitted to the [application submission](#applications) endpoint.

### Demographic Questions

For organizations using Greenhouse Inclusion, the response may contain demographic questions. Each question contains an array of answer options that may be rendered as checkboxes or a multi-select. The candidate may select zero, one, or more answer options per question. If an answer option is selected that has `free_form` set to `true`, the candidate must be allowed to type a free-form response. The free-form response is optional.