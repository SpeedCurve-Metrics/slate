---
title: SpeedCurve v1 API

language_tabs:
  - curl

toc_footers:
  - <a href='https://speedcurve.com/'>Sign Up for an API Key</a>

search: true
---

# SpeedCurve v1 API

The SpeedCurve v1 API is currently being actively developed. Please log any feedback, endpoint requests, errors or issues you find with [SpeedCurve support.](https://github.com/SpeedCurve-Metrics/SpeedCurve/issues)

Refer to this documentation for the latest changes.

[Change Log](#change-log)

## Getting Started

The SpeedCurve API is based on REST: it is comprised of resources with predictable urls and utilises standard HTTP features (like HTTP Basic Authentication, Response Codes and Methods). All requests, including errors, return JSON.

## API Endpoint

The base URI for the V1 API is:

`https://api.speedcurve.com/v1`

## API Key & Authentication

### API key

Your API key is available on the <b>Admin > Teams</b> page.

Please check our [support article](https://support.speedcurve.com/en/articles/415403-synthetic-api) for more details on managing API key.

> Basic Authentication

```shell
curl "https://api.speedcurve.com/v1/sites"
-u your-api-key:x
```

### Basic Authentication

[Basic Authentication over HTTPS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#basic_authentication_scheme) is used to authenticate with the API.
Your basic authentication username is your API key and the password can be set to anything.
In our examples the API key is `your-api-key` and the password is `x`.

The easiest way to authenticate in the API is sending the following HTTP header with your request:  
```
Authorization: Basic your-api-key
```

# Sites

## Get all sites

> Request

```shell
curl "https://api.speedcurve.com/v1/sites"
-u your-api-key:x
```

> Response

```json
{
  "sites": [
    {
      "site_id": 123,
      "name": "Guardian",
      "checks_scheduled": 1095,
      "urls":[
        {
           "url_id": 123,
           "url": "http://www.theguardian.com/"
        }
      ]
    }
  ]
}
```

Retrieves all sites and monitored URL's for an account/team and the median tests for a site across all URL's and regions.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
median | 0 | Return median tests across all URLs for a site. Off by default. (0 = off, 1 = on)
days | 14 | Number of days of median tests to return. (Max: 365)

### HTTP Request

`GET https://api.speedcurve.com/v1/sites`

### Response

Attribute | Description
--------- | -----------
sites | List of sites associated with this account/team.
site_id | ID of site which can then be used for deploys or adding notes.
name | Name of the site.
checks_scheduled | Number of scheduled checks this site uses per month.
urls | List of URL's associated with this site.
url_id | ID of URL.
url | URL associated with this site.
median | Median test result for URLs grouped by site. Median is selected based on Speed Index which is the default for the SpeedCurve dashboards.
## Get site details and settings

> Request

```shell
curl "https://api.speedcurve.com/v1/sites/299"
-u your-api-key:x
```

> Response

```json
{
  "site": {
    "site_id": 299,
    "name": "Guardian Beta",
    "urls": [
      {
        "url_id": 906,
        "label": "home",
        "url": "http://www.theguardian.com/uk?view=mobile"
      },
      {
        "url_id": 907,
        "label": "section",
        "url": "http://www.theguardian.com/football?view=mobile"
      }
    ],
    "regions": [
      {
        "region_id": "eu-west-1"
      },
      {
        "region_id": "ap-southeast-2"
      }
    ],
    "browsers": [
      {
        "browser_id": "apple-ipad"
      },
      {
        "browser_id": "apple-iphone-5"
      },
      {
        "browser_id": "chrome"
      },
      {
        "browser_id": "internet-explorer-11"
      }
    ]
  }
}
```

Retrieves details and settings for a specific site. Setting details like region_id and browser_id can then be used to filter other endpoints.

### HTTP Request

`GET https://api.speedcurve.com/v1/sites/{site_id}`

### Response

Attribute | Description
--------- | -----------
site_id | ID of site which can then be used for deploys or adding notes.
name | Name of the site.
urls | List of URLs associated with this site.
url_id | ID of URL.
label | Label for the URL.
url | URL associated with this site.
regions | List of regions that the site is being monitored from.
region_id | ID of region.
browsers | List of browsers that the site is being tested with.
browser_id | ID of browser.

## Delete a site

> Request

```shell
curl "https://api.speedcurve.com/v1/sites/299"
-u your-api-key:x
--request DELETE
```

Deletes a site and all settings, test history and URLs associated with it.

### HTTP Request

`DELETE https://api.speedcurve.com/v1/sites/{site_id}`

# Urls

## Get all URLs

> Request

```shell
curl "https://api.speedcurve.com/v1/urls"
-u your-api-key:x
```

> Response

```json
{
  "sites": [
    {
      "site_id": 299,
      "name": "Guardian Beta",
      "urls": [
        {
          "url_id": 906,
          "url": "http://www.theguardian.com/uk?view=mobile",
          "label": "home",
          "latest_tests": [
            {
              "test_id": "200308_KG_e8fa65d7621e501e02aed7e3cae082e4",
              "timestamp": "1583697600",
              "region_id": "ap-southeast-2",
              "browser_id": "chrome-fibre"
             }
          ]
        }
      ]
    }
  ]
}
```

Retrieves the metadata for all URLs across your sites.

### HTTP Request

`GET https://api.speedcurve.com/v1/urls`

### Response

Attribute | Description
--------- | -----------
sites | List of sites in your settings
site_id | ID for a site
name | Name of site.
urls | List of URLs associated with this site.
url_id | ID of URL.
label | Label for the URL.
url | URL associated with this site.
latest_tests | Most recent set of tests for this URL.


## Get all tests for a URL

> Request

```shell
curl "https://api.speedcurve.com/v1/urls/536"
-u your-api-key:x
```

> Response

```json
{
  "site_id": 123,
  "site_name": "The Guardian",
  "url_id": 123,
  "url": "http://www.theguardian.com/",
  "label": "Home",
  "script": "",
  "username": null,
  "password": null,
  "tests":[
    {
      "test_id":"140325_WS_2V3",
      "url":"http://www.theguardian.com/",
      "timezone": "Pacific/Auckland",
      "day":"2014-03-26",
      "timestamp": 1448147927,
      "region": "us-west-1",
      "status": 0,
      "run": 2,
      "browser": "Google Chrome",
      "browser_version": "46.0.2490.86",
      "viewport_width": 768,
      "viewport_height": 1024,
      "pixel_ratio": 2,
      "bandwidth_down": 5000,
      "bandwidth_up": 1000,
      "bandwidth_latency": 28,
      "bandwidth_packet_loss_rate": 0,
      "byte": 130,
      "dom_content_loaded": 420,
      "render": 421,
      "visually_complete": 420,
      "first_cpu_idle": 308,
      "time_to_interactive": 308,
      "first_contentful_paint": 1200,
      "largest_contentful_paint": 1400,
      "first_meaningful_paint": 1400,
      "cumulative_layout_shift": 0.010246000676623,
      "dom": 417,
      "loaded": 423,
      "size": 33695,
      "image_saving": 0,
      "requests": 6,
      "speedindex": 400,
      "html_requests": 1,
      "html_size": 6041,
      "css_requests": 2,
      "css_size": 6977,
      "js_requests": 2,
      "js_size": 41261,
      "image_requests": 3,
      "image_size": 27676,
      "font_requests": 0,
      "font_size": 0,
      "text_requests": 0,
      "text_size": 0,
      "flash_requests": 0,
      "flash_size": 0,
      "other_requests": 0,
      "other_size": 0,
      "first_party_requests": 60,
      "first_party_size": 88203,
      "first_party_cpu": 523,
      "first_party_long_tasks": 78,
      "first_party_num_long_tasks": 1,
      "first_party_longest_task": 78,
      "third_party_requests": 4,
      "third_party_size": 81203,
      "third_party_cpu": 670,
      "third_party_long_tasks": 209,
      "third_party_num_long_tasks": 3,
      "third_party_longest_task": 74,
      "blocking_scripts": 5,
      "blocking_css": 1,
      "har": "https://wpt.speed...40325_WS_2V3",
      "screen": "https://wpt.speed...screen.jpg",
      "custom_metrics": [
        {
          "mark": "speedcurve_js_top",
          "value": "1240"
        }
      ],
      "hero_metrics": [
        {
          "hero": "bg_img",
          "value": "8800"
        },
        {
          "hero": "biggest_img",
          "value": "6400"
        }
      ],
      "first_painted_hero": 13800,
      "last_painted_hero": 21400,
      "lighthouse_performance": 62,
      "lighthouse_pwa": 31,
      "lighthouse_accessibility": 69,
      "lighthouse_best_practice": 60,
      "lighthouse_seo": 100,
      "total_blocking_time": 223,
      "long_tasks": 286,
      "num_long_tasks": 4,
      "longest_task": 78,
      "cpu_idle": 4073,
      "cpu_scripting": 55,
      "cpu_layout": 106,
      "cpu_painting": 9,
      "cpu_render_idle": 943,
      "cpu_render_scripting": 2,
      "cpu_render_layout": 51,
      "cpu_render_painting": 1,
      "cpu_pageload_idle": 3228,
      "cpu_pageload_scripting": 45,
      "cpu_pageload_layout": 104,
      "cpu_pageload_painting": 6,
      "dom_elements": 1243
    }
  ]
}
```

Retrieves all the metadata for tests of a specific monitored URL.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
browser | All | Filter tests to a specific browser. Get browser_id from sites/{site_id} endpoint.
region | All | Filter tests to a specific region. Get region_id from sites/{site_id} endpoint.
days | 7 | Number of days of tests (Max: 365).

### HTTP Request

`GET https://api.speedcurve.com/v1/urls/{url_id}?days=30&browser=chrome&region=us-west-1`

### Response

Attribute | Description
--------- | -----------
site_id | ID of the site this URL belongs to.
site_name | Name of the site this URL belongs to.
url_id | ID of URL.
url | Monitored URL.
label | Page label for the URL.
script | WebPageTest script to run for this URL.
username | Basic authentication username for the browser to use when testing the page.
password | Basic authentication password for the browser to use when testing the page. Note this is stored in plain text so you should use a throw away account when testing logged in pages.
tests | List of most recent associated tests.
test_id | WebPageTest ID of the median test result.
url | Monitored URL.
timezone | Timezone of the account/team.
day | Day of the median test result.
timestamp | Unix timestamp of test result.
region | AWS region that test was run in.
status | Test result status. 0 = success and everything else is an error.
run | Number of WebPageTest run that is used for this test. Median is based on Speed Index.
browser | Browser name.
browser_version | Browser version number.
viewport_width | Width of the browser.
viewport_height | Height of the browser.
pixel_ratio | Device pixel ratio of the browser.
bandwidth_down | Connection download speed (Kbps).
bandwidth_up | Connection upload speed (Kbps).
bandwidth_latency | Connection latency (ms).
bandwidth_packet_loss_rate | Connection packet rate loss (%).
byte |  Time to first byte in ms of the median result. Also known as Backend (ms)
dom_content_loaded | Time when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading. (ms)
render | Time it start render which is extracted from the video of the page loading and is the first moment that a user see content appear on screen. (ms)
visually_complete | Time to the viewport being visually complete. (ms)
first_cpu_idle | Time to the page being interactive for the first time. (ms)
time_to_interactive | Time to the page being interactive and the CPU being quiet for at least 5s. (ms)
first_contentful_paint | Time to first contentful paint. (ms)
largest_contentful_paint | Time to largest contentful paint. (ms)
first_meaningful_paint | Time to first meaningful paint. (ms)
cumulative_layout_shift | Measures the instability of content by summing shift scores across layout shifts that don't occur within 500ms of user input.
dom | Time to DOM complete browser event. Also known as Page Load (ms)
loaded | Fully loaded time in ms of the median result. (ms)
size | Total size of all assets. (bytes)
image_saving | Number of bytes that could be saved if all images were compressed correctly. (bytes)
requests | Total number of asset requested and downloaded by the page.
speedindex | WebPagetest user experience score.
*_requests | Total number of asset requested for a specific content type.
*_size | Total size of assets for a specific content type. (bytes)
*_cpu | Total CPU usage of scripts. (ms)
*_long_tasks | Total time of any long tasks. A long task is any task over 50ms. (ms)
*_num_long_tasks | Total number of long tasks.
*_longest_tasks | The duration of the longest individual task. (ms)
blocking_scripts | Number of synchronous scripts that block rendering.
blocking_css | Number of CSS requests that block rendering.
har | URL to downloadable HAR file.
screen | URL to screen shot of loaded page.
custom_metrics | Any user timing marks and measures found on the URL.
hero_metrics | Rendering times for different elements in the viewport.
first_painted_hero | First of the hero times.
last_painted_hero | Last of the hero times.
lighthouse_performance | Google Lighthouse performance score.
lighthouse_pwa | Google Lighthouse PWA score.
lighthouse_accessibility | Google Lighthouse Accessibility score.
lighthouse_best_practice | Google Lighthouse Best Practice score.
lighthouse_seo | Google Lighthouse SEO score.
total_blocking_time | Total time of long tasks between First Contentful Paint and Time To Interactive. The first 50ms of each long task is not included. (ms)
cpu_idle | Total time the CPU was idle.
cpu_scripting | Total time the CPU was busy performing scripting-related tasks.
cpu_layout | Total time the CPU was busy performing layout-related tasks.
cpu_painting | Total time the CPU was busy performing painting-related tasks.
cpu_render_* | Amount of time before Start Render where the CPU was busy.
cpu_pageload_* | Amount of time before Page Load where the CPU was busy.
dom_elements | Number of DOM elements.

## Create a URL

> Request

```shell
curl "https://api.speedcurve.com/v1/urls"
-u your-api-key:x
--request POST
--data site_id=123
--data url=https://example.com/
--data label=Home
```

> Response

```json
{
  "status": "success",
  "message": "URL created",
  "url_id": 123
}
```

Creates a new URL in a site.

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
site_id | | The site that the URL will be created in
url | | The URL string
label | | (Optional) A label for identifying the URL
script | | (Optional) WebPageTest script used when running tests for this URL
username | | (Optional) HTTP basic authentication username
password | | (Optional) HTTP basic authentication password

### HTTP Request

`POST https://api.speedcurve.com/v1/urls`

## Update a URL

> Request

```shell
curl "https://api.speedcurve.com/v1/urls/123"
-u your-api-key:x
--request PATCH
--data label="New Label"
--data script=""
```

> Response

```json
{
  "status": "success",
  "message": "URL updated",
  "url_id": 123
}
```

Updates an existing URL. Only the parameters specified will be updated.

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | | (Optional) The URL string
label | | (Optional) A label for identifying the URL
script | | (Optional) WebPageTest script used when running tests for this URL
username | | (Optional) HTTP basic authentication username
password | | (Optional) HTTP basic authentication password

### HTTP Request

`PATCH https://api.speedcurve.com/v1/urls/{url_id}`

## Delete a URL

> Request

```shell
curl "https://api.speedcurve.com/v1/urls/906"
-u your-api-key:x
--request DELETE
```

> Response

```json
{
  "status": "success",
  "message": "URL deleted."
}
```

Deletes a URL and any test history associated with it.

### HTTP Request

`DELETE https://api.speedcurve.com/v1/urls/{url_id}`

# Tests

## Get a test

> Request

```shell
curl "https://api.speedcurve.com/v1/tests/140317_BA_3W8"
-u your-api-key:x
```

> Response

```json
{
  "test_id":"140325_WS_2V3",
  "url":"http://www.theguardian.com/",
  "timezone": "Pacific/Auckland",
  "day":"2014-03-26",
  "timestamp": 1448147927,
  "region": "us-west-1",
  "status": 0,
  "run": 2,
  "browser": "Google Chrome",
  "browser_version": "46.0.2490.86",
  "viewport_width": 768,
  "viewport_height": 1024,
  "pixel_ratio": 2,
  "bandwidth_down": 5000,
  "bandwidth_up": 1000,
  "bandwidth_latency": 28,
  "bandwidth_packet_loss_rate": 0,
  "byte": 130,
  "dom_content_loaded": 420,
  "render": 421,
  "visually_complete": 420,
  "first_cpu_idle": 308,
  "time_to_interactive": 308,
  "first_contentful_paint": 1200,
  "largest_contentful_paint": 1400,
  "first_meaningful_paint": 1400,
  "cumulative_layout_shift": 0.010246000676623,
  "dom": 417,
  "loaded": 423,
  "size": 33695,
  "image_saving": 0,
  "requests": 6,
  "speedindex": 400,
  "html_requests": 1,
  "html_size": 6041,
  "css_requests": 2,
  "css_size": 6977,
  "js_requests": 2,
  "js_size": 41261,
  "image_requests": 3,
  "image_size": 27676,
  "font_requests": 0,
  "font_size": 0,
  "text_requests": 0,
  "text_size": 0,
  "flash_requests": 0,
  "flash_size": 0,
  "other_requests": 0,
  "other_size": 0,
  "first_party_requests": 60,
  "first_party_size": 88203,
  "first_party_cpu": 523,
  "first_party_long_tasks": 78,
  "first_party_num_long_tasks": 1,
  "first_party_longest_task": 78,
  "third_party_requests": 4,
  "third_party_size": 81203,
  "third_party_cpu": 670,
  "third_party_long_tasks": 209,
  "third_party_num_long_tasks": 3,
  "third_party_longest_task": 74,
  "blocking_scripts": 5,
  "blocking_css": 1,
  "har": "https://wpt.speed...40325_WS_2V3",
  "screen": "https://wpt.speed...screen.jpg",
  "custom_metrics": [
    {
      "mark": "speedcurve_js_top",
      "value": "1240"
    }
  ],
  "hero_metrics": [
    {
      "hero": "bg_img",
      "value": "8800"
    },
    {
      "hero": "biggest_img",
      "value": "6400"
    }
  ],
  "first_painted_hero": 13800,
  "last_painted_hero": 21400,
  "lighthouse_performance": 62,
  "lighthouse_pwa": 31,
  "lighthouse_accessibility": 69,
  "lighthouse_best_practice": 60,
  "lighthouse_seo": 100,
  "total_blocking_time": 223,
  "long_tasks": 286,
  "num_long_tasks": 4,
  "longest_task": 78,
  "cpu_idle": 4073,
  "cpu_scripting": 55,
  "cpu_layout": 106,
  "cpu_painting": 9,
  "cpu_render_idle": 943,
  "cpu_render_scripting": 2,
  "cpu_render_layout": 51,
  "cpu_render_painting": 1,
  "cpu_pageload_idle": 3228,
  "cpu_pageload_scripting": 45,
  "cpu_pageload_layout": 104,
  "cpu_pageload_painting": 6,
  "dom_elements": 1243
}
```

Retrieves all the details available for a specific test.

### HTTP Request

`GET https://api.speedcurve.com/v1/tests/{test_id}`

### Response

Attribute | Description
--------- | -----------
test_id | WebPageTest ID of the median test result.
url | Monitored URL.
timezone | Timezone of the account/team.
day | Day of the median test result.
timestamp | Unix timestamp of test result.
region | AWS region that test was run in.
status | Test result status. 0 = success and everything else is an error.
run | Number of WebPageTest run that is used for this test. Median is based on Speed Index.
browser | Browser name.
browser_version | Browser version number.
viewport_width | Width of the browser.
viewport_height | Height of the browser.
pixel_ratio | Device pixel ratio of the browser.
bandwidth_down | Connection download speed (Kbps).
bandwidth_up | Connection upload speed (Kbps).
bandwidth_latency | Connection latency (ms).
bandwidth_packet_loss_rate | Connection packet rate loss (%).
byte |  Time to first byte in ms of the median result. Also known as Backend (ms)
dom_content_loaded | Time when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading. (ms)
render | Time it start render which is extracted from the video of the page loading and is the first moment that a user see content appear on screen. (ms)
visually_complete | Time to the viewport being visually complete. (ms)
first_cpu_idle | Time to the page being interactive for the first time. (ms)
time_to_interactive | Time to the page being interactive and the CPU being quiet for at least 5s. (ms)
first_contentful_paint | Time to first contentful paint. (ms)
largest_contentful_paint | Time to largest contentful paint. (ms)
first_meaningful_paint | Time to first meaningful paint. (ms)
cumulative_layout_shift | Measures the instability of content by summing shift scores across layout shifts that don't occur within 500ms of user input.
dom | Time to DOM complete browser event. Also known as Page Load (ms)
loaded | Fully loaded time in ms of the median result. (ms)
size | Total size of all assets. (bytes)
image_saving | Number of bytes that could be saved if all images were compressed correctly. (bytes)
requests | Total number of asset requested and downloaded by the page.
speedindex | WebPagetest user experience score.
*_requests | Total number of asset requested for a specific content type.
*_size | Total size of assets for a specific content type. (bytes)
*_cpu | Total CPU usage of scripts. (ms)
*_long_tasks | Total time of any long tasks. A long task is any task over 50ms. (ms)
*_num_long_tasks | Total number of long tasks.
*_longest_tasks | The duration of the longest individual task. (ms)
blocking_scripts | Number of synchronous scripts that block rendering.
blocking_css | Number of CSS requests that block rendering.
har | URL to downloadable HAR file.
screen | URL to screen shot of loaded page.
custom_metrics | Any user timing marks and measures found on the URL.
hero_metrics | Rendering times for different elements in the viewport.
first_painted_hero | First of the hero times.
last_painted_hero | Last of the hero times.
lighthouse_performance | Google Lighthouse performance score.
lighthouse_pwa | Google Lighthouse PWA score.
lighthouse_accessibility | Google Lighthouse Accessibility score.
lighthouse_best_practice | Google Lighthouse Best Practice score.
lighthouse_seo | Google Lighthouse SEO score.
total_blocking_time | Total time of long tasks between First Contentful Paint and Time To Interactive. The first 50ms of each long task is not included. (ms)
cpu_idle | Total time the CPU was idle.
cpu_scripting | Total time the CPU was busy performing scripting-related tasks.
cpu_layout | Total time the CPU was busy performing layout-related tasks.
cpu_painting | Total time the CPU was busy performing painting-related tasks.
cpu_render_* | Amount of time before Start Render where the CPU was busy.
cpu_pageload_* | Amount of time before Page Load where the CPU was busy.
dom_elements | Number of DOM elements.

## Delete a test

> Request

```shell
curl "https://api.speedcurve.com/v1/tests/170326...1041100518"
-u your-api-key:x
--request DELETE
```
> Response

```json
{
  "status": "success",
  "message": "URL deleted."
}
```

Deletes a single test result.

### HTTP Request

`DELETE https://api.speedcurve.com/v1/tests/{test_id}`

# Notes

## Get all notes
> Request

```shell
curl "https://api.speedcurve.com/v1/notes"
-u your-api-key:x
```

> Response

```json
{
  "notes": [
    {
      "note_id": 1912,
      "site_id": 123,
      "timestamp": 1420745970,
      "note": "I'm a note!",
      "detail": "I'm a longer note with way more detail."
    },
    {
      "note_id": 1911,
      "site_id": 123,
      "timestamp": 1420745966,
      "note": "New responsive site goes live!",
      "detail": ""
    }
  ]
}
```
Gets all the notes for the main site in a users account.

### Query Parameters

None.

### HTTP Request

`GET https://api.speedcurve.com/v1/notes`

### Response

Attribute | Description
--------- | -----------
note_id | ID of the note.
site_id | ID of site that this note is associated with.
timestamp | UTC Unix timestamp.
note | Short note content to display on charts.
detail | Longer note content to provide more context when a user interacts with a note.

## Add a note

> Request

```shell
curl "https://api.speedcurve.com/v1/notes"
-u your-api-key:x
--request POST
--data timestamp=now
--data site_id=123
--data note=I%27m%20a%20note!
--data detail=I%27m%20a%20longer%20note%20with%20way%20more%20detail.
```

> Response

```json
{
  "note_id": 1912,
  "site_id": 123,
  "timestamp": 1420745970,
  "note": "I'm a note!",
  "detail": "I'm a longer note with way more detail."
}
```


Add a note to one of the sites within your account/team.

### HTTP Request

`POST https://api.speedcurve.com/v1/notes`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
timestamp | now | Either a UTC Unix Timestamp or "now". Options: (now, unix timestamp).
site_id |  | ID of site to add note to.
note |  | Short URL encoded note to display globally across all charts for the main site. (Max: 255 characters).
detail |  | Optional note detail to display if people want more context.

### Response

Attribute | Description
--------- | -----------
note_id | ID of the note.
site_id | ID of associated site.
timestamp | UTC Unix timestamp.
note | Short note content to display on charts.
detail | Longer note content to provide more context when a user interacts with a note.

## Delete a note

> Request

```shell
curl "https://api.speedcurve.com/v1/notes/1912"
-u your-api-key:x
--request DELETE
```
> Response

```json
{
  "status": "success",
  "message": "Note deleted."
}
```

Delete a note.

### HTTP Request

`DELETE https://api.speedcurve.com/v1/notes/{note_id}`

# Deploys

## All deploys
> Request

```shell
curl "https://api.speedcurve.com/v1/deploys"
-u your-api-key:x
```

> Response

```json
{
  "deploys": [
    {
      "deploy_id": 101809,
      "site_id": 29142,
      "timestamp": 1534917858,
      "note": "JS Blocking",
      "detail": "Release 14.2 with JS blocking"
    },
    {
      "deploy_id": 100891,
      "site_id": 29169,
      "timestamp": 153483345,
      "note": "All mobile locations",
      "detail": ""
    }
  ]
}
```
Gets all deployments.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
site_id | All | Filter deploys to a specific site. Get site_id from sites/ endpoint.

### HTTP Request

`GET https://api.speedcurve.com/v1/deploys`

### Response

Attribute | Description
--------- | -----------
deploy_id | ID of the deployment.
site_id | ID of site the deploy has been added to.
timestamp | Unix timestamp of deploy.
note | Short note content to display on charts.
detail | Longer note content to provide more context when a user interacts with a note.

## Latest deploy
> Request

```shell
curl "https://api.speedcurve.com/v1/deploys/latest"
-u your-api-key:x
```

> Response

```json
{
  "deploy_id": 10,
  "site_id": 123,
  "timestamp": 1534917858,
  "status": "completed",
  "tests-completed": [
    {
      "test_id": "150217_Z2_CJ7",
      "url_id": 934,
      "browser": "safari",
      "region": "us-west-1"
    },{
      "test_id": "150217_6X_CJ8",
      "url_id": 934,
      "browser": "ie 11",
      "region": "us-west-1"
    }
  ],
  "tests-remaining": [],
  "note_id": 890282,
  "note": "#465 Responsive",
  "detail": "Deploy #465, awesome new responsive site live!"
}
```
Gets details and status of testing for the latest deployment.

### Query Parameters

None.

### HTTP Request

`GET https://api.speedcurve.com/v1/deploys/latest`

### Response

Attribute | Description
--------- | -----------
deploy_id | ID of the deployment.
site_id | ID of site the deploy has been added to.
timestamp | Unix timestamp of deploy.
status | Status of testing.
test-remaining | Remaining tests waiting to be tested.
test-completed | Tests completed.
note_id | ID of the related note for this deploy.
note | Short note content to display on charts.
detail | Longer note content to provide more context when a user interacts with a note.

## Add a deploy

> Request

```shell
curl "https://api.speedcurve.com/v1/deploys"
-u your-api-key:x
--request POST
--data site_id=123
--data url_id=1234
--data note=I%27m%20a%20note!
--data detail=I%27m%20a%20longer%20note%20with%20way%20more%20detail.
```

> Response

```json
{
  "deploy_id": 16,
  "site_id": 123,
  "timestamp": 1534917858,
  "status": "success",
  "message": "A deployment has been added",
  "info": [
    {
      "test_id": "150217_Z2_CJ7",
      "browser": "safari",
      "region": "us-west-1"
    },{
      "test_id": "150217_6X_CJ8",
      "browser": "ie 11",
      "region": "us-west-1"
    }
  ],
  "tests-requested": 2
}
```

Add a deployment and trigger an additional round of testing for one of the sites in your account/team. A note is also automatically added to your charts.

Depending on the number of URLs you have it can take a few min to complete a round of tests across your site so we don't recommend making this a dependency for deployments. Rather you should trigger a post deployment request to the API to trigger a round of testing once your release is live.

Common reasons of getting 403 HTTP error status when submitting a new deploy:
* a deploy is in progress - only one deploy at a time is allowed on a specific site_id or url_id. You must wait for it to complete before kicking off another deploy.
* checks per month budget exceeded for the team - please increase checks budget for your account
* some tests failed to be added - try later or contact the support team

### HTTP Request

`POST https://api.speedcurve.com/v1/deploys`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
site_id | | The ID of the site you'd like to trigger a round of testing on. Not required if a url_id is specified.
url_id | | (Optional) The ID of the URL within the site you'd like to trigger a round of testing on.
note |  | (Optional) Short URL encoded note to display globally across all charts for the main site. (Max: 255 characters).
detail |  | (Optional) URL encoded note detail to display if people want more context.

### Response

Attribute | Description
--------- | -----------
deploy_id | ID of deployment.
site_id | ID of the site associated with the deploy.
timestamp | Unix timestamp of deploy.
status | Status of deployment.
message | Status message.
info | Info about the tests added for this deployment.
tests-requested | Number of tests added to the queue.

## Get a deploy

> Request

```shell
curl "https://api.speedcurve.com/v1/deploys/1234"
-u your-api-key:x
```

> Response

```json
{
  "deploy_id": 10,
  "site_id": 123,
  "timestamp": 1534917858,
  "status": "completed",
  "tests-completed": [
    {
      "test_id": "150217_Z2_CJ7",
      "url_id": 934,
      "browser": "safari",
      "region": "us-west-1"
    },{
      "test_id": "150217_6X_CJ8",
      "url_id": 934,
      "browser": "ie 11",
      "region": "us-west-1"
    }
  ],
  "tests-remaining": [],
  "note_id": 890282,
  "note": "#465 Responsive",
  "detail": "Deploy #465, awesome new responsive site live!"
}
```

Get the details for a particular deployment.

### HTTP Request

`GET https://api.speedcurve.com/v1/deploys/{deploy_id}`

### Response

Attribute | Description
--------- | -----------
deploy_id | ID of the deployment.
site_id | ID of the site associated with the deploy.
timestamp | Unix timestamp of deploy.
status | Status of testing.
test-remaining | Remaining tests waiting to be tested.
test-completed | Tests completed.
note_id | ID of the related note for this deploy.
note | Short note content to display on charts.
detail | Longer note content to provide more context when a user interacts with a note.

# Performance Budgets

## Get all performance budgets

Retrieves all synthetic performance budgets for an account/team, and 7 days of data used to evaluate the budget.

> Request

```shell
curl "https://api.speedcurve.com/v1/budgets"
-u your-api-key:x
```

> Response

```json
{
  "budget_id": 106984,
  "metric": "speedindex",
  "metric_full_name": "SpeedIndex",
  "metric_type": "syn",
  "metric_suffix": "s",
  "absolute_threshold": 2000,
  "relative_threshold": null,
  "alert_after_n_tests": 3,
  "modified_at": 1517802795,
  "created_at": 1517802795,
  "notifications_enabled": true,
  "chart": {
    "chart_id": 8482,
    "title": "Page Size",
    "chart_type": "timeseries",
    "metric": [
      "SYN|speedindex"
    ],
    "correlation_metrics": [],
    "correlation_stat": "average",
    "stat": "average",
    "site_ids": [
      299
    ],
    "label": [
      "all"
    ],
    "region": [
      "each"
    ],
    "hostname": [],
    "con": [],
    "test_profile_name": [
      "Chrome"
    ],
    "customer_data": [],
    "dashboard": {
      "dashboard_id": 8978,
      "name": "NZ News Sites"
    }
  },
  "status": "over",
  "crossings": [
    {
      "status": "over",
      "name": "Total Size (Syn), Stuff, Article",
      "latest_data": [
        {
          "x": 1557931200,
          "y": 19703,
          "deploy_ids": [],
          "test_id": null,
          "aggregated_test_count": 5
        },
        {
          "x": 1558017600,
          "y": 16736,
          "deploy_ids": [],
          "test_id": null,
          "aggregated_test_count": 5
        }
      ],
      "difference_from_threshold": 10.516
    },
    {
      "status": "under",
      "name": "Total Size (Syn), Radio NZ, Article",
      "latest_data": [],
      "difference_from_threshold": 0.538
    }
  ],
  "largest_crossing": {
    "status": "over",
    "name": "Total Size (Syn), Stuff, Article",
    "latest_data": [],
    "difference_from_threshold": 10.516
  }
}
```

### Query Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| deploy_id |         | Only retrieve performance budgets that are affected by a certain deploy ID |

### HTTP Request

`GET https://api.speedcurve.com/v1/budgets`

### Response

| Attribute | Description |
|-----------|-------------|
| budget_id | ID of the budget |
| metric | Which metric the budget is for. |
| metric_full_name | The human-readable name of the metric. |
| metric_type | The type of metric. Will always be "SYN" (synthetic). |
| metric_suffix | A unit suffix that can be appended to numeric values (e.g. "s" for seconds). |
| absolute_threshold | The absolute threshold of the budget. |
| relative_threshold | The relative threshold of the budget, if any. |
| alert_after_n_tests | The number of tests the budget must be over before an alert is sent |
| modified_at | Unix timestamp of when the budget was last modified |
| created_at | Unix timestamp of when the budget was created |
| notifications_enabled | Whether notifications (alerts) are enabled |
| chart | The Favorites chart that the budget belongs to |
| status | The current status of the budget. Will be "over" or "under". |
| crossings | A list of all data series within the budget's chart, each with their own status. |
| largest_crossing | The crossing that is the furthest away from the budget threshold |

# Settings

## Import Settings

> Request

```shell
curl "https://api.speedcurve.com/v1/import"
-u your-api-key:x
--request POST
--data mode=merge
--data data={ JSON settings object }
```

> Success Response

```json
{
  "status":"success",
  "message":"'Guardian' was updated."
}
```

You can update the settings in your account/team by importing JSON settings. [Bulk site settings import](https://support.speedcurve.com/en/articles/74074-bulk-site-settings-import) shows the JSON schema required which you can use to add new sites, browsers, regions and test times or update existing URLs.

Using this endpoint you can keep all your settings in version control or programmatically update specific URLs as often as you like.

Please note the size of JSON payload you can send is restricted to **1MB**.

### Error responses

> Error Response

```json
{
  "status":"error",
  "message":"Trying to add new sites and settings to existing team 'XXX' would it over its current monthly budget of 10000.  Total unallocated budget for 'XXX' is 1000, but 50000 is required."
}
```

Common HTTP response statuses and their reasons:
* 400 - validation problems (malformed or over-sized JSON)
* 500 - logic errors or account restrictions in imported settings (wrong site category, adding new sites with number of checks bringing account over its checks budget, no testing regions specified etc.)
* 503 - timeout, it's recommended to split your JSON to batches 30 sites or less

### HTTP Request

`POST https://api.speedcurve.com/v1/import`

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
mode |  | (merge,replace) "Merge" - Leaves existing settings in place and applies any changes found in the new JSON. To update an existing URL use the "url_to_update" option in the JSON schema. “Replace” - Warning: deletes ALL the existing settings for ALL sites first and then adds the new JSON. Any history for URLs, performance budgets and notes will be deleted. Only use this mode if you want to start from a clean slate with an empty account.
data |  | [JSON settings object](https://support.speedcurve.com/en/articles/74074-bulk-site-settings-import)

### Response

Attribute | Description
--------- | -----------
status | Status of import.
message | Status message.

## Export Settings

> Request

```shell
curl "https://api.speedcurve.com/v1/export"
-u your-api-key:x
```

> Response

```json
{
  "teams":[
    {
      "team": "Guardian",
      "sites": [
        {
          "site": "Guardian Beta",
          "urls": [
            {
              "url": "http://www.theguardian.com/uk?view=mobile",
              "label": "Home"
            }
          ]
        },
        {
          "site": "Other Site",
          "urls": [
            {
              "url": "https://example.com/",
              "label": "URL with script and basic auth",
              "username": "guardian",
              "password": "test123",
              "script": "blockDomainsExcept\texample.com\nnavigate\t%URL%"
            }
          ]
        }
      ],
      "site_settings":
        {
          "regions": [
            "US West Coast"
          ],
          "browsers": [
            "Apple iPad",
            {
              "name": "Apple iPad Landscape",
              "browser": "Chrome",
              "viewport": {
                  "width": 1024,
                  "height": 768,
                  "devicePixelRatio": 2
              },
              "bandwidthDown_KBps": 5000,
              "bandwidthUp_KBps": 1000,
              "latency_Ms": 28
          },
          "Apple iPhone 5",
          {
              "name": "Apple iPhone 5 Landscape",
              "browser": "Chrome",
              "viewport": {
                  "width": 568,
                  "height": 320,
                  "devicePixelRatio": 2
              },
              "bandwidthDown_KBps": 1600,
              "bandwidthUp_KBps": 768,
              "latency_Ms": 300
          },
          "Chrome"
      ],
      "times": [
          "16:00"
      ]
      }
    }
  ]
}
```

Returns the current account/team settings in the JSON import format. You can then edit and import these settings using the "merge" mode to make changes. Warning: Not all settings like budgets, notes, domains etc are yet supported by the export/import endpoints. If you do an export and then import the same file using the "replace" mode you will lose any of the non supported settings. Always use the "merge" mode when importing unless you want to delete all settings in your account and start from scratch.

### HTTP Request

`GET https://api.speedcurve.com/v1/export`

### Response

Attribute | Description
--------- | -----------
JSON | JSON object that represents your account/team settings

# LUX

## Export LUX Data

> Request

```shell
curl "https://api.speedcurve.com/v1/lux/export"
-u your-api-key:x
```

> Response

```json
{
  "download_url":"https://s3.amazonaws.com/lux-exports.speedcurve.com/lux.gz"
}
```

Get LUX Export URL. Returns an S3 pre-signed URL for the specified LUX export file. The URL expires in 10 minutes. LUX export files are done on a daily basis and contain the data from 00:00:00 to 23:59:59 UTC. Files become available after 5am UTC. If you try to access the file between midnight and 5am you'll get a URL that returns an error.

**This API endpoint is not enabled by default.** In order to use this endpoint, you must check the **Enable LUX export API** checkbox in your SpeedCurve team's LUX settings. Please refer to the [**LUX Export Documentation**](https://speedcurve.com/lux-export-documentation/) for information on how to parse and interpret the exported data.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
date | "" | The date parameter is a date string of the format "Ymd", eg, "20180128". It is optional and defaults to yesterday (the most recent export file).

### HTTP Request

`GET https://api.speedcurve.com/v1/lux/export`

### Response

Attribute | Description
--------- | -----------
download_url | Returns an S3 pre-signed URL for the specified LUX export file. The URL expires in 10 minutes.


# Errors

The SpeedCurve API uses the following error codes:

Error Code | Description
---------- | -------
400 | Bad Request -- Request cannot be fulfilled due to bad syntax.
401 | Unauthorized -- Invalid API key.
403 | Forbidden -- Insufficient privileges to perform this action.
404 | Not Found -- The requested resource was not found.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

# Change Log

9th Dec 2020

* Update API docs to match recently added metrics like web vitals etc.

20th Mar 2020

* Removed pagespeed metric which has been deprecated for a year and replaced by the Lighthouse metrics.
* Removed undocumented values for test results: "test" and "label".
* Added rate limiting of 50 requests per second.

23rd Jan 2020

* Added; first contentful paint, largest contentful paint and first meaningful paint to API and docs.

22nd May 2019

* Add the `url_id` parameter to the /deploy endpoint.

20th May 2019

* Add /budgets endpoint for fetching the status of performance budgets.

23rd April 2019

* Add CPU time metrics split out by idle, scripting, layout, and painting.

30th Oct 2018

* Add "First CPU Idle" and "Time To Interactive" metrics. Note that "First Interactive" is now deprecated and will be removed down the track.

4th Sept 2018

* Add /lux/export endpoint for exporting raw LUX page views.

22nd Aug 2018

* Add timestamps to /deploy endpoint.
* Add add Lighthouse metrics.
* Add hero rendering metrics.

24th Oct 2017

* Added third party and blocking metrics.

25th Sept 2017

* Added [hero rendering times](https://speedcurve.com/blog/web-performance-monitoring-hero-times/)

1st May 2017

* Add support for deleting sites, URLs, tests and notes.

24 Feb 2017

* Add a list of all deploys to the /deploys endpoint.
* Rename /deploy endpoint to /deploys for consistency.

9 Feb 2017

* Removed median tests results from /sites endpoint by default. Can be re-enabled with median=1.

9 Jan 2017

* Added "status" for any test results so errors vs successful tests are easier to determine.

31 Oct 2016

* Added more detail to test data including bandwidth down/up, latency and packet loss along with custom metrics.

25 Oct 2016

* Added new import/export endpoints that takes a JSON settings object and let's you update the basic settings for an account/team.

18 May 2016

* Add site/{site_id} endpoint with site settings details.
* Change "location" to "region" in returned JSON to match Settings naming.
* Removed "statusboard" format option form /sites endpoint.
* Added custom_metrics to urls/{url_id} endpoint.

23 Nov 2015

* Added support for triggering a deploy on any site in an account/team.
* Added support for site_id on Notes letting you add notes to any site.
* Switched to note_id, deploy_id, site_id for clarity and more consistency.

18 Feb 2015

* Added Deployment method to trigger additional rounds of testing for the main site in an account.

9 Jan 2015

* Added Notes method to read and write notes to the main site in an account.

24 May 2014

* Added support for multi-site accounts to the "urls" method.
* Added more details to the median results for the "sites" method.

26 April 2014

* Switch to HTTPS only
* Extended Sites method to include multi-site accounts.

27 Mar 2014

* Add 14 day median history to Sites method.

23 Mar 2014

* Changed method names to plural and switched "user" to "sites".
* Added Pagespeed
* Added Speed Index

