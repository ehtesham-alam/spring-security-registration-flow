Endpoints:- 

====================================================================

Type = POST Request

ENDPOINT:- http://localhost:8080/register

JSON = {
	"firstName":"Ankit",
	"lastName":"Gupta",
	"password":"123456",
	"email":"ankit@tcs.com"
	}

After this user will get this URL to Validate 

At this point of time:-

A token is generated and stored in user_registration.verification_token (The validity of this token is SYSTEM_TIME+10 mins from generation, and it would get deleted from database if user tries to verify after 10 minutes)

Here user_registration.verification_token has a one to one relationship with user_registration.user

---------------------------------------------------------------------------------

1	2022-02-18 01:54:45.351000	0403cae5-14f1-457d-80bd-3d56325b8999	1

---------------------------------------------------------------------------------

And at this point in time:-

user_registration.user
(enabled column in the database has the value = 0)

----------------------------------------------------------------------------------


1	ankit@tcs.com	0	Ankit	Gupta	$2a$11$D1e7tN0okA1v1qwiHhGy5OOpO/s8BORhJ6iuQNyBz//j9LTxO3Ex.	USER

-----------------------------------------------------------------------------------

But after authenticating ie clicking the genereated URL in the console(this can be modified to send this url to user's email address)

eg:- URL

Click the link to verify your account: http://localhost:8080/verifyRegistration?token=0403cae5-14f1-457d-80bd-3d56325b8999

Copy this link in Postman or Browser to activate the user in the database.

In user_registration.user
(enabled column in the database changes from 0 to 1) ie.


------------------------------------------------------------------------------------


1	ankit@tcs.com	1	Ankit	Gupta	$2a$11$D1e7tN0okA1v1qwiHhGy5OOpO/s8BORhJ6iuQNyBz//j9LTxO3Ex.	USER


-----------------------------------------------------------------------------------

The token from user_registration.verification_token would automatically be discarded if the user is unable to verify in 10 minutes

ELSE

(Within 10 minutes)
User can regenerate Verification Token with the help of old token which is explained in much better way in subsequent sections.

================================================================================================================================================================

Just try to repaste the above generated URL:-(After 10 minutes)

ie http://localhost:8080/verifyRegistration?token=0403cae5-14f1-457d-80bd-3d56325b8999

You'll be greeted with BAD USER

================================================================================================================================================================

If you have the Previous token link with you, you can even ResendVerifyToken as GET request to get yourself a new token.

The previous generated URL was:-

http://localhost:8080/verifyRegistration?token=0403cae5-14f1-457d-80bd-3d56325b8999

AND the ResendVerifyToken for it is...

The endpoint for this is:-

ENDPOINT:- http://localhost:8080/resendVerifyToken?token=0403cae5-14f1-457d-80bd-3d56325b8999

You'll be greeted with Verification link sent and the old token would be replaced with the new token, and then you can register yourself with the new token with /verifyRegistration Endpoint.

==================================================================================================================================================================

To Reset Password:-

TYPE = POST_Request

ENDPOINT:- http://localhost:8080/resetPassword

JSON = {
	"email":"ankit@tcs.com"
	 }

Check response back from the edpoint and in console.... You'll get a new token for that emailID with the SavePassword ENDPOINT..

PS:- You'll find out that data in user_registration.password_reset_token would be populated, and the expiration time would be SYSTEM_TIME+10 minutes

eg:- Click the link to Reset your Password: http://localhost:8080/savePassword?token=2760c208-cdee-4362-a8fc-666513b5a7fd



==================================================================================================================================================================

To Save the Password:-

TYPE = POST_Request

Use the token from above link and hit this ENDPOINT combined with the newly generated token that you got from /resetPassword  

ENDPOINT:- http://localhost:8080/savePassword?token=2760c208-cdee-4362-a8fc-666513b5a7fd

JSON = {
	"newPassword":"123"
	 }

You'll be greeted with Password Reset Successfully and the password would be updated in our database


==================================================================================================================================================================

To Change the Password with the Old Password:-

TYPE = POST_Request

ENDPOINT:- http://localhost:8080/changePassword

JSON = {
	 "email":"ankit@tcs.com",
	 "oldPassword":"123",
	 "newPassword":"321"
	 }

You'll be greeted with Password Changed Successfully and your password would be changed in the database.

==================================================================================================================================================================


