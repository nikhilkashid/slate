---
title: Coinn Documentation

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:

search: true
---

# Introduction

Welcome to the Coinn API! You can use our API along with Coinn Android SDK integration to authorize and enable delivery boys to collect payments from customers using the Coinn mobile application. Integration of Coinn within your application ensures a simple and convenient end to end automation of the collection of payments from customers in a Cash on delivery scenario. All the delivery boy has to do is ask the customer to "Pay by Coinn". The API also help in capturing delivery related information such as Customers current location, time of delivery, time taken to complete the payment and order

We have language bindings in curl for now. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Getting Started

To start accepting payments using Coinn, you need:

* API keys that can be generated through [Coinn's dashboard](http://example.com/developers)
* Coinn Delivery SDK for Android integrated in your app for the delivery boy
* Order creation and payment capturing process in your backend

The process of accepting payments from your end customers is as follows:

* Once the order is created, customer is notified with a SMS about the option of paying by Coinn
* As soon as delivery boy reaches the point of delivery, Coinn app in customer's phone notifies the customer with bill details
* Customer fills his/her payment details and authorizes the payment
* Order creation API hands over to you the order_id
* Your server side backend uses the order_id to capture the payment
* You get the money in your bank account in T+1 days.

# Payment Flow

It is required that the Coinn delivery SDK for Android is integrated with the app provided to the delivery boy. Once the integrations is completed, below mentioned flow is followed:

**Register Mobile Device** 

At the time of registration or signin of the delivery boy app, Coinn SDK checks for the delivery boy details and registers or updates the details of the delivery boy and enables the mobile device to function as a Coinn beacon. At this point, if the delivery boy's mobile device is not compatible or Coinn beacon mode fails, the merchant admin will be notified and provided with a sticker to be stuck on delivery boy's mobile device to enable Coinn beacon.

**Order Creation** 

An order is created using a server to server call once the delivery boy accepts the request to pick up a Cash on Delivery order. Order creation request will contain the phone number of delivery boy assigned, delivery location coordinates, name of the merchant, phone number of the customer and amount to be paid by the customer. In case customer is not a Coinn user, he/she will be notified about the option to pay by Coinn via SMS. 

**Delivery Boy Location Update** 

Once the order is created, the location of delivery boy will be updated periodically on Coinn server. This updated location will be used to identify delivery boy's proximity to customer location and enable the bluetooth of the delivery boy's mobile device.

**Accepting Payments using Coinn** 

Once the delivery boy is at customer location, customer will be able to see Merchant name from the corresponding order in Coinn app. Customer fills his/her payment details and authorizes payment. Once the payment is completed, the status of the order will be changed and delivery boy will be notified about the status of the payment.

# Libraries and Integration

We have SDK for android which should be integrated with the delivery boy app to accept payments through Coinn. Please follow the steps given below to integrate Coinn SDK in your app and start accepting payments through Coinn. 

You can find the sample app at - https://github.com/coinnpayments/coinn-delivery-sdk-sample

## Installation

Please be sure to keep your app up to date with the latest version of the SDK. All releases follow semantic versioning. Make sure you have a jcenter( ) / mavenCentral( ) entry in your repositories like :

<aside class="codetext">

 repositories  <br>
 
    {  <br>
 
 &emsp; jcenter( ) <br>
 
 }

</aside>
   

To use this library in your android project, just simply add the following dependency into your build.gradle

<aside class="codetext">

 dependencies  <br>
 
    {  <br>
 
 &emsp;     compile 'in.coinn:coinn_delivery_sdk:0.1.6'<br>
 
 }

</aside>

## Permissions

Integrating Coinn SDK requires following permissions for different aspects of library. The following needs to be added in AndroidManifest.xml file 

<aside class="codetext">

  <br>&lt;!--  Permission to manage Bluetooth settings of the device - -&gt;<br>
    <br>&lt;uses-permission android:name="android.permission.BLUETOOTH" /&gt;<br>
    &lt;uses-permission android:name="android.permission.BLUETOOTH_ADMIN" /&gt;<br>
    &lt;uses-permission android:name="android.permission.READ_PHONE_STATE" /&gt;<br>

    <br>&lt;!-- Permission to connect to internet from the handset --&gt;<br>
    &lt;uses-permission android:name="android.permission.INTERNET" /&gt;<br>

 <br>    &lt;!-- Permission to acess the last known location of the delivery boy --&gt;<br>
     &lt;uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" /&gt;<br>
    &lt;uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/&gt;<br>
</aside>


<aside class="notice">
Please ensure all the permissions are granted for mobile devices running on Marshmallow
</aside>

## Usage

Once the library is installed in your Android project, the following needs to be implemented to start accepting payments through Coinn

### Initialise Coinn SDK

1. Create an object for CoinnClient.

<aside class="codetext">
 CoinnClient coinnClient = CoinnClient.getInstance(Context);
</aside>

2. Pass merchant parameters to initialise the SDK. The paramesters includes Client ID, Client Secret and Environment 

<aside class="codetext">
coinnClient.init("4e074b1492218f8f4af2", "7fc19dc5355cbbb8c754a61551301920d677c6ee", CoinnEnvironment.SANDBOX); 
</aside>

where Client ID is 4e074b1492218f8f4af2 and Client Secret is 7fc19dc5355cbbb8c754a61551301920d677c6ee

### Start accepting Payments

To start accepting payments through Coinn, please follow the following steps to complete the integration

1. Send delivery boy info to Coinn. With this call we will check if the delivery boy is registered with Coinn to start accepting payments through Coinn. If the user is not registered with Coinn, he/she will be registered when the below mentioned function is called. This function is to be called whenever delivery boy logins to delivery boy app. 
 
<aside class="codetext">
coinnClient.registerAtCoinn("9619020263", "Nikhil", "nikhil@delivery.in"); 
</aside>
 
 - where First Parameter is Mobile Number of delivery boy
 - where Second Parameter is Name of delivery boy
 - where Third Parameter is Email-ID of delivery boy (optional)

2.  Update location of delivery boy. This needs to be called whenever there is a change in the delivery boys location (onLocationChanged()). This call ensures that we have the updated location of the delivery boy and whenever the delivery boy is in vicinity of the customer, his device's bluetooth will be turned on in the background and the delivery boy can accept payments through Coinn from the customer. We recommend that location is updated and this function is called whenever there is a change in location by 100 m or the last known location is  3 mins older.

<aside class="codetext">
        coinnClient.updateLocation(19.125051, 72.912406);
</aside>
 - where First Parameter is Latitude. Value is of type Double
 - where First Parameter is Longitude. Value is of type Double

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: OAuth CLIENT_ID:CLIENT_SECRET
```

> Make sure to replace `CLIENT_ID:CLIENT_SECRET` with your API client ID and secret.

All the server side requests like create order, get order status, getting details of previous orders must be authenticated with Oauth auth using "client-id:client-secret" as the auth token. The API request header will look like the following:

`Authorization: OAuth CLIENT_ID:CLIENT_SECRET`

`Content-Type: application/json`


<aside class="notice">
You must replace <code>CLIENT_ID:CLIENT_SECRET</code> with your personal API client ID and secret.
</aside>


# API Reference

## Create Order 


```shell
curl "http://52.74.124.11:9669/api/v2.1/order/"
-H "Authorization: OAuth ce74318717114c6328d3:ffe2f53d62f443435f16e30e423921a4391127fb9"
-H "Content-Type: application/json"
-d 
   '{
      "customer_phone":"9619020263",
      "delivery_boy_phone":"9455821330",
      "bill_number":"ORDER1",
      "delivery_location":
        {
        "latitude":19.132911,
        "longitude":72.917471
        },
      "vendor":"newVendor",
      "external_merchant":"YummyFood",
      "payment":2
    }'
