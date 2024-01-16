# Take-home assignment translated text.

To configure the MailJet API, I downloaded the non-thread-safe version of PHP 8.1.7 and unzipped it into a folder for use with Composer after installing it. into a folder for use with Composer after installing it. 
I started a composer project inside the folder I was going to use for testing with the API and installed it. API tests and 			installed the composer package from Mailjet via their github repository. 
I then registered on the mailjet website, after activating the account, configured the API keys and installed the composer v2.0 	package. I installed the composer package vlucas/phpdotenv to store the API keys in environment variables. 
I created a class called Mail , inside this class are stored the API configuration options and the methods for sending the and the 	   methods to send mails and SMS using this API are stored inside this class.

## Problems encountered while doing this first task:
To test the API connection, I created a test method to send an email without templates using an API sample.
using an example of the API. When I tried to send the emails, the API only returned 401 and 402 errors,
I spent a couple of hours analysing my code and looking for the reason why it wasn't working. The reason
why it wasn't working was because I had forgotten to wrap the message in an array.

### Problem:

```php
function sendSMS(string $SENDER_EMAIL, string $RECIPIENT_EMAIL): Response
{
    $body = [
        "Messages" => [
            "From" => [
                "Email" => "yoquiale@gmail.com",
                "Name" => "Esquivel"
            ],
            "To" => [
                [
                    "Email" => "yoquiale@gmail.com",
                    "Name" => "Alejandro",
                ]
            ],
            "Subject" => "SMS de prueba de Mailjet",
            "TextPart" => "Esto es un SMS de prueba",
            "HTMLPart" => "<h2>Esto es un texto de prueba</h2>",
        ],
        "SandboxMode" => false
    ];

    return $this->mj->post(Resources::$Email, ['body' => $body]);
}
```

### Solution

```php
function sendSMS(string $SENDER_EMAIL, string $RECIPIENT_EMAIL): Response {
  $body = [
    "Messages" => [
      [ //Este array
        "From" => [
          "Email" => "yoquiale@gmail.com",
          "Name" => "Esquivel"
        ],
        "To" => [
          [
            "Email" => "yoquiale@gmail.com",
            "Name" => "Alejandro",
          ]
        ],
        "Subject" => "SMS de prueba de Mailjet",
        "TextPart" => "Esto es un SMS de prueba",
        "HTMLPart" => "<h2>Esto es un texto de prueba</h2>",
      ]
    ],
    "SandboxMode" => false
  ];

  return $this->mj->post(Resources::$Email, ['body' => $body]);
}
```

I created several methods to send a template based on the code available in the API. The mails arrived to the recipient, but the body of the message (the template) did not arrive.

