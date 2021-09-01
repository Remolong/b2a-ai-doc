# Web Service Risk Balances

## Ejemplos para el uso del servicio web

### PHP
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'http://you-domain.tdl/wp-json/api/v1/risk-balances/update/',    
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS => array(
  'ws_code' => 'your_ws_code_here',
  'ws_secret_key' => 'your_ws_secret_key_here',
      'users[0][customer_cif]' => 'A01010101',
      'users[0][risk_balance]' => '9621.43',
      'users[1][customer_cif]' => 'B02020202',
      'users[1][risk_balance]' => '6805.2'
  ),  
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```

### JavaSript - jQuery
```
var form = new FormData();
form.append("ws_code", "your_ws_code_here");
form.append("ws_secret_key", "your_ws_secret_key_here");
form.append("users[0][customer_cif]", "A01010101");
form.append("users[0][risk_balance]", "9621.43");
form.append("users[1][customer_cif]", "B02020202");
form.append("users[1][risk_balance]", "6805.2");

var settings = {
  "url": "http://you-domain.tdl/wp-json/api/v1/risk-balances/update/",
  "method": "POST",
  "timeout": 0,
  "processData": false,
  "mimeType": "multipart/form-data",
  "contentType": false,
  "data": form
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

### Python
```
import requests

url = "http://you-domain.tdl/wp-json/api/v1/risk-balances/update/"

payload={'ws_code': 'your_ws_code_here',
'ws_secret_key': 'your_ws_secret_key_here',
'users[0][customer_cif]': 'A01010101',
'users[0][risk_balance]': '9621.43',
'users[1][customer_cif]': 'B02020202',
'users[1][risk_balance]': '6805.2'}
files=[

]
headers = {
  'Cookie': 'PHPSESSID=trtdom7p70v14d51iidtumn604'
}

response = requests.request("POST", url, headers=headers, data=payload, files=files)

print(response.text)
```

### Curl
```
curl --location --request POST 'http://you-domain.tdl/wp-json/api/v1/risk-balances/update/' \
--form 'ws_code="your_ws_code_here"' \
--form 'ws_secret_key="your_ws_secret_key_here"' \
--form 'users[0][customer_cif]="A01010101"' \
--form 'users[0][risk_balance]="9621.43"' \
--form 'users[1][customer_cif]="B02020202"' \
--form 'users[1][risk_balance]="6805.2"'
```