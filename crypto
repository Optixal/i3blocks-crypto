#!/usr/bin/env python3
# coding=utf-8

import json
import requests
import os
import sys

PRICE_CHANGE_PERIOD = '1h' # Available: '1h', '24h', '7d'
PRICE_CHANGE_URGENT_PERCENT = 10

API_URL = 'https://api.coinmarketcap.com/v1/ticker/{}/' # CoinMarketCap API
coin = os.environ.get('BLOCK_INSTANCE', 'bitcoin')
r = requests.get(API_URL.format(coin))
data = json.loads(r.text)[0]

base = 'usd'
if len(sys.argv) > 1: base = sys.argv[1]

if base.lower() == 'btc':
    price = float(data['price_btc'])
    precision = 8
else:
    price = float(data['price_usd'])
    if price > 100: precision = 0
    elif price > 0.1: precision = 2
    else: precision = 6

percentChange = float(data['percent_change_' + PRICE_CHANGE_PERIOD])
percentChangeFormat = '<span color="{}">{}{:.2f}%</span>'
if percentChange > 0: percentChangeInfo = percentChangeFormat.format('#3BB92D', '', percentChange)
elif percentChange == 0: percentChangeInfo = percentChangeFormat.format('#CCCCCC', '', percentChange)
else: percentChangeInfo = percentChangeFormat.format('#F7555E', '', percentChange)

print(('{} {:.' + str(precision) + 'f} {}').format(data['symbol'], price, percentChangeInfo)) # Full Text
print(('{} {:.' + str(precision) + 'f}').format(data['symbol'], price)) # Short Text

if percentChange > PRICE_CHANGE_URGENT_PERCENT:
    exit(33)
