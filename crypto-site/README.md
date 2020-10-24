

## Request
``` GET/shop ```

## Response

### Body
```
[
    "Prouct Name" :{
        "id" : "Shoe",
        "name" : "Shoe",
        "usage" : [
            "Some usage"
        ],
        "category" : "some category",
        "manufacturer" : "some name",
        "price" : 200
    }
]
 ```

## Request
```POST/cryptodetails/?username=""```

## Response

### Body
```
{
      "user-name": "name",
      "currency-details" : [
          "brass":"",
          "gold": "",
          "silver": ""
      ],
      "shopped-items" :[]
      
     }
 ```
## Request
```POST/Purchase```

### Body 
``` 
{
    "product-purchased": [],
    "total-amount": "",
    "coins-spent": {
        "brass":,
        "gold":,
        "silver":
    },

}

```
## Request
```GET/Transaction/?name=""```
## Response

### Body
```
{
    
    transaction:[
        {
            send:"",
            amount:"",
            date:"",
            coins-type:[
                "brass":,
                "silver":,
                "gold":
            ],
            time:""

        }
    ]
      
    }
 ```

## Request
```GET/SaveLater/?name=""```

## Response

### Body
```
{
    
    saveLater : [
        "totalItems":,
        "item" : {
            "name":
        }
    ]
    
      
    }
 ```
## Request
```POST/Receive```

### Body
```
{
    
    {
        "amount":,
        "requestedTo":
    }
      
    }
 ```
## Response

``` {
    status:''
}
```

## Request
```POST/Send```

### Body
```
{
    
    {
        "amountSend":,
        "sendTo":,
        "amountLeft": {
            "gold":,
            "brass":,
            "silver":
        }
    }
      
    }
 ```
## Response

``` {
    status:''
}
```

## Request
```POST/```

### Body
```
{
    totalAmountLeft:"",
    coins-left:{
        brass:,
        gold:,
        silver:
    }      
}
 ```
## Response

``` 
{
    status:''
}
```







    



