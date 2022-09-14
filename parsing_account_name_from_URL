import re
import json
import requests
import time
from random import randint 
from bs4 import BeautifulSoup
import pandas as pd


def instaparse(item: str):
    if item == None or str(item) == 'nan':
        return str(item)
    parts = re.match('.*instagram(?:.com){0,1}(?:\/){0,1}(?:\%2F){0,1}(p\/){0,1}([\@\w\d\.\_\-]+)(?:[\/\?\&\%]){0,1}.*', 
                     item, re.IGNORECASE)
    if parts == None:
        return item.replace('@', '') if not(re.match('.*tiktok.com\/(\@).*', item)) else item
    if parts[1] == None:
        return parts[2]
    return item
    '''Блок ниже сейчас выключен - нужен VPN и switcher IP-адресов VPN'''
    if parts[1] == '/p':
        r = requests.get("https://instagram.com/"+parts[1:2]+"/?__a=1") 
        # Проверить: скачать JSON по любой ссылке в data.json и распарсить:
        # username = json.loads(open('~/Downloads/data.json', 'r').read())['items'][0]['user']['username']
        username = json.loads(r.text)['items'][0]['user']['username']
        return username
    return item


def tiktokparse(item: str):
    if item == None or str(item) == 'nan':
        return str(item)
    parts = re.match('.*tiktok.com\/(\@){0,1}([\w\d\_\.\-]+)(?:[\/\?\&\%]){0,1}.*', item , re.IGNORECASE) 
    if parts == None:
        return item.replace('@', '')
    if parts[1] == '@':
        return parts[2]
    # return item
    '''Блок ниже сейчас включен - работает без switcher'а IP-адресов, но не обрабатывает кривые ссылки '''
    if re.match('https\:\/\/v(?:m|t).*', item, re.IGNORECASE)
        or re.match('.*tiktok\.com.*', item, re.IGNORECASE) 
        or re.match('.*vk\.com\/away\.php.*', item, re.IGNORECASE) :
        headers = {
            'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36',
        }
        time.sleep(randint(1, 10)/10)
        try:
            data = requests.get(item, headers=headers)
            return re.match('.*tiktok.com\/(?:\@){0,1}([\@\w\d\.\_\-]+)(?:[\/\?\&\%]){0,1}.*', 
                        data.url, re.IGNORECASE).group(1)
        except:
            print(item)
            return item
    
    return item


def likeeparse(item: str):
    if item == None or str(item) == 'nan':
        return str(item)
    parts = re.match('.*likee(?:\.){0,1}video(?:\.ru){0,1}\/(\@|p\/|k\/){0,1}([\w\d\_\.\-]+)(?:[\/\?\&\%]){0,1}.*', 
                     item , re.IGNORECASE) 
    if parts == None:
        return item.replace('@', '')
    if parts[1] == '@':
        return parts[2]
    # return item
    '''Блок ниже сейчас включен - работает без switcher'а IP-адресов '''
    if parts[1] == 'p/':
        headers = {
            'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36',
        }
        time.sleep(randint(1, 10)/10)
        try:
            data = requests.get(item, headers=headers)
            return re.match('.*tiktok.com\/(?:\@){0,1}([\@\w\d\.\_\-]+)(?:[\/\?\&\%]){0,1}.*', 
                        data.url, re.IGNORECASE).group(1)
        except:
            print(item)
            return item
    if parts[1] == 'k/':
        try:
            soup = BeautifulSoup(requests.get(url, headers=headers).text, "html.parser")
            get_profile_html = soup.select_one("script:-soup-contains('userinfo')").string
            json_response = json.loads(
                re.search(r"window\.userinfo = (.*);", get_profile_html).group(1)
            )['yyuid']
        except:
            print(item)
            return item
    
    return item

 
# clippers['Instagram_nickname'] = clippers['Instagram'].apply(instaparse)
# clippers['TikTok_nickname'] = clippers['TikTok'].apply(tiktokparse)
# clippers['Likee_nickname'] = clippers['Likee'].apply(likeeparse)
