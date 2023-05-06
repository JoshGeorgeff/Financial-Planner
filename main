import pandas as pd
import datetime 
import requests
from requests.exceptions import ConnectionError
from bs4 import BeautifulSoup

def webContentDiv(webContent, classPath, value):
    webContentDiv = webContent.find_all('div', {'class': classPath})

    try:
        spans = webContentDiv[0].find_all(value)
        texts = [span.get_text() for span in spans]
    except IndexError:
        texts = []
    return texts

def realTimePrice (stockCode):
    url = 'https://finance.yahoo.com/quote/' + stockCode + '?p=' + stockCode + '&.tsrc=fin-srch'

    try: 
        r = requests.get(url)
        webContent = BeautifulSoup(r.text, 'lxml')

        #PULL PRICE AND CHANGE
        texts = webContentDiv(webContent, 'D(ib) Mend(20px)', 'fin-streamer')
        if texts != []:
            price, change = texts[0], texts[1] + ' ' + texts[2]
        else:
            price, change = [], []

        #PULL VOLUME
        texts = webContentDiv(webContent, 'Ta(end) Fw(600) Lh(14px)', 'fin-streamer')
        if texts != []:
             for count, target in enumerate(texts):
                  if target == 'Volume':
                      volume = texts[count+1]
        else:
            volume = []

        #PULL PATTERN
        pattern = webContentDiv(webContent, 'Fz(xs) Mb(4px)', 'span')
        try:
            latestPattern = pattern[0]
        except IndexError:
            latestPattern = []

        #PULL TARGET
        texts = webContentDiv(webContent, 'Ta(end) Fw(600) Lh(14px)', 'td')
        if texts != []:
             for count, target in enumerate(texts):
                 if target == '1y Target Est':
                     oneYearTarget = texts[count+1]
        else:
            oneYearTarget = []

    except ConnectionError:
        price, change = [], []

    return price, change
