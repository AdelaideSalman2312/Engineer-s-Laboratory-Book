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
**Access Token**
  
  It expires in 59 minutes time just before an hour is depleted.
  
  **Since I have now gotten my access token we can now talk about curl lol**

  Curl is an open-source, command line tool for sending data. Many API clients include an option to import Curl code snippets, e.g:

API Connector's Curl import tool
Postman's Curl-to-Postman function
These Curl import tools will automatically encode your credentials to base64 during the import process.
So Imagine me on top of a matatu with a speaker yes it campaign season but this time I am campaigning about encryption . Base 64 is not encryption please  anybody can decode it that is why all API requests must go through https.Most API clients will automatically apply basic authentication if you enter your credentials as https://username:password@api.com
curl.exe -v -u "YOUR_CONSUMER_KEY:YOUR_CONSUMER_SECRET" "https://sandbox.safaricom.co.ke/oauth/v1/generate?grant_type=client_credentials" - Please do not leave spaces between the key and the secret kitakuramba wewe .

**But what does that command mean**

*curl.exe -v -u "KEY:SECRET" "https://sandbox.safaricom.co.ke/oauth/v1/generate?grant_type=client_credentials"*

- **curl.exe**: the program. It sends a request to a web address and shows you what comes back, like a browser without the visuals. On Windows you add .exe so PowerShell doesn’t confuse it with a different command.
- **-v**: verbose. It shows you everything curl sends and receives, including the headers. Lines starting with > are what you sent; lines starting with < are what came back.
- **-u "KEY:SECRET"**: user credentials. Curl joins them with a colon, Base64-encodes them, and puts them in a header called Authorization: Basic ....

**When you remove u** - you will get bad request in  your output . Safaricom rejects it because it had no way to know who was asking. This error usually means that this error usually means missing or wrong credentials .

- **The URL**: where the request goes. The part after ? (grant_type=client_credentials) is extra information sent along with the request. Here, it tells Safaricom, “I’m an app proving who I am with my own credentials.”
 You didn’t type**GET** anywhere, but curl sends a GET request by default. GET means “give me something,” and here you’re asking for a token.

**Summary**: curl sent a GET request to Safaricom’s token address, carrying your Base64-encoded key and secret in a header. Safaricom checked them and sent back a token that lasts about an hour.

# **Password**
So Safaricom has decided it is not going to an enabler of lazy developers - yes you read that right karibu Kenya.
To send a payment prompt, Safaricom needs to know which business is asking for money. The password proves that. You don’t receive it ready-made; you make it yourself, like mixing a recipe or cooking your own food - Safaricom is not offering  takeouts .

The recipe has three ingredients:

Shortcode: the business’s number. For the sandbox, Safaricom gives you a test one.
Passkey: a long secret code that goes with that shortcode. Also given to you for testing.
Timestamp: today’s date and time written as one long number. For example, 5 November 2026 at 2:30 pm becomes 20261105143000.

**How to mix them**: stick all three together in that order, like shortcode + passkey + timestamp, making one long string. Then encode it with Base64, the same scrambling curl did to your key and secret. The result is your password.

**Lets take a look at post**
curl.exe -X POST "URL" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-Type: application/json" -d "{...data...}"
- **X POST**: changes the request type from the default GET to POST.
- **H "..."**: adds a header. You can use -H as many times as you need.
    - **Authorization**: Bearer YOUR_ACCESS_TOKEN shows Safaricom the token you got. “Bearer”             means “whoever carries this token is allowed in.” Notice the change: you used Basic with          your key and secret to get the token, and now you use Bearer with the token itself.
      - **Content-Type**: application/json tells Safaricom the data you’re sending is in JSON   format.
        
-d "...": the data itself, the body of the request.

**KEY : SECRET VS ACCESS TOKEN**

A token can be regenerated with your secret and key but once a person has your secret and key 
they inside your app .Your key and secret are the master keys: whoever has them can keep generating tokens forever until you change them. A token is a temporary pass. If it leaks, the damage is limited to about an hour, and then it’s useless. That’s why you use the master keys as rarely as possible and send the short-lived token with every request instead.

# **Password Mixer**
Please pay attention :
1. Get the passkey and shortcode from the test credentials.
2. Now it is time to be a mixer
   *import base64
from datetime import datetime

shortcode = "YOUR_SHORTCODE"
passkey = "YOUR_PASSKEY"
timestamp = datetime.now().strftime("%Y%m%d%H%M%S")

raw = shortcode + passkey + timestamp
password = base64.b64encode(raw.encode()).decode()

print("Timestamp:", timestamp)
print("Password:", password)*


What each part does:

- **datetime.now().strftime(...)** gets the current time and writes it in the YYYYMMDDHHmmss format Safaricom expects.
- **shortcode + passkey + timestamp** joins the three ingredients in the right order.
- **.encode()** turns the text into bytes, because Base64 works on bytes, not text.
base64.b64encode(...) encodes it, and .decode() turns the result back into normal text you can copy.

**Summary**: the script builds your password from the shortcode, passkey and current time, then prints both the password and the timestamp it used.

**Mistakes I logged**
1. “Can’t open file” usually means the terminal isn’t in the file’s folder - remedy just type python "name of your file in your code editor" - not the path friends .

 **READER NOTE**
 1 . note: always copy the password and timestamp from the same run. One mismatched copy-paste is a classic way to get stuck.
 When now you  have all three pieces you need: a token, a password and its timestamp. Time to send your own STK Push.

 **The JSON for the POST**
 Typing JSON straight into a PowerShell command is painful, because PowerShell mangles the quotes. So we’ll put the data in a file instead. In the same folder as your password script, create stk.json:

json
{
  "BusinessShortCode": "YOUR_SHORTCODE",
  "Password": "YOUR_PASSWORD",
  "Timestamp": "YOUR_TIMESTAMP",
  "TransactionType": "CustomerPayBillOnline",
  "Amount": "1",
  "PartyA": "2547XXXXXXXX",
  "PartyB": "YOUR_SHORTCODE",
  "PhoneNumber": "2547XXXXXXXX",
  "CallBackURL": "https://example.com/callback",
  "AccountReference": "Test001",
  "TransactionDesc": "Test payment"
}
**Procedure**
1.Password and Timestamp: run your script and copy both from the same run.
2.PartyA and PhoneNumber: your own number. PartyA is who pays, and PhoneNumber is where the prompt appears.
3.PartyB and phonenumber : the same shortcode again. This is who receives the money. And the same phone number - Do not let these trip you up .
4.CallBackURL: leave it as the placeholder for now. You know what that means: the prompt will arrive, but the result will go nowhere. We’ll fix that next.
5. AccountReference: keep it short. Daraja rejects an AccountReference longer than 12 characters with a cryptic error.

**Now send it**
*curl.exe -v -X POST "https://sandbox.safaricom.co.ke/mpesa/stkpush/v1/processrequest" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-Type: application/json" -d "@stk.json"*

The @ in -d "@stk.json" tells curl to read the data from the file. Keep the quotes around it, because in PowerShell a bare @ means something else.

**Summary**: you’re sending a POST request with your Bearer token in a header and a JSON body from a file. The body contains your password, the timestamp you used to build it, and the payment details.

**Please run a strict program here**
Do not dare delete the bearer as you input you access token .



 
 
3. 












**
