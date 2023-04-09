
Problem statement:

When you signup on Cognito screen, a native user gets created in the user pool. If you federate the user pool through a social provider like Google, then you can also sign in using your google account. This way, same email id can end up with 2 different users - a native cognito user, and a federated Google user. What we want is that, only 1 user exists in the user pool for a given email id. 


Solution:
Create a Lambda and register it as Congito user pool's Pre Sign up trigger.
When a Google federated user is logging in, check if native user already exists for that email,
 and link the Google user with existing native user using Cognito api's 'admin_link_provider_for_user' function.
 
And if native user does not exist, then first create a native user with a temporary password using  'admin_create_user',
and then link Google login as above. 

When creating a user from lambda trigger, just beware of infinite recursion that can happen due to user creation and their triggers. Same has been handled in code based on event trigger source.

Like Google, other federations like Facebook can also be handled. The admin_link_provider_for_user supports linking of max 5 users. 

To federate a user pool with a social provider like Google, follow this video:
https://www.youtube.com/watch?v=pKA8qXINjkU&list=PLt4YNKEVZK8-VM5FwFCXzlFmTFOWD0Tq2&index=123
