---
description: >-
  The JavaScript SDK is designed to generate the payment solution within your
  web application without reloading the page.
---

# JAVASCRIPT

{% hint style="info" %}
Le SDK Javascript ne permet pas le traitement des données de facturation. Vous devez utilisé un SDK BACKEND pour générer un lien de facturation qui vous servira ensuite après une requête HTTP.
{% endhint %}

Toute utilisation du SDK JAVASCRIPT requiert un compte marchand valide et un lien de facturation. Si vous n'en disposez pas encore [créez-en un](https://account.intram.org/register) dès maintenant. L'intégration du SDK JAVASCRIPT se fait en important le script qui suit avant la fermeture de la balise fermante`</body>` 

```markup
<script src="https://cdn.intram.org/sdk-javascript.js"></script>
```

Une fois le SDK importé dans votre application web vous pouvez l'utiliser de cette manière : 

```javascript
intramOpenWidget({
    billing_link:'https://account.intram.org/payment/gate/5f6a223d768b1875356440cc', //le lien de paiement
    key:'5e59e0c34bb8737cedf4c0ec92d9ae94007e33e5c30280596456990d9fc2f60' //votre clé public
})
```

{% hint style="success" %}
EXEMPLE AVEC AJAX
{% endhint %}

```javascript
 $.ajax(
 {
       url: 'url',
       method: "POST",
       data: data,
       success: function (data) 
         {
             intramOpenWidget({
                billing_link:data.receipt_link,
                key:'5e59e0c34bb8737cedf4c0ec92d9ae94007e33e5c30280596456990d9fc2f60'
             })
          }
});
```

