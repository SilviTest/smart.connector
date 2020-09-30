![SMART by altares](https://smart.altares.nl/assets/logo-smart-black.svg "Smart by altares")
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

## Usecase: Exporting your customers to SMART
The first usecase is exporting the customers from your CRM into a list in SMART. We don't need the whole customers database, but only key identifiers to  know which companies are your customer. We support the following types of identifiers:

* DUNS
* KvK number (NL)
* Siret (FR)
* VAT number (BE)

By exporting these identifiers into a list in SMART, you will be able to create selections on the total market and exclude or include your customers. Using the list API you can maintain your customer list in SMART automatically.

### Adding companies to a list via the API

You can add items to a list you have created by using the PUT call to the list endpoint. For the netherlands it would be:


> https://smart.altares.nl/api/v201/dnb-nld/list/{ListId}


Where in the body of the call you specify the values to be added like this: 

```
{
  "Values": [
  	{
      "Item1": "415562391"
    }
  ],
  "Email": true,
  "Type": "DUNS",
  "Name": "",
  "Append": true,
  "Public": false,
  "IncludeFamilyTree": false,
  "IsCustomList": false,
  "ListCategoryId": 0
}
```

## Usecase: Importing data from selections
In SMART you are able to create selections by filtering the total market on a wide collection of traits and specifics for companies. This can generate collections of leads that you might want to import into your own CRM application so that sales team can focus on those potential leads that actually have value.   
You first need to create that selection in SMART. Once it has been created you can trigger its execution from within your CRM application. Afterwards you can retrieve the collection of results either by going to the website and downloading the result in a CSV or Excel format, or by calling the API endpoint and retrieving the data in a JSON format.

### Downloading information from a selection via the API

There are two possible ways to get information from the selection. The first is directly by specifying the selection id. The second is indirectly by downloading information from a previously created result in JSON output.

### Specifying the selection.

This done by the following GET call in case of the Netherlands:


> https://smart.altares.nl/api/v201/dnb-nld/selection/results/{SelectionId}?currentPage=1&pageSize=1


The response will be as the following JSON output: 

```
{
    "pageSize": 1,
    "currentPage": 1,
    "totalPages": 10,
    "totalCount": 10,
    "hasPrevious": false,
    "hasNext": false,
    "companies": [
        {
            "fields": {
                "internalId": "1650672",
                "name": "John Fisher Holding B.V.",
                "kvk": "220637940800",
                "eststatusindicator": "Other",
                "kvksoho": "Other",
                "indecactive": "No",
                "dnbriskindicator": "Other"
            }
        }
    ],
    "selectionName": "PossibleNewCustomers"
}
```

### Specifying the selection.

Another way is first creating a downloadable file. You do this by first opening the Selection, and then choose the export option. After the information is processed the Selection will be available as an Excel file in the head menu Downloads, submenu Selections. As an alternative to a file download, you can obtain the information from such a file directly as JSON output via the following call.

> https://smart.altares.nl/api/v201/dnb-nld/downloads/jsoncontent/DownloadId?currentPage=1&pageSize=1


The response will be as the following JSON output: 

```
{
    "PageSize": 1,
    "CurrentPage": 1,
    "TotalPages": 38,
    "TotalCount": 191,
    "HasPrevious": false,
    "HasNext": true,
    "Companies": [
        {
            "Fields": {
                "DUNS nummer": "414007729"
            }
        }
    ],
    "SelectionName": null
}
```


## Usecases combination, full circular automation
The idea is that you first create a list as a container for companies in the SMART website. You then add your present customers to that list via an API call to SMART. Subsequently in the SMART website you will make a Selection of companies that hold your interest, e.g. companies within a certain region. From that Selection you can exclude the list of present customers, since the Selection will be used to address possible new future customers. This adding an exclusion list to your Selection you also do in the SMART website where you define your Selection. The data of these possible new customers can be downloaded to your CRM system by an API call to SMART.

In your CRM you subsequently process that information, and possibly you add new companies to the list in SMART to exclude them in a new information download. That is you collect the information from the Selection in your CRM system, you mark a company as already addressed, and you add that company to de list of companies used for exclusion in your Selection, because you either do not want to approach them twice, or they have become a new customer.

### Creating an empty list in SMART quickly

For creating an empty list in SMART you choose Selection in the main menu. You select some fake conditions so you'll end up with 0 records. You then save it with the 'Save as list' option in the left pane. Write down the ID from your newly created list, which you can see in the menu List.

### Creating a selection in SMART

Next you create a selection in SMART. The will the companies that will have your interest as a possible new customer. You can exclude your previously created List from the Selection by adding it as an excluded list in the Submenu 'My company'. 

---
Postman collection to support these usecases can be found in this repository.