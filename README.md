# Importing a profile to Postman

The instructions will help you import profile data to work with Altcraft Platform via Postman.

## Installing and running Postman

1. First, make sure you have **Postman** installed. You can download it from the official website: (https://www.postman.com/downloads/)

2. Run **Postman**.

3. Create a new request. In Postman, click the **“Home”** button in the upper left corner.

![1](https://github.com/user-attachments/assets/acfcd14e-47b4-4134-9dc9-8d23aada030a)

Click the **“New request”** button in the window that opens.

![2](https://github.com/user-attachments/assets/59be985d-23bf-4829-b3cf-e08609ef4e42)

4. Fill in the request parameters
In the **“Request URL”** field, enter the API URL:
(https://demo.altcraft.com/api/v1.1/profiles/import)
Select the **“POST”** method for your request.

![3](https://github.com/user-attachments/assets/856b3208-8c2c-413d-810a-df26325c772c)

Go to the **“Body”** tab, located under the URL field, and select the **“raw”** mode. Make sure that the format is **“JSON”**

![4](https://github.com/user-attachments/assets/b3fd3d4c-542b-45d7-81fc-4a964d6bf283)

![cropped](https://github.com/user-attachments/assets/d1aac08d-6bbe-44f1-840f-fb3f2b75e647)

When using JSON in the request body, you must use the Content-Type: application/json header.
To do this, go to the **"Headers"** section. In the **“Key”** column, enter “Content-Type:”, and in the **“Value”** column, enter “application/json”. Then click **“Save”**.

![5](https://github.com/user-attachments/assets/ae0cb126-4238-4cdb-aa63-0d052de274e8)

## Writing a request
To do this, go back to the **“Body”**, **“raw”** tab. Let’s say you want to import a client’s profile via their email address. Then you should specify email in the matching field. Also known:

• **token** - c7f55f8f24204b9f91bfaaedda052e49,
• **db_id** - 1,
• **_fname** – John,
• **_Iname** – Doe

Let's make a request:

 {
 "token": "c7f55f8f24204b9f91bfaaedda052e49",
 "db_id": 1,
 "matching": "email",
 "email": "john@example.com",
 "skip_triggers": true,
 "skip_invalid_subscriptions": true,
 "detect_geo": true,
 "data": {
 "_fname": "John",
 "_lname": "Doe"
 "_bdate": "YOUR_BDATE_HERE"T21:00:00Z"
}
}

![6](https://github.com/user-attachments/assets/532e2a0b-ebfc-419f-9cf6-c7005cb1bfc4)

• **"token"** — API token;
• **"db_id"** — database identifier;
• **"matching"** and **"email"** — parameter for searching for a profile in the database. In this case, an email address is used to search for a profile. If a profile with such an address already exists in the database, the request will update its data; if not, a new profile will be created. The "matching" parameter can take various values. However, it is not mandatory and, if not specified, the profile will be searched by email;
• **"skip_triggers"** - skip triggers
(default – false);
• **"skip_invalid_subscriptions"**- skip invalid subscriptions
(default – false);
• **"detect_geo"** - enables automatic detection of geo data by the _regip or _ip field in data;
• **"data"** — an object containing profile data.
• **"_fname"**, **"_lname"** – the client's first and last name.
• **"_bdate"** – the client's date of birth in the format "1990-01-15T12:00:00Z".

Replace **"YOUR_BDATE_HERE"** with the correct date of birth in ISO 8601 format (YYYY-MM-DD). To get the date 20 years ago, you need to use Pre-request Script in Postman. Go to the **“Scripts”** tab, **"Pre-request Script"** and paste the following JavaScript code:

{
let today = new Date();
let pastDate = new Date();
pastDate.setFullYear(today.getFullYear() - 20);
let formattedDate = pastDate.toISOString().slice(0, 10);
pm.environment.set("pastDate", formattedDate);
}

![7](https://github.com/user-attachments/assets/2f117dae-f978-4217-a6a7-c48eb7206fc6)

Go back to the **“Body”** tab. After that, replace **"YOUR_BDATE_HERE"** with **{{{pastDate}}** in the JSON request. This script calculates the birth date 20 years ago and stores it in the variable **pastDate**. Postman uses double curly braces for variable substitution.

![8](https://github.com/user-attachments/assets/836506a4-6e9f-42f7-8e2c-1d0ccf0a6206)

6. Supplement the request with other customer profile data.
The request will look like this (you can copy it and paste it into the **Body** field):

{
"token": "c7f55f8f24204b9f91bfaaedda052e49",
"db_id": 1,
"matching": "email",
"email": "john@example.com",
"skip_triggers": true,
"skip_invalid_subscriptions": true,
"detect_geo": true,
"data": {
"_fname": "John",
"_lname": "Doe",
"_bdate": "{{pastDate}}T21:00:00Z",
"_sex": 0,
"_regdate": "2024-10-27T10:00:00Z",
"_regip": "192.168.1.1",
 "_ip": "192.168.1.1",
 "_tz": "Europe/Moscow",
 "_postal_code": "12345",
 "_os": "Windows 10",
 "_browser": "Chrome",
 "_vendor": "form_#31",
 "phones": ["+79000000000"],
 "subscriptions": [
 {
 "channel": "email",
 "email": "john@example.com",
 "resource_id": 1,
 "custom_fields":
 {
 "_browser_name": "Chrome",
 "_device_type": "web"
 }
 }
 "cats": [
 "category_1",
 "category_2"
 ]
 },
 {
 "channel": "sms",
 "phone": "+79000000000",
 "resource_id": 2
 },
 {
 "channel": "push",
 "subscription_id": "abcdefghijklmnqrstuvwxyz",
 "provider": "android-firebase",
 "resource_id": 2
 },
 {
 "channel": "telegram_bot",
 "cc_data": {}
 },
 {
 "channel": "whatsapp",
 "cc_data": {
 "phone": "+79000000000"}
 },
 {
 "channel": "viber",
 "cc_data": {
 "phone": "+79000000000"}
 }
 ]
}
}

New fields:
• **"_sex"** — gender, "0" for male, "1" for female;
• **"_city"** — city;
• **"phones"** — phone number ;
• **"subscriptions"**– an array that stores data about profile subscriptions to resources. One object is one subscription;
• **"channel"** – channel type, for example, "email", "sms", " push";
• **"resource_id"** — resource identifier;
• **"cc_data"** - chat id for Telegram bot, profile phone for WhatsApp or Viber.

## Sending a request

Click the **"Send"* button * top right.

![9](https://github.com/user-attachments/assets/58206609-7f4f-46d5-b858-c30556744fb3)

## Checking the result

Look at the response code (Status Code) at the bottom of the window. Code 200 OK or something like this usually means a successful import.
Check the response body **“Body”** at the bottom of the screen for any error messages or additional information. The value of **“profile_id”** is the ID of the imported profile. You can copy its value and use to check if the import is correct.

![10](https://github.com/user-attachments/assets/871a283f-4d52-4d19-ba72-d48f1a4f0e60)

If there are errors, check the server's response to error messages. Pay attention to the syntax your request. Extra characters or unclosed brackets may cause an error.
If something went wrong, the response message will contain the error code and its description:

![11](https://github.com/user-attachments/assets/c54d8859- c621-40b4-b57a-e53699668f3a)

If you were unable to resolve the error yourself, please send the error code and description to Altcraft support:
(https://altcraft.com/ru/help-center).

Checking the import (Optional step)
Create **“New Request”** as described above. Use the Post method (select from the drop-down menu on the left).
In the **“Request URL”** field, enter the API URL:
(https://demo.altcraft. com/api/v1.1/profiles/get).

In the **“Body”**, **“raw”** field, insert the query:

{
"token": "c7f55f8f24204b9f91bfaaedda052e49",
"db_id": 1,
"matching": "profile_id",
"profile_id": "675f30d95ec2d7d61fd01e6a" }

You will need to find the imported profile ID in the response to the previous POST request. Then replace token, db_id and profile_id with the actual profile ID.

![12](https://github.com/user-attachments/assets/7bc7dd55-8dd4- 402c-a1e6-7d17340a0d6d)

Click **“Send”**. If the import is successful, the **“Body”** field will show the code 200 OK and information about the imported profile, as well as the message "error_text": "Successful operation"

![13](https://github.com/user-attachments /assets/dbfdbb1d-5632-4bf9-ae6d-e41e09fe1fd6)

![14](https://github.com/user-attachments/assets/8ac5ead2-0f34-421d-add3-1417738baaaa)
