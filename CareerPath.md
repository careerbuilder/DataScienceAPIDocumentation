Career Path
==================

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [Career Path](#career-path)
    - [Summary](#summary)
    - [Finding Paths from a Carotene ID](#finding-paths-from-a-carotene-id)
        - [Request Structure](#request-structure)
        - [Response Structure](#response-structure)
    - [Finding Paths Between Two Carotene IDs](#finding-paths-between-two-carotene-ids)
        - [Request Structure](#request-structure-1)
        - [Response Structure](#response-structure-1)
    - [Versioning](#versioning)

<!-- markdown-toc end -->

## Summary

The CareerPath service returns likely paths a person can transition from a
particular position (as identified by a Carotene ID), or likely paths between
two given positions. For example, Administrative Assistant and Office Manager
are common next jobs for Receptionists.

## Finding Paths from a Carotene ID

This endpoint returns the most common paths that can be followed from a given Carotene ID.

### Request Structure

Endpoint: `/core/careerpath/carotenev3.3/paths/from/{carotene_id}`

Requests consist of:

| Field name    | Type    | Place        | Required | Notes                                                                                                                                                                                                   |
|:--------------|:--------|:-------------|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `carotene_id` | string  | query string | yes      | The Carotene ID from which to start the path searches.                                                                                                                                                  |
| `max_hops`    | integer | request body | no       | The maximum number of hops, or steps, to consider when doing path searches. Larger values would likely increase the time it takes to complete the searches. Allowed values: from 1 to 5; defaults to 3. |
| `rank_threshold`   | integer | request body | no       | A measure of how many of the top results on each hop are considered and tracked when searching for paths. Larger values would likely increase the time it takes to complete the searches. Allowed values: from 1 to 8; defaults to 5. |

Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/careerpath/carotenev3.3/paths/from/11.30597 \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"max_hops": 3,
	"rank_threshold": 5
      }'
```

### Response Structure

The response consist of a set of career path steps, or transitions from one
Carotene id to the next one, along with a score indicating how likely that step
has been found to be followed.

Example response body:

```json
{
    "career_path_data": [
        {
            "from": "11.30597",
            "to": "33.0",
            "score": 0.008
        },
        {
            "from": "11.30597",
            "to": "43.1",
            "score": 0.01935
        },
        {
            "from": "43.1",
            "to": "33.0",
            "score": 0.002341
        }
    ]
}
```

## Finding Paths Between Two Carotene IDs

This endpoint returns the most common paths that can be followed, starting from a given Carotene ID, and leading to a specified target carotene id.

### Request Structure

Endpoint: `/core/careerpath/carotenev3.3/paths/from/{carotene_id_from}/to/{carotene_id_to}`

Requests consist of:

| Field name         | Type    | Place        | Required | Notes                                                                                                                                                                                                   |
|:-------------------|:--------|:-------------|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `carotene_id_from` | string  | query string | yes      | The Carotene ID from which to start the path searches.                                                                                                                                                  |
| `carotene_id_to`   | string  | query string | yes      | The target Carotene ID that should be the ending point of the paths returned.                                                                                                                           |
| `max_hops`         | integer | request body | no       | The maximum number of hops, or steps, to consider when doing path searches. Larger values would likely increase the time it takes to complete the searches. Allowed values: from 1 to 5; defaults to 3. |
| `rank_threshold`   | integer | request body | no       | A measure of how many of the top results on each hop are considered and tracked when searching for paths. Larger values would likely increase the time it takes to complete the searches. Allowed values: from 1 to 8; defaults to 5. |

Example cURL request:

```
curl -X POST \
  https://api.careerbuilder.com/core/careerpath/carotenev3.3/paths/from/11.30597/to/33.0 \
  -H 'Accept: application/json;version=1.0' \
  -H 'Authorization: <BEARER_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
	"max_hops": 3,
	"rank_threshold": 5
      }'
```

### Response Structure

The response for this endpoint are identical to the one for [finding paths from Carotene ID](#response-structure), except that the last step in the described paths (if any) will always transition to `carotene_id_to`.

## Versioning
The current version of the service is 1.0.

Version must be specified in the Accept header. E.g. `application/json;version=1.0`.

Our general versioning strategy is available [here](/Versioning.md).
