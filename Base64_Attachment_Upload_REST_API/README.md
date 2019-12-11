# Base64 Attachment Upload REST API
<img align="right" src="logo_base64.png"/>

This plugin provides a REST API that allows upload of Base64 encoded attachments:
* Direct - allows direct upload of Base64 encoded attachments where metadata are stored in URL
* JSON - allows upload of Base64 encoded attachments where both attachment and metadata are stored in JSON structore

## Authentication
Both direct and JSON endpoints require preemptive basic authentication.

## Direct endpoint

`/rest/base64attachment/1.0/direct/{issueIdOrKey}/{filename}/`

Allows direct upload of Base64 encoded attachments where metadata are stored in URL.  

The URL can be parametrized as follows:
* __{issueIdOrKey}__ - path parameter that should be replaced with the issue id or key
* __{filename}__ - path parameter that should be replaced with filename that will be displayed in Jira

Additionally, following optional query parameters may be used:
* __contentType__ - if used, then Jira will store this content type for the uploaded attachment.  If not specified, then internal Jira functionality will decide best content type for uploaded attachment.
* __zipFlag__ - if used, then the zip flag will be passed to Jira to indicate that the uploaded attachment is a zip archive.

Example:
<table>
 <tr>
  <th>
   URL
  </th>
  <td>
   http://localhost:8080/rest/base64attachment/1.0/direct/SAM-43/sample.txt/?contentType=text%2Fplain&zipFlag=false
  </td>
 </tr>
 <tr>
  <th>
   Method
  </th>
  <td>
   POST
  </td>
 </tr>
 <tr>
  <th>
   Content Type (HTTP Header)
  </th>
  <td>
   text/plain
 </tr>
 <tr>
  <th>
   Body
  </th>
  <td>
   <pre>SGVsbG8gd29ybGQhCg==</pre>
  </td>
 </tr>
</table>

## JSON endpoint

`/rest/base64attachment/1.0/json`

Allows upload of Base64 encoded attachments where both attachment and metadata are stored in JSON structore.

The JSON structure included in body can be parametrized as follows:
* __issueIdOrKey__ - required parameter, should be replaced with the issue id or key
* __base64EncodedData__ - required parameter, should contain base64 encoded attachment data
* __filename__ - required parameter, should be replaced with filename that will be displayed in Jira
* __contentType__ - optional parameter, if used then Jira will store this content type for the uploaded attachment (If not specified, then internal Jira functionality will decide best content type for uploaded attachment)
* __zipFlag__ - optional parameter, if used then the zip flag will be passed to Jira to indicate that the uploaded attachment is a zip archive

Example:
<table>
 <tr>
  <th>
   URL
  </th>
  <td>
   http://localhost:8080/rest/base64attachment/1.0/json
  </td>
 </tr>
 <tr>
  <th>
   Method
  </th>
  <td>
   POST
  </td>
 </tr>
 <tr>
  <th>
   Content Type (HTTP Header)
  </th>
  <td>
   application/json
 </tr>
 <tr>
  <th>
   Body
  </th>
  <td>
   <pre>{
 "issueKeyOrId" : "SAM-43",
 "base64EncodedData" : "SGVsbG8gd29ybGQhCg==",
 "filename" : "sample.txt",
 "zipFlag" : "false",
 "contentType" : "text/plain"
}</pre>
  </td>
 </tr>
</table>


## API Response

This chapter describes the structure of response that is returned by both API endpoints.

### Success

Both endpoints respond with a JSON response that contain following fields when the API request was processed successfully:
* __result__ - OK, when attachment was correctly created
* __attachmentId__ - attachment ID

Example:

`{"result":"OK","attachmentId":"10118"}`

### Error

Both endpoints respond usually with a JSON response that will contain following fields when the API request processing failed:
* __result__ - ERROR
* __errorMessage__ - error message

The HTTP status code of an error response will be in the 4** or 5** range.

Example:

`{"result":"ERROR","errorMessage":"IssueIdOrKey parameter can not be null!"}`
