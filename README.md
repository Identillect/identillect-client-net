# Delivery Trust .NET Client

## Installation

If you use [NuGet](http://www.nuget.org/), simply run the following:
`PM> Install-Package deliverytrust-api-client`

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
    
