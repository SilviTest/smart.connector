![SMART by altares](https://smart.altares.nl/assets/smart-altares-black.png "Smart by altares")
# Client connector
This is the development guide for connecting client applications to SMART. Here you can find usecases and examples on how to connect to the SMART services.

SMART will give you the deepest insights within the West-European market. SMART prospecting will help you get a better understanding of your market and customer portfolio. Optimize your targeting and ROI of marketing campaigns according to the deepest commercial data available.  
For more information about the product itself you can visit our [website](https://www.altares.nl/en/platforms/smart/) and read more about Smart.

---
## API
With SMART we deliver a range of webservices using REST endpoints, which can be communicated with using HTTP commands and JSON data. The REST endpoints are documented with [Swagger](https://swagger.io/) tooling and can be read at our [API documentation website](https://slim-api.olbico.nl).  
To be able to interact with our services you need to have a valid account and login. Please contact our customer services if help is needed to set up proper accounts.

## Authentication
If you are in possession of a valid login you can access our API endpoint by authenticating using [OAuth 2.0](https://oauth.net/2/) headers and protocols. In short, this means you have to retrieve a valid token by authenticating with our application, and then you can interact with the API's supplying the retrieved token in the 'Authorization' HTTP header.
Our login authority follows the [OpenID](https://openid.net/what-is-openid/) standard. All needed information for authenticating can be loaded from our configuration page at: https://security.smart.altares.group/.well-known/openid-configuration.