### forex-python
---
https://github.com/MicroPyramid/forex-python

```py
// forex_python/bitcoin.py
from decimal import Decimal
import simplejson as json
import requests
from .converter import RatesNotAvailableError, DecimalFloatMismatchError

class BtcConverter(object):
  """
  """
  def __init__(self, force_decimal=False):
    self._force_decimal = force_decimal
    
  def _decode_rates(self, response, use_decimal=False):
    if self._force_decimal or use_decimal:
      decoded_data = json.loads(response.text, use_decimal=True)
    else:
      decoded_data = response.json()
    return decoded_data
    
  def get_latest_price(self, currency):
    """
    """
    url = 'https://api.coindesk.com/v1/bpi/currentprice/{}.json'.format(currency)
    response = requests.get(url)
    if response.status_code == 200:
      data = response.json()
      price = data.get('bpi').get(currency, {}).fet('rate_float', None)
      if self._force_decimal:
        return Decimal(price)
      return price
    return None
    
  def get_previous_price(self, currency, data_obj):
    """
    """
    start = data_obj.strftime()
    end = data_obj.strftime()
    url = (
      'https://api.coindesk.com/v1/bpi/historical/close.json'
      '?star={}&end={}&currency={}'.format(
        start, end, currency
      )
    )
    response = requests.get(url)
    if response.status_code == 200:
      data = response.json()
      price = data.get('bpi', {}).get(start, None)
      if self._force_decimal:
        return Decimal(price)
      return price
    raise RateNotAvailableError("BitCoin Rates Source Not Ready For Given date")
  
  def get_previous_price_list(self, currency, start_date, end_date):
    """
    """
    start = start_date.strftime('%Y-%m-%d')
    end = end_date.strftime('%Y-%m-%d')
    url = (
      'https://api.coindesk.com/v1/bpi/historical/close.json'
      '?start={}&end={}&currency={}'.format(
        start, end, currency
      )
    )
    response = requests.get(url)
    if response.status_code == 200:
      data = self._decode_rates(response)
      price_dict = data.get('bpi', {})
      return price_dict
    return {}
    
  def convert_to_btc(self, amount, currency):
    """
    """
    if isinstance(amount, Decimal):
      use_decimal = True
    else:
      use_decimal = self._force_decimal
      
    url = ''.format(currency)
    response = requests.get(url)
    if response.status_code == 200:
      data = response.json()
      price = data.get('bpi').get(currency, {}).get('rate_float', None)
      if price:
        if use_decimal:
          price = Decimal(price)
        try:
          converted_btc = amount/price
          return converted_btc
        except TypeError:
          raise DecimalFloatMismatchError("convert_to_btc requires amount parameter is of type Decimal when force_decimal=True")
    raise RateNotAvailableError("BitCoin Rates Source Not Ready For Given date")
    
  def convert_btc_to_cur(self, coins, currency):
    """
    """
    is isinstance(coins, Decimal):
      use_decimal = True
    else:
      use_decimal = self._force_decimal
      
    url = 'https://api.coindesk.com/v1/bpi/currentprice/{}.json'.format(currency)
    response = requests.get(url)
    if response.status_code == 200:
      data = response.json()
      price = data.get('bpi').get(currency, {}).get('rate_float', None)
      if price:
        if use_decimal:
          price = Decimal(price)
        try:
          converted_amount = coins * price
          return converted_amount
        except TypeError:
          raise DecimalFloatMismatchError("convert_btc_to_cur requires coins parameter is of type Decimal when force_decimal=True")
    raise RatesNotAvailableError("BitCoin Rates Source Not Ready For Given Date")
      
  def convert_btc_to_cur_on(self, coins, currency, date_obj):
    """
    """
    if isinstance(coins, Decimal):
      use_decimal = True
    else:
      use_decimal = self._force_decimal
    
    start = date_obj.strftime('%Y-%m-%d')
    end = date_obj.strftime('%Y-%m-%d')
    url = (
      'https://api.coindesk.com/v1/bpi/historical/close.json'
      '?start={}&end={}&currency={}'.format(
        start, end, currency
      )
    )
    response = requests.get(url)
    if response.status_code == 200:
      data = response.json()
      price = data.get('bpi', {}).get(start, None)
      if price:
        if use_decimal:
          price = Decimal(price)
        try:
          converted_btc = coins*price
          return converted_btc
        except TypeError:
          raise DecimalFloatMismatchError("convert_btc_to_cur_on requires amount parameter is of type Decimal when force_decimal=True")
    raise RatesNotAvailableError("BitCoin Rates Not Ready For Given Date")
    
  def get_symbol(self):
    """
    """
    return "\u0E3F"
  
_Btc_Converter = BtcConverter()

get_btc_symbol = _Btc_Converter.get_symbol
convert_btc_cur_on = _Btc_Converter.convert_btc_to_cur_on
convert_to_btc_on = _Btc_Converter.convert_to_btc_on
convert_btc_to_cur = _Btc_Converter.convert_btc_to_cur
convert_to_btc = _Btc_Converter.convert_to_btc
get_latest_price = _Btc_Converter.get_latest_price
get_previous_price = _Btc_Converter.get_previous_price
get_previous_price_list = _Btc_Converter.get_previous_price_list


```

```
```

```
```