```
> The above command returns JSON structured like this:

```json
{
  "added_on": "2016-01-28T12:51:04.370784",
  "app_installed": true,
  "bill_details": "",
  "bill_number": "ORDER1",
  "customer_email": null,
  "customer_phone": "+919619020263",
  "delivery_boy_phone": "+919455821330",
  "delivery_location": "{\"latitude\": 19.132911, \"longitude\": 72.917471}",
  "delivery_reached_at": null,
  "external_merchant": "YummyFood",
  "external_merchant_location": null,
  "id": 61,
  "order_status": {
    "description": "Order started.",
    "id": 1,
    "status": "STARTED",
    "status_code": 0
  },
  "order_status_reason": null,
  "payment": "2",
  "resource_uri": "/api/v2.1/order/61/",
  "uuid": "39518bd8f76e4a8bb7939f670ffb1f1b",
  "vendor": "newVendor"
}
```
This endpoint captures the order for which payment is to be collected post delivery. The POST request should be call when an order with post delivery payment is recieved and assigned to the delivery boy.
### HTTP Request

`POST http://52.74.124.11:9669/api/v2.1/order/`

### Query Parameters

Parameter | Mandatory/Optional | Description
--------- | ------- | -----------
customer_phone | Mandatory | This phone number will be used to determine if the customer is a Coinn user and to notify him about the order and bill details
delivery_boy_phone | Mandatory | This phone number will be used to identify the delivery guy assigned to deliver a particular order.
bill_number | Mandatory | Unique identifier provided by merchant for each order which can be used to determine order related items such as order status for the corresponding order
delivery_location | Mandatory | Approximate Latitude and Longitude of the delivery location, to be provided as a JSON object. This will be used to trigger Coinn mode when delivery boy is in proximity of the delivery location
vendor | Optional | Name of the third party vendor/aggregator of merchants eg:- TinyOwl/Foodpanda etc. This will be displayed to the customer on his Coinn app when he is ready to pay
external_merchant | Mandatory | Name of the merchant on whose behalf the order is being delivered
external_merchant_location | Optional | Location of the merchant from where the order is being picked up
payment | Mandatory | Total bill amount to be paid by the customer in Rupees


<aside class="success">
Remember- The request is to be authenticated with "CLIENT_ID:CLIENT_SECRET"
</aside>
<aside class="success">
Remember- The amount to be paid should be provided in Rupees"
</aside>

