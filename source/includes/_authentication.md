# Authentication

The Common Application will provide credentials to partner organizations. When calling the Common App APIs, partners need to generate an Authentication Token by passing their credentials. After generating the token, the partner will be able to call other API services by passing the Authentication token and the provided Partner API key.

## Steps for Accessing the API

> To authorize, use this code:

```csharp
var client = new RestClient("https://<API_ENDPOINT>/auth/token");
var request = new RestRequest(Method.POST);
request.AddHeader("postman-token", "3b481a64-a5ab-547f-16b5-48123e8d4099");
request.AddHeader("cache-control", "no-cache");
request.AddHeader("content-type", "application/json");
request.AddParameter("application/json","{\r\n  \"UserName\":\"PartnersUserName\",\r\n \"Password\":\
"PartnersPassword\"\r\n}",ParameterType.RequestBody);
IRestResponse response = client.Execute(request);
```

<b>1.</b> The Common App will create a username, password and API Key that will be shared with the partner

<b>2.</b> Each partner will then use those credentials to access our authorization API and receive a unique token and refresh token for each both lower (development) and production environments. There will be one set of tokens per (partner x environment); so the partner would need to make the initial authorization call in each environment before proceeding to other API calls.

<b>3.</b> HTTP Request for Partner Authentication endpoint:  https://API_ENDPOINT/auth/token

&nbsp;&nbsp;&nbsp;a. <b>Token:</b> Valid for one hour, after that it will expire and the partner must refresh the token(Step 4) to get a new valid token.

&nbsp;&nbsp;&nbsp;b. <b>RefreshToken:</b> Valid for 15 days and must be saved by the partner so the partner can use it when the token value expires without passing partner UserName and Password again. Follow step 4 to refresh the token.

&nbsp;&nbsp;&nbsp;c. <b>Request Type:</b> POST<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <b>Request Header:</b> content-type : application/json<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <b>Request Body:</b><br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"UserName": "PartnerUserName",<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Password": "PartnerPassword"<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <b>Response:</b><br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Token : "TokenValue",<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RefreshToken: "RefreshTokenValue"<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br/>

> To refresh token, use this code:

```csharp
var client = new RestClient("https://<API_ENDPOINT>/auth/token/refresh");
var request = new RestRequest(Method.POST);
request.AddHeader("postman-token", "baa134f6-d11d-f3dc-6ca0-51c693a5cc27");
request.AddHeader("cache-control", "no-cache");
request.AddHeader("content-type", "application/json");
request.AddParameter("application/json", "{\r\n  \"RefreshToken\":\"RefreshTokenValue\"\r\n}", ParameterType.RequestBody);
IRestResponse response = client.Execute(request);
```

<b>4</b>. Refresh Token endpoint: https://<API_ENDPOINT>/auth/token/refresh

&nbsp;&nbsp;&nbsp;a.The refresh token is generated once in step 2 and thereafter needs to be saved by the partner securely until it expires after 15 days. Common App doesn't store any tokens or any authentication information.

&nbsp;&nbsp;&nbsp;b. After the refresh token has expired, the partner needs to follow step 2 again to get the new token and refresh tokens.<br/><br/>
&nbsp;&nbsp;&nbsp;c. <b>Request Type:</b> POST<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Request Header:</b> content-type : application/json<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Request Body:</b><br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "RefreshToken": "RefreshTokenValue"<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Response:</b><br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Token : "TokenValue"<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br/>

> To access other API endpoints, use this code:

```csharp
var http = require("http");

var options = {
  "method": "POST",
  "hostname": "https://<API_ENDPOINT>/<ENV[dev/prod]>/",
  "path": "/applicant/search",
  "headers": {
    "content-type": "application/json",
    "authorization": "eyJraWQiOiJjZ21ZNllqVlwvV3psNG9qZTBGMDl1bFBzbStXeGhObzlcLzF3UDJJZEF0Kzg9IiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiI1YzlhYTYyMC1mZWMxLTQ4MjItOWRjNy0yNzg5MjNmMjViMDUiLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAudXMtZWFzdC0xLmFtYXpvbmF3cy5jb21cL3VzLWVhc3QtMV9sMDY3WWRvWDkiLCJjb2duaXRvOnVzZXJuYW1lIjoiQjA3MTA1MEQtOUEyRS00QjJFLTg2QzMtREFDNTA1Q0YxNTg3IiwiYXVkIjoiNGMwNnBpZjNoZGl0ODFhbXY4MnNvOHY5YmoiLCJldmVudF9pZCI6IjdlZTQ4YzdkLWU1OTMtMTFlNy1iY2FiLWU3MTA1NGZiZGE2ZSIsInRva2VuX3VzZSI6ImlkIiwiY3VzdG9tOlBBUlROR",
"X-API-KEY" : "zjgyn8HHJkiyre1iPjg6"   
"cache-control": "no-cache"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({ Email: '2akumar@ca.org', DOB: '01.02.1985' }));
req.end();
```

<b>5.</b> To access other API endpoints, the partner will need to pass this token as a "Authorization" and the APIKey as an "X-API-KEY" header in every Request.<br/><br/>
&nbsp;&nbsp;&nbsp;a. <b>Request Type:</b> POST<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Request Header:</b> content-type : application/json<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Request Header:</b> X-API-KEY<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Request Body:</b><br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "Email":"fyvg1@mailinator.com",<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "DOB":"12/24/1986"<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br/>
