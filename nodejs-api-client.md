---
description: >-
  The Node.JS library for INTRAM (intram.com). Built on the INTRAM HTTP API
  (beta).
---

# NODEJS API CLIENT

## GENERATE YOUR API KEYS

API keys are your digital references towards Intram systems. We use them to identify your account and the applications you will create. These keys are necessary for any integration of the APIs of Intram's payments APIs. Here are the steps to follow:

* First you need to have an Intram Business account activated. [Create ](https://account.intram.cf/register)one if it is not yet the case.
* [Login](https://account.intram.cf/login) to your account and click on  Developers at the menu level on the left.
* Then click on API, you will see all yours API keys.
* You can switch to `SANDBOX MODE,` or`ENABLE LIVE MODE`.

## INSTALLATION

### INTRAM INSTALLATION VIA THE COMMAND NPM

```
npm install --save intram-nodejs
```

## BASIC CONFIGURATION

[Login](https://account.intram.cf/login) to your account then click on Developers and click on [API](https://account.intram.cf/api) at the menu level left. The APIs key of your merchant account are directly available depending on the mode\('sandbox' or 'live'\) chosen. Retrieve the API keys and give them as arguments to the following methods

{% code title="Setup" %}
```javascript

var intram = require('intram');

var setup = new intram.Setup({
  mode: 'sandbox', // optional. use in sandbox mode.
  marchandKey: 'tpk_5e9469e65341de91988b352eba11f9f0c5f671384e1d6bfb09ce30103',
  privateKey: 'tpk_5e9469e65341de91988b352eba11f9f0c5f671384e1d6bfb09ce30103b',
  publicKey: "5e59e0c34bb8737cedf4c0ec92d9ae94007e33e5c30280596456990d9fc2f60",
  secret: 'tsk_243a7b89fd82a2b4e049c0c8ff39c3012ee6ec70bda3288ad2bf6a1270439c'          
});
```
{% endcode %}

{% hint style="info" %}
**INFOS**

If you are in test use the **sandbox** keys otherwise use the **production** keys and specify the mode by replacing `"sandbox"` with `"live"` in the code above.  
{% endhint %}

{% hint style="success" %}
**BEST PRATICE**

It might usually be suitable to put your API configuration in environment variables. In that case you can initialize intram.Setup without passing configuration parameters. The library will automatically detect the environment variables and use them. Auto-detected environment variables: **INTRAM\_MARCHAND\_KEY**, **INTRAM\_PRIVATE\_KEY**, **INTRAM\_PUBLIC\_KEY**, **INTRAM\_SECRET**
{% endhint %}

### 1- CONFIGURING YOUR SERVICE/ACTIVITY/COMPANY INFORMATION

You can configure your service/activity/company information as shown below. Intram uses these settings to configure the information that will appear on the payment page, PDF invoices, and printed receipts.

Intram offers you several designs for the payment portal. So you can choose a specific template, and add the reference to your store.

 You can also define a color for the payment portal. Only code color is needed

```javascript
var store = new intram.Store({
  name: 'LandryShop', // only name is required
  tagline: "Votre satisfaction, notre priorité.",
  phoneNumber: '229670000000',
  postalAddress: 'Benin-Cotonou Akpakpa',
  websiteURL: 'Benin - Cotonou - Akpakpa',
  logoURL: 'http://www.landryShop/logo.png',
  color:"#a845f4", //for custom paiement portal color,
  template:"default", // Choose the payment portal template that your customers will see
});
```

### 2 - CONFIGURING INSTANT PAYMENT NOTIFICATION

The INSTANT PAYMENT NOTIFICATION is the URL of the file on which you want to receive payment transaction information for possible backoffice processing. Intram uses this URL to send you instantly, by request`POST`, the information relating to the payment transaction.

{% hint style="success" %}
There are two ways to set up the Instant Payment Notification URL: either by going to your Intram account at the level of your app's configuration information or directly in your code.
{% endhint %}

Using the second option offers you the two possibilities below.

#### **2-1 - GLOBAL CONFIGURATION OF INSTANT PAYMENT NOTIFICATION URL**

This instruction should be included in the configuration of your service/activity.

```javascript

var store = new intram.Store({
    name: 'Store at Sandra',
    callbackURL: 'http://www.my-shop.com/fichier_de_reception_des_données_de_facturation'
});
```

#### **2-2 - CONFIGURING THE INSTANT PAYMENT NOTIFICATION URL OR AN INVOICE INSTANCE**

{% hint style="info" %}
This configuration will overwrite the global redirection settings if they have already been defined
{% endhint %}

```javascript
invoice.callbackURL = 'http://www.my-shop.com/fichier_de_reception_des_données_de_facturation'
```

{% hint style="info" %}
The successful validation of the payment transaction returns the structure below containing the customer information, the URL of its Intram invoice in PDF format and also a hash to verify that the received data come from our servers.
{% endhint %}

#### **EXPECTED JSON  ANSWER**

```javascript
 { 
  error: false,
  status: 'PENDING',
  transaction:
   { 
     _status: 'PENDING',
     _channels: [],
     _type: 'DEBIT',
     marchand:
      { 
        _env: 'sandbox',
        _template: [],
        _user_id: '5f64d189a220c61399b360bb',
        _web_site: 'www.oualid_company.com',
        _company: 'oualid company',
        _acronyme: 'LIDOU',
        _base_url_site_web: '',
        _url_checkout: '',
        _url_success: '',
        _url_error: '',
        _email: 'oualid@company.com',
         },
     _store: { name: 'LandryShop' },
     _reference: 'ZMNfTG2b1O',
     _amount: 1000,
     _country_code: 'BJ',
     _qrcode:'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHQAAAB0CAYAAABUmhYnAAAAAklEQVR4AewaftIAAAL2SURBVO3BQY7cUAhF0Zsni6WyKJbKgE6GjL5k2VVKI8758/MPawyxRhFrFLFGEWsUsUYRaxSxRhFrFLFGEWsUsUYRa
                xSxRhFrFLFGuXhIFnxTpXOHLOgqnTtkwTdVOk+INYpYo4g1ysXLKp03yYITWdBVOl2l08mCk0rnpNJ5kyx4k1ijiDWKWKNcfJgsuKPSuaPSeaLSeUIW3FHpfJJYo4g1ilijXAwjC04qnU4WdJXObybWKGKNItYoF7+cLLhDFkwm1ihijSLWKBcfVul
                8UqVzIgtOKp0nKp3/iVijiDWKWKNcvEwWfJMs6Cqdk0qnkwVdpXMiC/5nYo0i1ihijfLn5x8GkwVdpTOZWKOINYpYo1w8JAu6SqeTBV2l08mCrtLpZEFX6ZzIgjtkQVfp3CELukrnRBZ0lc6bxBpFrFHEGuXiwyqdk0rnpNLpZMETsqCrdDpZ0FU6n
                Sy4QxZ8k1ijiDWKWKNcfJgsuKPS6WTBE5VOJwtOKp0nZMEdsqCrdJ4QaxSxRhFrlIuHKp1OFnSVTicLukrnpNLpZEFX6TwhC04qnTsqnRNZ8ElijSLWKGKNcvFllU4nC94kC7pK56TS6WTBSaXTyYITWdBVOp8k1ihijSLWKBcvq3TuqHSekAV3VDq
                TiTWKWKOINcrFQ7LgmyqdrtI5kQVdpdPJghNZ0FU6d1Q6nSw4qXSeEGsUsUYRa5SLl1U6b5IFd8iCOyqdScQaRaxRxBrl4sNkwR2Vzh2y4KTSOZEFXaXTyYI3VTqfJNYoYo0i1igXv1yl08mCE1nQVTpPVDp3yIKu0nmTWKOINYpYo1z8crKgq3Sek
                AVdpdPJgk4WdJXOSaXTyYKu0nlCrFHEGkWsUS4+rNL5pErnCVnwRKVzhyzoKp03iTWKWKOINcrFy2TBN8mCrtLpZMFJpXMiC05kwROyoKt0nhBrFLFGEWuUPz//sMYQaxSxRhFrFLFGEWsUsUYRaxSxRhFrFLFGEWsUsUYRaxSxRhFrlL+fIPw23ey
                uKAAAAABJRU5ErkJggg==',
 invoice :{
        transaction: '5f6a223d768b1875356440cc',
         items : [
                  [
                    'name': 'Chaussures Croco',
                    'quantity': '3',
                    'unit_price': '10000',
                    'total_price': '30000',
                    'description': 'Chaussures faites en peau de crocrodile authentique qui chasse la pauvreté',
                   ],
                   [
                     'name': 'Chemise Glacée',
                     'quantity': '1',
                     'unit_price': '5000',
                     'total_price': '5000',
                     'description': '',
                   ],
           ],
          taxes [
                 [
                  'name': 'TVA (18%)',
                  'amount': '6300',
                 ],
                 [
                  'name': 'Livraison',
                  'amount': '1000',
                 ],
                ],
      'amount' : '42300',
      'currency': 'XOF',
      'custom_data'[
          'categorie' => 'Jeu concours',
          'periode' => 'Noël 2015',
          'numero_gagnant' => '5',
          'prix' => 'Bon de réduction de 50%',
          ]
      'actions' [
          'cancel_url' => 'http://my-shop.com/cancel_url',
          'callback_url' => 'http://my-shop.com/callback_url',
          'return_url' => 'http://my-shop.com/return_url',
          ],
        }
      }
    }
    
```

**IF YOU USE EXPRESS**

```bash
npm add body-parser
```

```javascript
var bodyParser = require('body-parser')
  app.use( bodyParser.json() );       // to support JSON-encoded bodies
  app.use(bodyParser.urlencoded({     // to support URL-encoded bodies
    extended: true
}));
```

### 3 - RECOVERY OF PAYMENT STATUS

```javascript
app.post('/payment-url', function(req, res) {
    var status = req.body.status;
});
```

### 4 - RECOVERY OF THE TOTAL PAYMENT AMOUNT

```javascript
app.post('/payment-url', function(req, res) {
    var amount = req.body.transaction.invoice.amount;
});
```

## APIs

### INITIALIZATION OF A PAYMENT 

This Api service allows you to create an invoice and redirect your customer to our platform so that it can complete the payment process. We recommend using this API because it is the most suitable in 99% of cases. The main advantage of this option is that customers can choose to pay from a variety of payment options available on our platform. In addition, if a new option is added in the future, it will appear directly on the payment page without you having to change anything in your source code.

```javascript
// Do this if you want to redirect your customers to our website so that they can complete the payment process
// It is important to note that the constructor requires respectively as parameters
// an instance of the intram.Setup and intram.Store classes
var invoice = new intram.CheckoutInvoice(setup, store);
```

### 1 - ADDING PAYMENT INFORMATION

####       **1-1 - ADDING ARTICLES AND INVOICE DESCRIPTION:**

It is important to know that billing items are primarily used for presentation purposes on the payment page. Intram will not use any of the amounts declared to bill the customer. To do this, you must explicitly use the `SetTotalAmount` method of the API to specify the exact amount to bill the customer.

```javascript
// Adding items to your bill is very basic.
// The expected parameters are product name, quantity, unit price,
// the total price and an optional description.
invoice.addItem('Croco shoes', 1, 10000, 30000, 'Shoes made of genuine crocodile skin that hunts poverty');
invoice.addItem('Ice Shirt', 1, 5000, 5000);
```

{% hint style="info" %}
You can optionally define a general invoice description that will be used in cases where you need to include additional information in your invoice.
{% endhint %}

```javascript

invoice.description = "Optional Description";
```

####      **1-2 - CONFIGURATION OF THE TOTAL AMOUNT OF THE INVOICE:**

Intram expects you to specify the total amount of the customer's bill. This will be the amount that will be billed to your client. We consider that you have already done all calculations at your server before setting this amount.

{% hint style="info" %}
Intram will not perform calculations on its servers. The total amount of the invoice set from your server will be the one that Intram will use to bill your customer.
{% endhint %}

```javascript
invoice.totalAmount = 42300;
```

####  **1-3 - CONFIGURATION OF THE CURRENCY OF THE INVOICE:**

Intram expects you to specify the currency of the customer's bill. This will be the currency that will be attached to your client  billed. 

{% hint style="success" %}
Currently, the available currency are:

* XOF
* USD
* EUR
{% endhint %}

```javascript
invoice.currency = 'XOF';
```

### 2 - REDIRECTION TO THE INTRAM PAYMENT PAGE

After adding items to your invoice and configuring the total invoice amount, you can redirect your customer to the payment page by calling the `create` method from your invoice object`invoice`. Please note that the `invoice.create()` method returns a boolean \(true or false\) depending on whether the invoice was successfully created or not. This allows you to put a `if - else` statement and handle the result as you see fit.

```javascript
// The following code describes how to create a payment invoice at our servers,
// then redirect the customer to the payment page
// and then post his payment receipt if successful.
invoice.create()
  .then(function (){
    console.log(transaction.invoice.status);
    console.log(transaction.invoice.transaction); // Bill Token
    console.log(transaction.invoice.message);
    console.log('https://account.intram.cf/payment/gate/'+invoice.transaction); // Intram invoice payment redirection URL
  })
  .catch(function (e) {
    console.log(e);
  });
```

### ADDITIONAL API METHODS

#### 1 - ADDING OF TAXES \(OPTIONAL\)

You can add tax information on the payment page. This information will then be displayed on the payment page, PDF invoices and printed receipts, electronic receipts.

```javascript
// The parameters are the name of the tax and the amount of the tax.
invoice.addTax('TVA (18%)', 6300);
invoice.addTax("Livraison", 1000);
```

#### 2 - ADDING ADDITIONAL DATA \(OPTIONAL\)

If you need to add additional data to your payment request information for future use, we offer you the option of backing up custom data on our servers and recovering it once the payment is successful.

{% hint style="info" %}
Custom data is not displayed anywhere on the payment page, invoices/receipts, downloads, and impressions. They are only retrieved using our `confirm` callback action at the API level
{% endhint %}

```javascript
// Custom data allows you to add additional data to your billing information
// that can be retrieved later using our callback Confirm action
invoice.addCustomData("category", "contest");
invoice.addCustomData("period", "Christmas 2015");
invoice.addCustomData("numero_gagnant", 5);
invoice.addCustomData("price", "50% discount coupon");
```

#### 3 - RESTRICTION OF THE MEANS OF PAYMENT TO DISPLAY \(OPTIONAL\)

By default, the payment methods activated in your integration configuration will all be displayed on the payment page for all your invoices.

However, if you want to keep the list of payment methods to display on the payment page of a given invoice, we offer you the possibility to do so using the methods `addChannels`.

{% hint style="success" %}
Currently, the available means of payment are:

`MOOV`, `MTN-BENIN`, `VISA`,  `INTRAM`.
{% endhint %}

```javascript
// Addition of several payment methods at a time
invoice.addChannels(['moov-benin', 'mtn-benin', 'visa'])
```

### CONFIGURING A REDIRECT URL AFTER CANCELING PAYMENT

#### **1-1 - CONFIGURING A REDIRECT URL AFTER CANCELING PAYMENT**

You can optionally define a URL to which your customers will be redirected after an order cancellation.

{% hint style="info" %}
There are two ways to configure the order cancellation URL: either globally at the configuration information level of your application or by order.
{% endhint %}

{% hint style="danger" %}
**Using the second option overwrites the global settings if they have already been defined.**
{% endhint %}

```javascript
var store = new intram.Store({
name: 'Alexandra',
cancelURL: 'http://alexandra.com/cancel_url'
});
```

#### **1-2 - CONFIGURING THE REDIRECTION URL AFTER CANCELING PAYMENT ON AN INVOICE INSTANCE**

{% hint style="danger" %}
This configuration will overwrite the global redirection settings if they have already been defined.
{% endhint %}

```javascript
invoice.cancelURL = 'http://magasin-le-choco.com/cancel_url';
```

#### **1-3 - CONFIGURING A REDIRECT URL AFTER PAYMENT CONFIRMATION**

Intram does a great job of managing downloads and receipt printing of payments after your customer has successfully paid for their order. However, there may be cases where you would want to redirect your customers to another URL after they have successfully paid for their order. The configuration below meets this need.

{% hint style="info" %}
Intram will add `?token=invoice_token` to your URL. We will explain how to use this token in the next chapter.
{% endhint %}

#### **1-4 - GLOBAL CONFIGURATION OF THE REDIRECT URL AFTER PAYMENT CONFIRMATION.**

This instruction should be included in the configuration of your service/activity

```javascript
var store = new intram.Store({
  name: 'Alexandra',
  returnURL: 'http://alexandra.com/return_url'
});
```

#### **4-5 - CONFIGURING THE REDIRECT URL AFTER CONFIRMING PAYMENT ON AN INVOICE INSTANCE**

{% hint style="danger" %}
This configuration will overwrite the global redirection settings if they have already been defined.
{% endhint %}

```javascript
invoice.returnURL = 'http://magasin-le-choco.com/return_url';
```

## CHECKING PAYMENT STATUS

Our API allows you to check the status of all payment transactions using the invoice token. You can therefore keep your invoice token and use it to check the payment status of the invoice. The status of an invoice can be either `PENDING (PENDING)` , `CANCELED (CANCELED)` , or `COMPLETED (COMPLETED)` depending on if yes or not the customer has paid the bill.

{% hint style="info" %}
However, this option is suitable for PAR payments as it would allow you for example to know the payment status of your bill even if the customer is still on our payment page.
{% endhint %}

```javascript

// Intram will automatically add the invoice token as a QUERYSTRING "token"
// if you have configured a "return_url" or "cancel_url".
var token = 'test_VPGPZNnHOC';

var invoice = new intram.CheckoutInvoice(setup, store);
invoice.confirm(token)
.then(function (){
  // Retrieve payment status
  // The payment status can be either completed, pending, cancelled
  console.log(invoice.status);

  console.log(invoice.responseText);  // Server response

  // The following fields will be available if and
  // only if the payment status is equal to "completed".

  // You can retrieve the name, the email address and the
  // customer's phone number using the following object
  console.log(invoice.customer); // {name: 'Alioune', phone: '773830274', email: 'aliounebadara@gmail.com'}

  // URL of the electronic PDF receipt for download
  console.log(invoice.receiptURL); // 'https://app.intram.com/sandbox-checkout/receipt/pdf/test_VPGPZNnHOC.pdf'
})
.catch(function (e) {
  console.log(e);
});
```

