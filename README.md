# Freshsales - ZeroBounce Integration

ZeroBounce is an Email validation tool which helps teams in making sure that their contact lists are not containing any contacts with invalid email addresses. ZeroBounce claims that their accuracy level is 99% and their tools help with Email Marketing ROI.

This integration helps in identifying whether the contacts in Freshsales have a valid email address or not with the help of ZeroBounce.

## How this integration works?
Freshsales-ZeroBounce Integration runs in two scenarios,

1) Whenever a new contact is created in Freshsales.
   
2) Whenever the default Emails field in Freshsales is updated in a contact.

The marketplace application listens to any contact create or update events in your Freshsales account and then sends a request to ZeroBounce to check if the Email address entered in the default "Emails" field is valid or not. ZeroBounce analyzes the email address and then responds with a set of details as shown in below example,

Consider the email address entered in the contact to be "james.sampleton@freshworks.com" then following will be the response from ZeroBounce,

```javascript
{"address"=>"james.sampleton@freshworks.com",
"status"=>"catch-all",
"substatus"=>"",
"freeemail"=>false,
"didyoumean"=>nil,
"account"=>"jamessampleton",
"domain"=>"freshworks.com",
"domainagedays"=>"9691",
"smtpprovider"=>"g-suite",
"mxfound"=>"true",
"mxrecord"=>"aspmx.l.google.com",
"firstname"=>"james",
"lastname"=>"sampleton",
"gender"=>"male",
"country"=>nil,
"region"=>nil,
"city"=>nil,
"zipcode"=>nil,
"processedat"=>"2024-08-26 12:18:56.333"}
```

The primary information in this response is the "status" value. This status value is stored in a custom text field in the contacts module in Freshsales. The other details are added as a note in the respective contact record for reference.

## What is status in ZeroBounce Email Validation?
ZeroBounce validates each email address and the status value helps us in determining if we should be sending emails to the respective email address or not. Following are status codes in ZeroBounce,

  **Valid** - The email address is valid and the bounce rate is minimal. 
  
  **Invalid** - Emails sent to email addresses with status as Invalid will most likely bounce.
  
  **Catch-all** - Its difficult to identify if these emails are valid or not and we can only determine its validity by sending an email. If you want to send emails to them then do so by creating a separate list of catch-all email addresses.
  
  **Spamtrap** - Avoid sending emails to them.
  
  **Abuse** - Avoid sending emails to them.
  
  **Do_not_mail** - Valid email addresses but should not be emailed.
  
  **Unknown** - ZeroBounce is not sure about this email address and its more likely to be invalid.

More information about ZeroBounce status codes can be seen in the below document,
https://www.zerobounce.net/docs/email-list-validation/status-codes/

## Do I need credits in ZeroBounce?
Yes, you will need to have credits in ZeroBounce for this integration to work. By default, ZeroBounce offers 100 free credits and their pricing can be check in the below page,
https://www.zerobounce.net/email-validation-pricing/

## Do I need to have a paid plan in Freshsales?
Yes, Freshsales offers APIs in paid plans only and so you will need a paid plan in Freshsales. Following article has information about the API limits in each plan,
https://crmsupport.freshworks.com/support/solutions/articles/50000005599-does-the-web-application-have-api-request-limits-for-an-account-

# How to get my API key in ZeroBounce?
Login to your ZeroBounce account and click on the "API" tab in the left nav bar or open the link - https://www.zerobounce.net/members/API

![image](https://github.com/user-attachments/assets/b200a697-cad5-4792-9771-4a09a3bc2f91)

# How do I get my API key in Freshsales and what is my Freshsales domain?
In order to get your Freshsales API key, login to your Freshsales account and click on the personal settings icon in the top right corner -> Click on Personal settings,

![image](https://github.com/user-attachments/assets/479fa6b2-446e-4519-9fac-25da4e0b2810)

Next go to "API" tab within your personal settings and copy the API key mentioned under "CRM API Details",

![image](https://github.com/user-attachments/assets/7ed89859-ad67-4b3b-b77e-8d585a9771ae)

Your Freshsales domain is a part of your Freshsales account URL. For example, if your Freshsales account URL is "crm.myfreshworks.com" then your Freshsales domain is "crm".

## How to create a custom text field in contacts module in Freshsales?
A custom text field in the contacts module can be created by following the below steps,

1) Go to Admin settings in your Freshsales account and click on Contacts,

  ![image](https://github.com/user-attachments/assets/ab83491a-ec7c-4216-9d0c-931ce6f5f88c)

2) Click on Add field in the right corner on the contacts form fields page,

   ![image](https://github.com/user-attachments/assets/15649035-ca6f-46c3-badc-4bbf57c210bd)


3) Select "Text field" and click on Add selected,

   ![image](https://github.com/user-attachments/assets/e386ad0e-854c-43fc-9459-52082497cb5d)

4) Enter a field label like "ZeroBounce Status" and then click on Save,

   ![image](https://github.com/user-attachments/assets/e418b370-8c93-4223-bf2f-1c6c9bbcb431)

5) Copy the internal name for that field and paste it in the app installation settings. For example if your field label is "ZeroBounce status" then the custom field internal name will be "cf_zerobounce_status".
   
Note - The internal name does not change. So if you change the field label then the internal name will not change it will remain the same on how it was created initially.

## Why should I create a custom text field in Freshsales?
A custom text field is required to store the "status" value under the respective contact. The primary reason to create a custom field is so that you can filter contacts based on the values stored in this text field and also export this data from Freshsales whenever required. 

## What happens if I enter an incorrect internal field name for the text field or do not create one?
If you enter an incorrect internal field name or place a dummy value in this field then the status value will not be stored in any contact field and you will not be able to filter or export contacts based on ZeroBounce email validation. The overall response from ZeroBouce will still be displayed under notes.

## Limitation
In case of bulk import through CSV when a high number of records are created within a minute, the app at the moment is not updating the field for all records as backend platform limits are being breached. We are working on a solution to fix this scenario. 

## For Support, drop an email to pulkit.chowdry@freshworks.com
