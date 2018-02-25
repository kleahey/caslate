# Standard Response Codes

The Common App API uses the following HTTP status codes:


HTTP Status Code | Scenario | Response Body
:---------- | :------- | :-------
200 | Response OK | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;……Response as per the method.<br/>}
201 | Record Created | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"ID": "9898809807878"<br/>}
202 | Record Accepted | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;……Response as per the method.<br/>}
400 | POST Call made without<br/>having any data | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Bad Request"<br/>}
401 | Unauthorized (i.e. Username<br/>and Password) or token is not<br/>correct | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Unauthorized Access"<br/>}
403 | Forbidden (i.e. Forbidden<br/>to access specific methods) | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Request Forbidden"<br/>}
404 | Record Not Found | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Record Not Found"<br/>}
409 | Record Already Exists | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Record Already exists",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"ID" : "86786876876786"<br/>}
422 | Validation Error | {<br/>&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;"Message": "Validation Failed",<br/>&nbsp;&nbsp;&nbsp;"Errors":<br/>&nbsp;&nbsp;&nbsp;&nbsp;[<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Field": "Email",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Email should not be empty."<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Field": "DOB",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Invalid DOB"<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;}
500 | Server Error | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Internal Server Error"<br/>}
501 | Method does not exist | {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Message": "Method not implemented"<br/>}