## Update Order Status

```shell
curl "http://52.74.124.11:9669/api/v2.1/order/61/"
-H "Authorization: OAuth ce74318717114c6328d3:ffe2f53d62f443435f16e30e423921a4391127fb9"
-H "Content-Type: application/json"
-d '{"status_code":5,"order_status_reason ":"ok"}'
```

> The above command returns JSON structured like this:

```json
{
   "added_on": "2016-01-28T12:51:04.370784",
   "app_installed": true,
   "bill_details": "",
   "bill_number": "ORDER1",
   "customer_email": null,
   "customer_phone": "+919619020263",
   "delivery_boy_phone": "+919455821330",
   "delivery_location": "{\"latitude\": 19.132911, \"longitude\": 72.917471}",
   "delivery_reached_at": null,
   "external_merchant": "YummyFood",
   "external_merchant_location": null,
   "id": 61,
   "order_status": {
       "description": "Transaction wholly settled by cash payment",
       "id": 6,
       "status": "COMPLETED-CASH_PAYMENT",
       "status_code": 5
   },
   "order_status_reason": "ok",
   "payment": "2",
   "resource_uri": "/api/v2.1/order/61/",
   "uuid": "39518bd8f76e4a8bb7939f670ffb1f1b",
   "vendor": "newVendor"
}
```

This endpoint updates the order status. This api is to be called to update the order status if the transaction amount was partially/wholly settled through cash payment by the customer. This can happen in following two cases:

* Customer pays partial amount through Coinn and settles rest of the amount by Cash
* Customer opts to pay entire amount by Cash instead of using Coinn

<aside class="success">The order status will updated automatically for all completed transactions. This API is to be called only to handle cases when the payment is not entirely completed via Coinn and customer opts not to use Coinn or pays only partial amount via Coinn </aside>

### HTTP Request

`PATCH http://52.74.124.11:9669/api/v2.1/order/61/<ID>`

### URL Parameters

Parameter | Mandatory/Optional | Description
--------- | ------- | -----------
ID | Mandatory | The ID of the order to retrieve
status_code | Mandatory | Code to indicate the status to be updated
order_status_reason  | Mandatory | Text giving reason for the update of status. Eg:- Customer could not download app etc

# Return URL


```shell
curl "http://http://52.74.124.11/test.php"
-H "Content-Type: application/json"
-H "Accept: * / *"
-d 
'{
    "order_status": 
        {
            "description": "Order completed. Payment done.",
            "status": "COMPLETED",
            "status_code": 1
        }, 
    "uuid": "39518bd8f76e4a8bb7939f670ffb1f1b", 
    "payment": "2.00", 
    "transaction_amount":"2.00",
    "bill_number":"ORDER1"
  }'
```
> The above command will be Posted by Coinn on completion of transaction. In this POST request, the key "payment" gives amount to be collected entered during order creation. "transaction_amount" is the amount paid by the customer through Coinn.

Return URL is where Coinn sends the data related to transactions so that you can check the status of the transaction. On completion of every transaction through Coinn this url will be called to post the transaction status. This URL can be configured in [Settings](http://example.com/developers)
This page is to be hosted on your server to enable Coinn to post notifications regarding the transaction when it is completed. The status will be sent as a POST request with JSON body to the configured URL 

### HTTP Request

`POST http://http://52.74.124.11/test.php`

### Query Parameters

Parameter | Description
--------- |-----------
order_status | Text field giving the order status
status_code | Integer field giving the code for the order status
uuid | Unique ID associated with the order. This will be a alpha numeric String
payment | Amount paid in Rupees. This will be sent as a String


<aside class="success">
Remember- The Return URL should be configured by the merchant in his Settings section
</aside>

# Status Codes

The following status codes will be used to indicate the order status at any given point of time. The order will be considered for Status code 1,2,3,4 and 5

Status Code | Status | Description
--------- | ------- | -----------
0 | STARTED |  This indicates that the order has been freshly created and no transaction has not started yet
1 | COMPLETED | This indicates successful payment of the full amount and thus completion of the order
2 | CANCELLED | This indicates that the user had initiated the payment and canclled it later before completing the authorisation. In such a case, the update order status request (mentioned above) should be called from merchant server to mark the order as completed
3 | FAILED | This indicates that the customer had intiated the transaction but the transaction failed midway. In this case the order will be considered as comlpeted
4 | PARTIAL_COMPLETED | This indicates that the order amount was partially payed by Coinn and rest was paid by cash by the customer
5 | COMPLETED_CASH_PAYMENT | This indicates that the order amount was entirely payed by Cash

# Error Codes

The Coinn API returns json with an error code and description in case of any error during the request as shown. All successful responses are returned with HTTP Status code 200.

### HTTP Error Codes Reference

Status Code | Status | Description
--------- | ------- | -----------
200 | OK |  Transaction completd successfully 
400 | BAD REQUEST | Bad request to the API due to invalid field or wrong format etc
401 | UNAUTHORIZED | Invalide authorisation 
404 | NOT FOUND | The requested URL was not found 
502,500,504 | SERVER ERROR | Server side error or internal Coinn errors found and captured