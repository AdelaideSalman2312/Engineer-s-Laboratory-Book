# Daraja API my scattered graph notes 

**Procedure**

1.Open the Safaricom daraja portal 

2.At the top left side of the portal sign up - if this is your first time in the portal 

3.Sign up with your details and wait to receive the authentication code  in your email
  and then you will be directed to the developers side of the portal
  
4.Go to My Apps

5.Create a new sand box app

6.You will get a drop down list and choose Lipa na M-pesa - since it is used in most small 
   and medium enterprises 
   
7.Then name your sandboxed application and create it then copy your consumer key and consumer secret and passkey 

 Go to APIs scroll down to lipa na M-Pesa and click - then simulate the API 
{
  "MerchantRequestID": "135b-4679-8ea6-1c3dca88c6a41201755",
  "CheckoutRequestID": "ws_CO_051120261556448758417109",
  "ResponseCode": "0",
  "ResponseDescription": "Success. Request accepted for processing",
  "CustomerMessage": "Success. Request accepted for processing"
}
# Callbacks

Callbacks confuse almost everyone at first, so let’s slow down and use an everyday picture.

Think of a fundi fixing your phone. You drop it off and he says, “Sawa, I’ve received it.” That’s not the same as “it’s fixed.” So you leave your number and tell him to call you when it’s done. Later, he calls: either “it’s fixed” or “the screen can’t be repaired.”

M-Pesa works exactly the same way:

“Sawa, I’ve received it” is the response you just saw, with ResponseCode: 0. It only means Safaricom accepted your request and sent the prompt to the phone. It says nothing about whether anyone paid.
Your phone number is the callback URL: the address you give Safaricom so it can reach you later.
The phone call later is the callback. After the person enters their PIN, cancels, or ignores the prompt, Safaricom sends the result to that address.
**why can’t Safaricom just include the result in that first response you got?**
The first response is about whether safaricom sent a prompt to the users one -the stk push

**Now take it one step further. At the moment that first response comes back, what is the person holding the phone doing? Have they paid yet?**
Nope they have received the prompt ; they can choose to pay ; input the correct pin or put a wrong pin or cancel the transaction or the person can ignore the prompt and let it time out.
So there are four outcomes, and only one of them means money moved. Your app can’t know which one happened until the callback arrives.

**Using curl.exe****on Windows if your postman is not a bitch** 
the first errors were from curl: (3) URL rejected: Bad hostname
https://sandbox.safaricom.co.ke/oauth/v1/generate?grant_type=client_credentials
**So these next errors were like progress**
curl: (3) URL rejected: Port number wasnot a decimal number between 0 and 65535 - THis was error from my computer -- curl: (3) URL rejected means curl never sent anything. It misread part of my command as a web address. Port numbers come after a colon, so curl probably saw KEY:SECRET and thought it was an address like website:port. That happens when the -u flag isn’t attached to the credentials, for example a typo in -u, a missing quote, or the credentials appearing before -u.
{
                    "requestId": "7b5f-4542-ac5d-78715e3756372965638",
                    "errorCode": "400.008.01",
                    "errorMessage": "Invalid Authentication passed"
                } --- This one was from Safaricom --- The JSON response means my request actually reached Daraja. Its server answered “Invalid Authentication passed,” so it received credentials but didn’t accept them.

The usual causes for the second error:

- The placeholders are still in the command instead of your real key and secret.
- A space, or a missing first or last character, crept in while copying.
- The key and secret are swapped. The key comes first, then the colon, then the secret.
- The key came from one app and the secret from another.

  **Since I have now gotten my access token we can now talk about curl lol**

  Curl is an open-source, command line tool for sending data. Many API clients include an option to import Curl code snippets, e.g:

API Connector's Curl import tool
Postman's Curl-to-Postman function
These Curl import tools will automatically encode your credentials to base64 during the import process.
So Imagine me on top of a matatu with a speaker yes it campaign season but this time I am campaigning about encryption . Base 64 is not encryption please  anybody can decode it that is why all API requests must go through https.Most API clients will automatically apply basic authentication if you enter your credentials as https://username:password@api.com
curl.exe -v -u "YOUR_CONSUMER_KEY:YOUR_CONSUMER_SECRET" "https://sandbox.safaricom.co.ke/oauth/v1/generate?grant_type=client_credentials" - Please do not leave spays between the key and the secret kitakuramba wewe .
    



