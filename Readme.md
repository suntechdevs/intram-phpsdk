# INTRAM PHP SDK



The [PHP](https://www.php.net) library for [INTRAM (intram.org)](https://intram.org).

Built on the INTRAM HTTP API (beta).

## Installation via composer

```sh
composer require intram/php-sdk
```

## API configuration

Setup intram API keys.

```php
$intram = new \Intram\Intram\(
            "5b06f06a0aad7d0163c414926b635ee9cdf41438de0f09d7",
            "pk_9c0410014969f276e8b3685fec7b1b2ab41fc760db29",
            "sk_08bd75f9468b484d8a9f24daddff4638d6513fdcf3f",
            "d8a9f24daddff4638d6513fdcf3f",
            true)
```
Log in to your Intram account, click on Developer, then on API at this level, get the API keys and give them as arguments to the controller.
Initialize Intram PayCfa by entering in order: `PUBLIC_KEY`,` PRIVATE_KEY`, `INTRAM_SECRET`,` INTRAM_MARCHAND_KEY`, `MODE`
The mode: `true` for live mode and `false` for test mode.


##Configure your department / company information



###### Setting Store name
(required)
```php
$intram->setNameStore("Suntech Store"); 
```



###### Setting Store Logo Url

```php
$intram->setLogoUrlStore("https://www.suntechshop/logo.png");
```



###### Setting Store Web site

```php
$intram->setWebSiteUrlStore("https://www.suntechshop");
```



###### Setting Store phone

```php
$intram->setPhoneStore("97000000");
```



###### Setting Store Postal adress

```php
$intram->setPostalAdressStore("BP 35");
```

##Create a request paiement
In order to allow the user to make a payment on your store, you must create the transaction and then send them the payment url or the qr code to scan. 
For that :

###### Add Invoice Items
Add the different products of the purchase (required)
```php
$intram->setItems([
            ['name'=>"T-shirt",'qte'=>"2",'price'=>"500",'totalamount'=>"1000"],
            ['name'=>"trouser",'qte'=>"1",'price'=>"12500",'totalamount'=>"12500"],
        ]);
```

###### Setting TVA Amount
TVA (optional)
```php
$intram->setTva([["name" => "VAT (18%)", "amount" => 1000],["name" => " other VAT", "amount" => 500]]);
```


###### Adding Custom Data
(optional)
```php
$intram->setCustomData([['CartID',"32393"],['PERIOD',"TABASKI"]]);
```



###### Setting Total Amount 
Order total (required)
```php
$intram->setAmount(13600);
```
###### Setting Currency 
Currency of paiement (required)
```php
$intram->setCurrency("XOF");
```

###### Setting Description 
Description of operation (required)
```php
$intram->setDescription("Pretty and suitable for your waterfall");
```


###### Setting Template 
 (required)
```php
$intram->setTemplate("default");
```


###### Setting Store Redirection Url

```php
$intram->setRedirectionUrl("https://www.suntechshop/redirection-url");
```


###### Setting Store Return Url

```php
$intram->setReturnUrl("https://www.suntechshop/return-url");
```


###### Setting Store Cancel Url

```php
$intram->setCancelUrl("https://www.suntechshop/cancel-url");
```


###### Make the payment request

```php
$response = json_decode($intram->setRequestPayment());
```
###### Expected response

```php
{
              +"status": "PENDING"
              +"transaction_id": "5f2d7a96b97d9d3fea912c11"
              +"receipt_url": "localhost:3000/payment/gate/5f2d7a96b97d9d3fea912c11"
              +"total_amount": 1000
              +"message": "Transaction created successfully"
              +"error": false
}
```

###### Get data
```php
$transaction_id = $response->transaction_id;
$status = $response->status;
$receipt_url = $response->receipt_url;
$total_amount = $response->total_amount;
$message = $response->message;
$error = $response->error;
```

##Get transaction status

Give the transaction identifier as an argument to the function (required)
```php
$intram->getTransactionStatus(5f2d7a96b97d9d3fea912c11); 
```

###### Expected response

```php
{
  +"error": false
  +"status": "PENDING"
  +"transaction": {
    +"_status": "PENDING"
    +"_channels": []
    +"_created_at": "2020-09-15T14:56:10.316Z"
    +"_id": "5f60d7a0573bc71854bf095e"
    +"_type": "DEBIT"
    +"_store": {
      +"name": "Sodjinnin Store"
      +"postal_adress": "BP 35"
      +"logo_url": "https://www.google.com/webhp?hl=fr&sa=X&ved=0ahUKEwi7kZ3Qpt_qAhWxC2MBHVFZDDgQPAgH"
      +"web_site_url": "https://www.random.org/"
      +"phone": "94784784"
      +"template": "default"
    }
    +"_reference": "SaCsEhzMFc"
    +"_amount": 1000
    +"_country_code": "BJ"
    +"_qrcode": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHQAAAB0CAYAAABUmhYnAAAAAklEQVR4AewaftIAAAL9SURBVO3BQQ7jCAhFwZcvi6NyKI7KgkyWrCxZdtLdDFWv9wdrDLFGEWsUsUYRaxSxRhFrFL ▶"
    +"__v": 0
    +"invoice": {
      +"custom_datas": []
      +"_created_at": "2020-09-15T14:56:10.335Z"
      +"_id": "5f60d7a0573bc71854bf095f"
      +"currency": "XOF"
      +"items": array:1 [
        0 => {
          +"_id": "5f60d7a0573bc71854bf0960"
          +"name": "short"
          +"price": 500
        }
      ]
      +"taxes": array:1 [
        0 => {
          +"_id": "5f60d7a0573bc71854bf0961"
          +"name": "tva"
          +"amount": 100
        }
      ]
      +"amount": 1000
      +"description": "At vero eos et accusam et justo duo dolores"
      +"transaction": "5f60d7a0573bc71854bf095e"
      +"__v": 0
    }
  }
  +"message": "statut de la transaction"
}
```



# Running Tests
To run tests just setup the API configuration environment variables. An internet connection is required for some of the tests to pass.

## License
MIT
