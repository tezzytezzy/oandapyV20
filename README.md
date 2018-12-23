# oandapyV20
How to use oandapyV20 API: MAKE IT SIMPLE

I find quite a few sample codes are scattered aound on Internet to retrieve prices via oandapyV20 API.

Here below I compile them neatly for the followings:

1. [Get Current Price](https://github.com/tezzytezzy/oandapyV20/new/master?readme=1#get-current-price)
2. [Get Streaming Price](https://github.com/tezzytezzy/oandapyV20/new/master?readme=1#get-streaming-price)
3. [Get Historical Price](https://github.com/tezzytezzy/oandapyV20/new/master?readme=1#get-historical-price)

First of all, download the package:
```bash
    $ pip install oandapyV20
```

Import some packages next:
```python
    import json
    import oandapyV20
    from oandapyV20 import API
    from oandapyV20.exceptions import V20Error
    import configparser
    import oandapyV20.endpoints.pricing as pricing
```

Create **config_v20.txt** file in the YAML syntax under **./Config**. It includes your account_id and api_key, both of which are given after sign-up with OANDA:
```
    [oanda]
    account_id = 981-223-99988798-111
    api_key = fods348u3091feoiwhfrh2irhfpe-jf382u32hrrehfkhewffkf
```

```python
    #Better NOT to embed API Key in the code, rather read that off a text file (config_v20.txt)
    config = configparser.ConfigParser()
    config.read('./Config/config_v20.txt')
    account_id = config['oanda']['account_id']
    api_key = config['oanda']['api_key']

    print(account_id + "\n" + api_key)
```

Get Current Price
-----------------
```python
    params ={  
              "instruments": "EUR_USD,EUR_JPY"  
            } 

    r = pricing.PricingInfo(accountID=account_id, params=params)
    rv = api.request(r)
    print(r.response)
```

Get Streaming Price
-------------------
```python
    instruments = "USD_JPY"
    s = pricing.PricingStream(accountID=account_id, params={"instruments":instruments})
    try:
        n = 0
        maxrecs = 5
        for R in api.request(s):
            print(json.dumps(R, indent=2))
            n += 1
            if n > maxrecs:
                #s.terminate("maxrecs received: {}".format(MAXREC))
                s.terminate("maxrecs received")

    except V20Error as e:
        print("Error: {}".format(e))
```

Get Historical Price
--------------------
```python
    import oandapyV20.endpoints.instruments as instruments

    #must be in the ISO8601 format
    from_date = "2018-11-20T00:00:00.000000Z"
    to_date = "2018-11-30T00:00:00.000000Z"

    params ={
               "granularity": "D",
               "from": from_date,
               "to": to_date
            }
    r = instruments.InstrumentsCandles(instrument="USD_JPY",
                                       params=params)
    api.request(r)
    print(r.response)
```
