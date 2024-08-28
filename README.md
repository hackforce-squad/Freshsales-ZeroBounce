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
{"address"=>"james.sampleton@freshworks.com", "status"=>"catch-all", "substatus"=>"", "freeemail"=>false, "didyoumean"=>nil, "account"=>"jamessampleton", "domain"=>"freshworks.com", "domainagedays"=>"9691", "smtpprovider"=>"g-suite", "mxfound"=>"true", "mxrecord"=>"aspmx.l.google.com", "firstname"=>"james", "lastname"=>"sampleton", "gender"=>"male", "country"=>nil, "region"=>nil, "city"=>nil, "zipcode"=>nil, "processedat"=>"2024-08-26 12:18:56.333"}
```

The primary information in this response is the "status" value. This status value is stored in a custom text field in the contacts module in Freshsales. The other details are added as a note in the respective contact record for reference.

## Do I need credits in ZeroBounce?
Yes, you will need to have credits in ZeroBounce for this integration to work. By default, ZeroBounce offers 100 free credits and their pricing can be check in the below page,
https://www.zerobounce.net/email-validation-pricing/

## Do I need to have a paid plan in Freshsales?
Yes, Freshsales offers APIs in paid plans only and so you will need a paid plan in Freshsales. Following article has information about the API limits in each plan,
https://crmsupport.freshworks.com/support/solutions/articles/50000005599-does-the-web-application-have-api-request-limits-for-an-account-

## Why should I create a custom text field in Freshsales?
A custom text field is required to store the "status" value under the respective contact. The primary reason to create a custom field is so that you can filter contacts based on the values stored in this text field and also export this data from Freshsales whenever required. 

## What happens if I enter an incorrect internal field name for the text field or do not create one?
If you enter an incorrect internal field name or place a dummy value in this field then the status value will not be stored in any contact field and you will not be able to filter or export contacts based on ZeroBounce email validation. The overall response from ZeroBouce will still be displayed under notes.

## For Support drop an email to pulkit.chowdry@freshworks.com
