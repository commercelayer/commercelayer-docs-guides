---
description: How to submit a payment request to Adyen
---

# Sending back the payment details

## Problem

Your customer has submitted credit card information to Adyen — i.e. through one of its [UI components](https://docs.adyen.com/checkout/prebuilt-ui) integrated into your client. You need to submit a payment request with the data returned by the component to proceed with the payment flow.

## Solution

To submit the payment request to Adyen you have to update the Adyen payment object first. To do that, send a `PATCH` request to the `/api/adyen_payments/:id` endpoint, filling the `payment_request_data` attribute with the encrypted credit card details contained in the `state.data` object of the UI component.

### Example

{% tabs %}
{% tab title="Request" %}
The following request updates the Adyen payment source identified by the "emdEKhoOMA" ID with the payment details received from the client:

```javascript
curl -X PATCH \
  http://yourdomain.commercelayer.io/api/adyen_payments/emdEKhoOMA \
  -H 'Accept: application/vnd.api+json' \
  -H 'Authorization: Bearer your-access-token' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
  "data": {
    "type": "adyen_payments",
    "id": "emdEKhoOMA",
    "attributes": {
      "payment_request_data": {
        "payment_method": {
          "type": "scheme",
          "encryptedCardNumber": "adyenjs_0_1_25$0lrKNd0vXuwsvfBiXZRAxe5li79agWcLwvwAKEK9wCEUTjWiPqm35ECE26fOcnulKQZu4zshvqO5m0sTXZiBGu/+a+fgXvH2LoeLUk1HFu1eCfOjywm7sJEp2BqDwBntdeJdGzn+NQ5wjIfqnhnE3mkjrHo2e5Qv8i4KC9w45SF8hCyPAORNhZ8hH/Omw/89E9+x/LEe4JzA6/jnzlkacgC6Xkrn4mXHKDL69RACIPsellPx8qrD5Ek83AFwraVq1XdsBwKgUZjGyFUqBBQMuYITL9MJmt/xmhsmxTJtq0nzSZfcUArNrbXN/6nHiVwgdY+++D6mV5L/GCFy1fQ9vg==$Iz/USqzZJDVMOnwZIfuqlPr7LASrqi0lfsNpFCiXoztORu9JOyC+96tBZwGduXEzi5ICJ4REH6SE5R2DLfnFGiXYXKmZl9dgtHQ6KWg8krzqeB6GVs0zvpS+jiKzc41XmIwdoq26vCNEBH//zNRei2wCJfMhNvclAfQt1cALoT/KeJ5Bxyg0AEMHI6SA7E3m8wjiAOqWFoGao6VI+69jM7KKUB4wyd+KBB17JdqUxUcgor2avVb8tSzvxQAB4f/T3u7PwZENQqVLIsQxRXjQLKwRaFSOHoOlE5aLZXEtRmNJGOFw6GvuD2TrXJYFLwV/LTexaKyvYLNHcVkAtT3mL4/WabN2IJqnSSCVabncNFiPT1FjxKkmmvyHrMgJ8RN3PqZAWvU3Aoce3gtWpj6294ar/NFIk5j19Vcfldr+kqKF+S0QFEsrAGZMevl6kd2osiX3zsUomzPtHfNHCucqy/atEfBiIyff39qevf+6sJOtmBbcYbM2bl67ktJclyg01aJXnigy4baamNDmd74WXM3xAYAmVY//ReO16rnEonQxdxPkIpM+c7xcpkUxnrLuFi9WuuiZSrboo8+4wwqCaKiCz3QxvnJzzKIBKdkkZFwp2LvH4EGv1VTp9um506u0q9n9UKgEM3hs/sVROoDjhABeCGQtN3KNJphlP+k3hs2sEThJ1ryxSrtjCaQZ++1DON+T5MT3HkeVukH7Bw0gNpoWL6EJUvBSjD6Kfh5Eme8FAeQQV+hhi4D7dAqoBo5G6EvVGpJICyDK8BHpLkCv60SRiDjXosdpgNkqQD4ikeouu1dvQP7EAus6Hjo97JXM2HflRjwDH4My5U6cggRCNg==",
          "encryptedExpiryMonth": "adyenjs_0_1_25$PTAXaRhLxVaQySTrgKejerdgRq7gS6QqMldV+WuftuyVPAMp62acmRth02lIAv+s6I0wcAkkx+7eUz12Joxbp8JLinFKusgQaqm5YZMKEjgMwlqAaysDpq1W//jelOw5cRLEEL+mpxZT9lmebDVR9E/1sm4NJTs0X7EKUiFUrO7s1N09tGI1UJ/r4mbht5KY0yqH2IaozeMvZdHcv4UolRu4+kgNDMNshq43jRvKuCyQszdXtvjQ9aQHyDApnS5piFrDe1vmjScX3ePDw0Czz0M5mfV3fDgt9p3KsOIxAHhpBy9R3/RlBykm6pKH8iziNG9ahi8rhAE7ayklnsIJGw==$tFHUYKl2wjXMFzzDjJSDKD3pjRCgaYh169wjc/Uv6CNwL1dyg5lY8Iug9B7cFOpiJXrofpzvxFBgKMxPVdZ8QI7j6y4T4//2oOLbjTkFrZC5shws1AOykiynCpZ94F9Q/FVicYCiLTAzGNvZhGdLTQKJ+mVRLkxwNcJ/aoH/1+OvMo1BHVrN5I+YGkl79in9os+sK7RtRxCRtW6anrkXqwY5dknkhvv0O3OpniqEB6XXlBUn8mcj0ObpmWm93YFfQjY1ycMy6wCMARQi11+QUV3Lk4e7EKNkIxoEmVKAPO384Ga+fyBFdwLZJJaimfBDGPHYxWqORrlA2Dab4O+G5YgMhSVbA9aVYsjti56qVxI5PVtXFxocmsOLYZD+pfWcAZ3IbGCucaNZVTNF0JGfhQFRb9Y+goh33tdT3ACu/ucWhe4Ggb0ftOWM80t0Z8/TdwWo0XHYg2EftoYYZMdlq55X4pAxBvxpNsxTC2KC4SiQDijVZ62rZHSLZEcs97PDtgsKQSKS//EsVHNcf7PLtg==",
          "encryptedExpiryYear": "adyenjs_0_1_25$ZdbZwGiBeViJOVgjeovGCiBTETekAQoFpn+Q3ndcsNSUK4V1rLnSc6lfmQvASUZ8aApNWAAjP5pnOlmVhv4KFVV9TdmPjJnHLCGHDK1iCfkaDEcj1+n1BGDjb0XfX1/hJ0eYEEN+yvOsvXFVa8KAYtOQcgTu1s6SUkYjHn/x+aIvPVHoOC9CUlrI90XctRPJ/6JLW75+4y28ibrL+jlsja95diJThn1A8atmTYLZDO1vKE0nReo2RTVPN0aY20Ey/mnaNN70oN4/Ye5qkjQ1Hm49VpDiuNYAA23f4cityMnFyKlXxDdAn82sIOh5UoOPFRHDV48MH6nq32RVwcb7rA==$beKfnB7PVSKXWdJhq3T0Elm/W7pNfVsoL04KZIfgU3NZc+DEgyJ85FIKrpBzWHiaCSB07+rMAAthc1F+ph8hFLv4SWaUCKNd630KDo6k0EStrrMe98P+zSJxQeBVTbd/lKNUJ7ZY9HLGmGmIQ8UEeELeqt+f8qaVWJEGFZrhlsRsSRjX4WoDGtSBziIUn2ORoOmJG9irrCqlXWHHIyU5ZLMqolM/JWXGxOB4OurK0966R1cbHzQBhmUS8Hiv5rywZbhRpbMGsn5qze3T3ivz5n7TKBhV7+AweC59DakL2tvwvVD5LyWwImAJA/rXzupeLzgabYIeKSTGIDl4ui6C38B7F9ZJNBzdl3FSULzv/oxCLVi7J6LwgenP6o82yApJSvwWxeEtBSLzexedK9VkNEIHHYZ0qTOXjvqHv1i1gBqCRlqOlKIy7JGbUYyfiFrBmWFtNjY6aD5LsnLtgQ3vx60Gp8tWVoQW4R9Xg3+ldRq0qnA/FIWD7rEyP/LS8aP1Zt8bL2DHOwOJNd1Ky7dFX0Q=",
          "encryptedSecurityCode": "adyenjs_0_1_25$yMXJ0hxgqFKfTSPhVq/muoh8EBP+2+dSjr+RA43p2lVeZxI6fJMPXBn+JjGElYYRjtoip2s0hnxcPefke45N+TAWBqDwI+trrOKeTVB+TaFgiomxQX1/cAQq4XHDgbir3R1IKit2Sa6hMLLwgUzHpdYsK2y9J7Etg9H46AsuvnbJEasV36K5rpZhtZy1eo6EojufIBu26WFJDUzEMIRnR6CsD27K0cWrYaxL22DPj5IUqPn7WPKCICm/C6Ol+H2S534tsjystbUPfFz4dCvhNB1urxRPDM3ISb7F4JKnVw056R44vERJKa79dFlnz97oNnDZGpIWCKUrkelifW8+kA==$xHvoXDYRk6Mmo9cOOGziskSnoQ1vkO29lG9q7Uz080Eb3PUMdfdfU07Qr/SbGTQBIZOHvjLEBh4yNbw3qFcCBiSHyO1V+ckkN4z3laI/XdQJPw4zyFhfB1ECszreIZxnt9mdZloiiJluSY7cqUEJ+JaAK2no6Y0S8i/CMaW3gbUq1qb92YNwMizi3q0oomQSfx3aWXtIoIR2feB6lDZ3KA24ljDJIxTG5ggF436g58E8ACTUqI0NGfWULLsWsSY5rsAnl6GH+kpcXTfd6GIK/5eE38LhDGRlcYUHMq0jwc9sTjeYTTnTctk4gEt84Dj2TtgHFBWjGeGqT1/jdVIStu3S4y/zhRTqrTux8JfmgMkCd0pGtlbKWl0q1V+xC/XHsI8zvEJfMxMYeMntwNR1Yx+wc6hLCbnKKaDmaKPSTNL8Z8dnLULc3HbVJYsD8OxNyR/d80apHLzBTL2yCoNhg3AtGuXZEvo+7UTiJJoDWeAhAdoExjFMxA=="
        },
        "origin": "https://checkout.yourdomain.com",
        "return_url": "https://checkout.yourdomain.com/eNoKkhzjyN",
        "browser_info": {
          "acceptHeader": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8",
          "colorDepth": 24,
          "javaEnabled": false,
          "language": "it-IT",
          "screenHeight": 1440,
          "screenWidth": 2560,
          "timeZoneOffset": -120,
          "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:81.0) Gecko/20100101 Firefox/81.0"
        }
      }
    }
  }
}'
```
{% endtab %}

{% tab title="Response" %}
On success, the API responds with a `200 OK` status code, returning the updated Adyen payment object:

```javascript
{
  "data": {
    "id": "emdEKhoOMA",
    "type": "adyen_payments",
    "links": {
      "self": "https://yourdomain.commercelayer.io/api/adyen_payments/emdEKhoOMA"
    },
    "attributes": {
      "payment_methods": {...},
      "payment_request_data": {
        "payment_method": {
          "type": "scheme",
          "encryptedCardNumber": "adyenjs_0_1_25$0lrKNd0vXuwsvfBiXZRAxe5li79agWcLwvwAKEK9wCEUTjWiPqm35ECE26fOcnulKQZu4zshvqO5m0sTXZiBGu/+a+fgXvH2LoeLUk1HFu1eCfOjywm7sJEp2BqDwBntdeJdGzn+NQ5wjIfqnhnE3mkjrHo2e5Qv8i4KC9w45SF8hCyPAORNhZ8hH/Omw/89E9+x/LEe4JzA6/jnzlkacgC6Xkrn4mXHKDL69RACIPsellPx8qrD5Ek83AFwraVq1XdsBwKgUZjGyFUqBBQMuYITL9MJmt/xmhsmxTJtq0nzSZfcUArNrbXN/6nHiVwgdY+++D6mV5L/GCFy1fQ9vg==$Iz/USqzZJDVMOnwZIfuqlPr7LASrqi0lfsNpFCiXoztORu9JOyC+96tBZwGduXEzi5ICJ4REH6SE5R2DLfnFGiXYXKmZl9dgtHQ6KWg8krzqeB6GVs0zvpS+jiKzc41XmIwdoq26vCNEBH//zNRei2wCJfMhNvclAfQt1cALoT/KeJ5Bxyg0AEMHI6SA7E3m8wjiAOqWFoGao6VI+69jM7KKUB4wyd+KBB17JdqUxUcgor2avVb8tSzvxQAB4f/T3u7PwZENQqVLIsQxRXjQLKwRaFSOHoOlE5aLZXEtRmNJGOFw6GvuD2TrXJYFLwV/LTexaKyvYLNHcVkAtT3mL4/WabN2IJqnSSCVabncNFiPT1FjxKkmmvyHrMgJ8RN3PqZAWvU3Aoce3gtWpj6294ar/NFIk5j19Vcfldr+kqKF+S0QFEsrAGZMevl6kd2osiX3zsUomzPtHfNHCucqy/atEfBiIyff39qevf+6sJOtmBbcYbM2bl67ktJclyg01aJXnigy4baamNDmd74WXM3xAYAmVY//ReO16rnEonQxdxPkIpM+c7xcpkUxnrLuFi9WuuiZSrboo8+4wwqCaKiCz3QxvnJzzKIBKdkkZFwp2LvH4EGv1VTp9um506u0q9n9UKgEM3hs/sVROoDjhABeCGQtN3KNJphlP+k3hs2sEThJ1ryxSrtjCaQZ++1DON+T5MT3HkeVukH7Bw0gNpoWL6EJUvBSjD6Kfh5Eme8FAeQQV+hhi4D7dAqoBo5G6EvVGpJICyDK8BHpLkCv60SRiDjXosdpgNkqQD4ikeouu1dvQP7EAus6Hjo97JXM2HflRjwDH4My5U6cggRCNg==",
          "encryptedExpiryMonth": "adyenjs_0_1_25$PTAXaRhLxVaQySTrgKejerdgRq7gS6QqMldV+WuftuyVPAMp62acmRth02lIAv+s6I0wcAkkx+7eUz12Joxbp8JLinFKusgQaqm5YZMKEjgMwlqAaysDpq1W//jelOw5cRLEEL+mpxZT9lmebDVR9E/1sm4NJTs0X7EKUiFUrO7s1N09tGI1UJ/r4mbht5KY0yqH2IaozeMvZdHcv4UolRu4+kgNDMNshq43jRvKuCyQszdXtvjQ9aQHyDApnS5piFrDe1vmjScX3ePDw0Czz0M5mfV3fDgt9p3KsOIxAHhpBy9R3/RlBykm6pKH8iziNG9ahi8rhAE7ayklnsIJGw==$tFHUYKl2wjXMFzzDjJSDKD3pjRCgaYh169wjc/Uv6CNwL1dyg5lY8Iug9B7cFOpiJXrofpzvxFBgKMxPVdZ8QI7j6y4T4//2oOLbjTkFrZC5shws1AOykiynCpZ94F9Q/FVicYCiLTAzGNvZhGdLTQKJ+mVRLkxwNcJ/aoH/1+OvMo1BHVrN5I+YGkl79in9os+sK7RtRxCRtW6anrkXqwY5dknkhvv0O3OpniqEB6XXlBUn8mcj0ObpmWm93YFfQjY1ycMy6wCMARQi11+QUV3Lk4e7EKNkIxoEmVKAPO384Ga+fyBFdwLZJJaimfBDGPHYxWqORrlA2Dab4O+G5YgMhSVbA9aVYsjti56qVxI5PVtXFxocmsOLYZD+pfWcAZ3IbGCucaNZVTNF0JGfhQFRb9Y+goh33tdT3ACu/ucWhe4Ggb0ftOWM80t0Z8/TdwWo0XHYg2EftoYYZMdlq55X4pAxBvxpNsxTC2KC4SiQDijVZ62rZHSLZEcs97PDtgsKQSKS//EsVHNcf7PLtg==",
          "encryptedExpiryYear": "adyenjs_0_1_25$ZdbZwGiBeViJOVgjeovGCiBTETekAQoFpn+Q3ndcsNSUK4V1rLnSc6lfmQvASUZ8aApNWAAjP5pnOlmVhv4KFVV9TdmPjJnHLCGHDK1iCfkaDEcj1+n1BGDjb0XfX1/hJ0eYEEN+yvOsvXFVa8KAYtOQcgTu1s6SUkYjHn/x+aIvPVHoOC9CUlrI90XctRPJ/6JLW75+4y28ibrL+jlsja95diJThn1A8atmTYLZDO1vKE0nReo2RTVPN0aY20Ey/mnaNN70oN4/Ye5qkjQ1Hm49VpDiuNYAA23f4cityMnFyKlXxDdAn82sIOh5UoOPFRHDV48MH6nq32RVwcb7rA==$beKfnB7PVSKXWdJhq3T0Elm/W7pNfVsoL04KZIfgU3NZc+DEgyJ85FIKrpBzWHiaCSB07+rMAAthc1F+ph8hFLv4SWaUCKNd630KDo6k0EStrrMe98P+zSJxQeBVTbd/lKNUJ7ZY9HLGmGmIQ8UEeELeqt+f8qaVWJEGFZrhlsRsSRjX4WoDGtSBziIUn2ORoOmJG9irrCqlXWHHIyU5ZLMqolM/JWXGxOB4OurK0966R1cbHzQBhmUS8Hiv5rywZbhRpbMGsn5qze3T3ivz5n7TKBhV7+AweC59DakL2tvwvVD5LyWwImAJA/rXzupeLzgabYIeKSTGIDl4ui6C38B7F9ZJNBzdl3FSULzv/oxCLVi7J6LwgenP6o82yApJSvwWxeEtBSLzexedK9VkNEIHHYZ0qTOXjvqHv1i1gBqCRlqOlKIy7JGbUYyfiFrBmWFtNjY6aD5LsnLtgQ3vx60Gp8tWVoQW4R9Xg3+ldRq0qnA/FIWD7rEyP/LS8aP1Zt8bL2DHOwOJNd1Ky7dFX0Q=",
          "encryptedSecurityCode": "adyenjs_0_1_25$yMXJ0hxgqFKfTSPhVq/muoh8EBP+2+dSjr+RA43p2lVeZxI6fJMPXBn+JjGElYYRjtoip2s0hnxcPefke45N+TAWBqDwI+trrOKeTVB+TaFgiomxQX1/cAQq4XHDgbir3R1IKit2Sa6hMLLwgUzHpdYsK2y9J7Etg9H46AsuvnbJEasV36K5rpZhtZy1eo6EojufIBu26WFJDUzEMIRnR6CsD27K0cWrYaxL22DPj5IUqPn7WPKCICm/C6Ol+H2S534tsjystbUPfFz4dCvhNB1urxRPDM3ISb7F4JKnVw056R44vERJKa79dFlnz97oNnDZGpIWCKUrkelifW8+kA==$xHvoXDYRk6Mmo9cOOGziskSnoQ1vkO29lG9q7Uz080Eb3PUMdfdfU07Qr/SbGTQBIZOHvjLEBh4yNbw3qFcCBiSHyO1V+ckkN4z3laI/XdQJPw4zyFhfB1ECszreIZxnt9mdZloiiJluSY7cqUEJ+JaAK2no6Y0S8i/CMaW3gbUq1qb92YNwMizi3q0oomQSfx3aWXtIoIR2feB6lDZ3KA24ljDJIxTG5ggF436g58E8ACTUqI0NGfWULLsWsSY5rsAnl6GH+kpcXTfd6GIK/5eE38LhDGRlcYUHMq0jwc9sTjeYTTnTctk4gEt84Dj2TtgHFBWjGeGqT1/jdVIStu3S4y/zhRTqrTux8JfmgMkCd0pGtlbKWl0q1V+xC/XHsI8zvEJfMxMYeMntwNR1Yx+wc6hLCbnKKaDmaKPSTNL8Z8dnLULc3HbVJYsD8OxNyR/d80apHLzBTL2yCoNhg3AtGuXZEvo+7UTiJJoDWeAhAdoExjFMxA=="
        },
        "origin": "https://checkout.yourdomain.com",
        "return_url": "https://checkout.yourdomain.com/eNoKkhzjyN",
        "browser_info": {
          "acceptHeader": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8",
          "colorDepth": 24,
          "javaEnabled": false,
          "language": "it-IT",
          "screenHeight": 1440,
          "screenWidth": 2560,
          "timeZoneOffset": -120,
          "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:81.0) Gecko/20100101 Firefox/81.0"
        }
      },
      "payment_request_details": {},
      "payment_response": {},
      "created_at": "2018-01-01T12:00:00.000Z",
      "updated_at": "2018-01-01T12:00:00.000Z",
      "reference": null,
      "reference_origin": null,
      "metadata": {}
    },
    "relationships": {
      "order": {
        "links": {
          "self": "https://commerce-layer.commercelayer.co/api/adyen_payments/emdEKhoOMA/relationships/order",
          "related": "https://commerce-layer.commercelayer.co/api/adyen_payments/emdEKhoOMA/order"
        }
      }
    },
    "meta": {
      "mode": "test"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Additional notes

#### 3D Secure authentication

In case of specific features enabled in the transaction – e.g. 3DS – you need to get from the client some additional information – such as `browserInfo`, `returnUrl`, etc. – and send them back together with the credit card details at the moment of the payment request submission, as shown in the example above — see [Adyen documentation](https://docs.adyen.com/checkout/3d-secure/redirect-3ds2-3ds1/web-component#make-a-payment) for any reference.

## More to read

See our API reference if you need more information on how to [update an Adyen payment](https://docs.commercelayer.io/api/resources/adyen_payments/update_adyen_payment). 

