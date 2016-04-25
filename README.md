# Delivery Trust .NET Client
Requires at least .NET Framework 4.0

## Installation

If you use [NuGet](http://www.nuget.org/), simply run the following:
`PM> Install-Package deliverytrust-api-client`

## Configuration
Specify the api endpoint and optionally the encryption and auth token. Note: You can choose to store your Encryption key and Auth token in your app/web.config to avoid needing to retrieve them each time a new DeliveryTrustClient is instantiated

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <appSettings>
        <add key="DeliveryTrust:ApiBaseUrl" value="https://api.identillect.com/api/v1/"/>
        <add key="DeliveryTrust:EncryptionKey"/>
        <add key="DeliveryTrust:AuthToken"/>
      </appSettings>
    </configuration>
 

## Client Documentation
The Delivery Trust API endpoints require Username/Password authentication. Please sign up at https://identillect.com/product
### Example usage

Begin by creating an instance of the client:

    var client = new DeliveryTrustClient();
    
Provision the `client` with authentication credentials:

    client.Login("emailaddress@domain.com", "password")
    
Provision the `client` with an encryption key:

    client.GetEncryptionKey();
    
 Finally send a secure email:
 
    client.SendSecureEmail(new DeliveryTrustEmail
    {
        Recipients = new List<DeliveryTrustRecipient>
        {
            new DeliveryTrustRecipient{Address = "nick.l@identillect.com"}
        },
        Subject = "Loan Documents",
        Body = "Your loan documents have been processed"
    });
    
### Additional Notes Regarding Authentication
Depending on the use case of your application additional authentication configuration may be required.

In order to skip subsequent login steps (keep your user logged in) you must store and provide the refresh token provided by the `client` whenever you create a new instance of the `client` (such as on each startup of the application).

    client.AuthTokenChanged += (sender, eventArgs) =>
    {
        //persist auth token in a shared location
        ConfigurationManager.AppSettings["DeliveryTrust:AuthToken"] = eventArgs.AuthToken;
    };
    
Next time you create an instance of the DeliveryTrustClient you can provide the `AuthToken` that you persisted and skip the `client.Login(...);` step.

When specifying the AuthToken, passing null for the first two arguments will prompt the client to retrieve them from your app or web.config

    var authToken = ConfigurationManager.AppSettings["DeliveryTrust:AuthToken"];
    var client = new DeliveryTrustClient(null, null, authToken);
