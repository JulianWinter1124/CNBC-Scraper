```python
import requests
from IPython.display import HTML
from bs4 import BeautifulSoup as BS
import json
import torch
import os
from flair.embeddings import TransformerDocumentEmbeddings
from flair.data import Sentence
```


```python
def cnbc_front():
    return requests.get('https://www.cnbc.com/world/')

def cnbc_sublink(href):
    headers = {
        'authority': 'www.cnbc.com',
        'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',
        'accept-language': 'de-DE,de;q=0.9,en-US;q=0.8,en-DE;q=0.7,en;q=0.6',
        'cache-control': 'no-cache',
        # Requests sorts cookies= alphabetically
        # 'cookie': 'region=WORLD; akaas_CNBC_Audience_Segmentation=1654590111~rv=59~id=fefd3956fcb30a20febe5a7e2a0f7d3e; __adblocker=false; adops_master_kvs=; __pnahc=0; __tbc=%7Bkpex%7DWl8ze249Q3ueYN0xB1uP_JvPCZedqHMJOSh6xaWdLzdRQdJWFmBTu7MwPEcJxYem; cX_P=l2x10tsh8s0k469f; __pat=-14400000; __pvi=%7B%22id%22%3A%22v-2022-05-08-10-21-52-987-PbboYaJrP4LSzKAT-0a9c0fed50e53cd6f4f46ff05a06fb87%22%2C%22domain%22%3A%22.cnbc.com%22%2C%22time%22%3A1651998113379%7D; xbc=%7Bkpex%7D_nwyZnBGbPR4tmJZm8z_LHD5Qf1plZ7pkkZz60Ba67tsJ2yadtsadm4-zBZkrWQ54SFo70MgssNDI7MdgkwYyMdadfdPpCV8csXV-9qRvGFMEdv9OLuzgy6JkL4mq1GVISE-z_4ZH4ZJPduFea6P3n5qrxmyrn4Ly3eez7yW0uk2aJxQgFOOjx2HK023k171ZrO0HPWQa_JbHALBG5TyeZraAPrmrURWOwQVgycgEGvRH71ksxgiYJUeF38EFrg7f_r3UMrV26F100MxLfpMfY4WfFWRnhR4Rxt_Sciko7Ruz_7dLRjbzm4eBJyC4VUO2ctqayT0klRDevAgT4f61EjErzYe__8KV8z0klU8pyY9JDo39j_R7Ir3KYoCTu0lcdtSC2rsx7J2sLDgfpLHyBcM6WqFQfSDyZQ4T2IMf6UAyqBJ50y4nQIZp4KPxDXjX030jn5Vwfhf_yzd1u0fFxOLqjf-yJ4KA1UUzGvF_KChX2eyZXzQw8T9DP87p39imWh-5KLf2fxXTEP4z3ACYA; x_debug_lvl=13; eupubconsent-v2=CPYquhVPYquhVAcABBENCOCgAP_AAAAAACiQH3tf_X_fb3_j-_59f_t0eY1P9_7_v20zjheds-8Nyd_X_L8X42M7vB36pq4KuR4Eu3LBAQVlHOHcTUmw6IkVqTPsbk2Mr7NKJ7PEmnMbO2dYGH9_n9XTuZKY79_v__7z_v-v_v__77f_7-3f3_v5_1---2AAAAAAAAAAAAAAA9gAAAFCQAQF5jAAIC8x0AEBeZKACAvMpABAXmAA.f_gAAAAAAAAA; OptanonAlertBoxClosed=2022-05-08T08:25:39.913Z; client_type=html5; client_version=4.5.0; AKA_A2=A; ak_bmsc=0C8C808FCED76A2746AFA3A00FAC816A~000000000000000000000000000000~YAAQZOF7XEIGeZKAAQAAg4Ogow8A8gOAe4J0Ulzo8N5q+g/eaWfsyanK3q9jCh/JHjK3VWZVo0EikqohvdZ85/IBCMk+fihGQAGtwiFQf+B5CcWqpkHEtG9D3i6NQTnk5sMRBimS/+j6ae3sRi7DLByflwKPu/oH52duA4INfPsubfCkCgm1tR2cgBfUYfvDBZqhhXfz61ZSEMbGWi2/fXkycCANuDEvKZMh/kGmtlxFGgM5eGeLXJ66wpUzmFQG0zJpIh94LODtiGjFVX07MIcPHQKniLWa/Mm4R2XPDzj/gw4BaRe38GGDmc2T1BUdQvq54D8QPqa4HfwMFi+loi4r1ak1u14c6flfoGw2Hli3A2vukQe4a6lfHpLAMXbF3/EqY1utl0cP; selectedRegion=WORLD; s_fid=40DDDDA1641014C5-3FF29579DF110937; s_getNewRepeat30=1652012648769-New; s_getNewRepeat90=1652012648770-New; s_vmonthnum=1654034400770%26vn%3D1; s_monthinvisit=true; s_lv=1652012648770; s_lv_s=First%20Visit; linktrk=%5B%5BB%5D%5D; s_cc=true; gig_bootstrap_2_Jx5rsFp18pauXYlKGzHQVpbahcR1iJ30bbyfqZsn69A6vbt3dQ7gYFCESWKMM1sP=_gigya_ver4; savedViewedSymbols=[{"symbolName":"US10Y","companyName":"U.S. 10 Year Treasury","countryCode":"US"}]; OptanonConsent=isIABGlobal=false&datestamp=Sun+May+08+2022+14%3A39%3A42+GMT%2B0200+(Central+European+Summer+Time)&version=6.16.0&hosts=&consentId=31de56c6-e4f4-46d9-94e5-518105029651&interactionCount=4&landingPath=NotLandingPage&groups=1%3A1%2C6%3A1%2C4%3A1%2CSTACK8%3A1%2C7%3A1%2CSTACK16%3A1%2C8%3A1&geolocation=DE%3BNW&AwaitingReconsent=false; sailthru_pageviews=26; mprtcl-v4_13B9270B={\'gs\':{\'ie\':1|\'dt\':\'us1-99f689156b88a84fbe3383020acfea25\'|\'cgid\':\'dad03945-2c52-409e-bead-924a0b85b4fd\'|\'das\':\'0cfec671-d5f2-47b6-a75f-a44e5b4e9f1e\'|\'csm\':\'WyI1OTE2NDI3MDA4NTU4NDIyNDYyIl0=\'|\'ssd\':1652011070582|\'sid\':\'A0095175-5718-4390-A650-1952091BD8E4\'|\'les\':1652013582978}|\'l\':false|\'5916427008558422462\':{\'fst\':1651998113156|\'ua\':\'eyJDTkJDTVBJRCI6IjU5MTY0MjcwMDg1NTg0MjI0NjIifQ==\'}|\'cu\':\'5916427008558422462\'}',
        'dnt': '1',
        'pragma': 'no-cache',
        'referer': 'https://www.cnbc.com/energy/',
        'sec-ch-ua': '" Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"',
        'sec-ch-ua-mobile': '?0',
        'sec-ch-ua-platform': '"Linux"',
        'sec-fetch-dest': 'document',
        'sec-fetch-mode': 'navigate',
        'sec-fetch-site': 'same-origin',
        'sec-fetch-user': '?1',
        'upgrade-insecure-requests': '1',
        'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.41 Safari/537.36',
    }

    return s.get(f'https://www.cnbc.com{href}', headers=headers)#, cookies=cookies


def cnbc_init():
    headers = {
    'authority': 'webql-redesign.cnbcfm.com',
    'accept': '*/*',
    'accept-language': 'de-DE,de;q=0.9,en-US;q=0.8,en-DE;q=0.7,en;q=0.6',
    'cache-control': 'no-cache',
    'content-type': 'application/json',
    'dnt': '1',
    'origin': 'https://www.cnbc.com',
    'partner': 'cnbc01',
    'pragma': 'no-cache',
    'referer': 'https://www.cnbc.com/',
    'sec-ch-ua': '" Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Linux"',
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': 'cors',
    'sec-fetch-site': 'cross-site',
    'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.41 Safari/537.36',
    }

    params = {
        'operationName': 'getAlertsNews',
        'variables': '{}',
        'extensions': '{"persistedQuery":{"version":1,"sha256Hash":"d25c606d75821d8f214bb56a75be95550e0aeba8eb740a1a37b58bcbc104c890"}}',
    }

    return s.get('https://webql-redesign.cnbcfm.com/graphql', params=params, headers=headers)

def cnbc(topic_id=20910258, offset=35, page_size=24, include_native=False):
    headers = {
        'authority': 'webql-redesign.cnbcfm.com',
        'accept': '*/*',
        'accept-language': 'de-DE,de;q=0.9,en-US;q=0.8,en-DE;q=0.7,en;q=0.6',
        'cache-control': 'no-cache',
        'content-type': 'application/json',
        'dnt': '1',
        'origin': 'https://www.cnbc.com',
        'partner': 'cnbc01',
        'pragma': 'no-cache',
        'referer': 'https://www.cnbc.com/',
        'sec-ch-ua': '" Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"',
        'sec-ch-ua-mobile': '?0',
        'sec-ch-ua-platform': '"Linux"',
        'sec-fetch-dest': 'empty',
        'sec-fetch-mode': 'cors',
        'sec-fetch-site': 'cross-site',
        'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.41 Safari/537.36',
    }

    params = {
        'operationName': 'getAssetList',
        'variables': f'{{"id":"{topic_id}","offset":{offset},"pageSize":{page_size},"nonFilter":true,"includeNative":{str(include_native).lower()},"include":[]}}',
        'extensions': '{"persistedQuery":{"version":1,"sha256Hash":"202314eccd1b9d973b75758156268b4fe5181b60cc2e9d3b0b76c1c617604b60"}}',
    }

    return requests.get('https://webql-redesign.cnbcfm.com/graphql', params=params, headers=headers)

def cnbc_article(url):

    headers = {
        'authority': 'www.cnbc.com',
        'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',
        'accept-language': 'de-DE,de;q=0.9,en-US;q=0.8,en-DE;q=0.7,en;q=0.6',
        'cache-control': 'no-cache',
        # Requests sorts cookies= alphabetically
        # 'cookie': 'region=WORLD; AKA_A2=A; akaas_CNBC_Audience_Segmentation=1654590111~rv=59~id=fefd3956fcb30a20febe5a7e2a0f7d3e; __adblocker=false; adops_master_kvs=; __pnahc=0; __tbc=%7Bkpex%7DWl8ze249Q3ueYN0xB1uP_JvPCZedqHMJOSh6xaWdLzdRQdJWFmBTu7MwPEcJxYem; cX_P=l2x10tsh8s0k469f; __pat=-14400000; __pvi=%7B%22id%22%3A%22v-2022-05-08-10-21-52-987-PbboYaJrP4LSzKAT-0a9c0fed50e53cd6f4f46ff05a06fb87%22%2C%22domain%22%3A%22.cnbc.com%22%2C%22time%22%3A1651998113379%7D; xbc=%7Bkpex%7D_nwyZnBGbPR4tmJZm8z_LHD5Qf1plZ7pkkZz60Ba67tsJ2yadtsadm4-zBZkrWQ54SFo70MgssNDI7MdgkwYyMdadfdPpCV8csXV-9qRvGFMEdv9OLuzgy6JkL4mq1GVISE-z_4ZH4ZJPduFea6P3n5qrxmyrn4Ly3eez7yW0uk2aJxQgFOOjx2HK023k171ZrO0HPWQa_JbHALBG5TyeZraAPrmrURWOwQVgycgEGvRH71ksxgiYJUeF38EFrg7f_r3UMrV26F100MxLfpMfY4WfFWRnhR4Rxt_Sciko7Ruz_7dLRjbzm4eBJyC4VUO2ctqayT0klRDevAgT4f61EjErzYe__8KV8z0klU8pyY9JDo39j_R7Ir3KYoCTu0lcdtSC2rsx7J2sLDgfpLHyBcM6WqFQfSDyZQ4T2IMf6UAyqBJ50y4nQIZp4KPxDXjX030jn5Vwfhf_yzd1u0fFxOLqjf-yJ4KA1UUzGvF_KChX2eyZXzQw8T9DP87p39imWh-5KLf2fxXTEP4z3ACYA; x_debug_lvl=13; eupubconsent-v2=CPYquhVPYquhVAcABBENCOCgAP_AAAAAACiQH3tf_X_fb3_j-_59f_t0eY1P9_7_v20zjheds-8Nyd_X_L8X42M7vB36pq4KuR4Eu3LBAQVlHOHcTUmw6IkVqTPsbk2Mr7NKJ7PEmnMbO2dYGH9_n9XTuZKY79_v__7z_v-v_v__77f_7-3f3_v5_1---2AAAAAAAAAAAAAAA9gAAAFCQAQF5jAAIC8x0AEBeZKACAvMpABAXmAA.f_gAAAAAAAAA; OptanonAlertBoxClosed=2022-05-08T08:25:39.913Z; client_type=html5; client_version=4.5.0; OptanonConsent=isIABGlobal=false&datestamp=Sun+May+08+2022+10%3A54%3A19+GMT%2B0200+(Central+European+Summer+Time)&version=6.16.0&hosts=&consentId=31de56c6-e4f4-46d9-94e5-518105029651&interactionCount=4&landingPath=NotLandingPage&groups=1%3A1%2C6%3A1%2C4%3A1%2CSTACK8%3A1%2C7%3A1%2CSTACK16%3A1%2C8%3A1&geolocation=DE%3BNW&AwaitingReconsent=false; sailthru_pageviews=13; mprtcl-v4_13B9270B={\'gs\':{\'ie\':1|\'dt\':\'us1-99f689156b88a84fbe3383020acfea25\'|\'cgid\':\'dad03945-2c52-409e-bead-924a0b85b4fd\'|\'das\':\'0cfec671-d5f2-47b6-a75f-a44e5b4e9f1e\'|\'csm\':\'WyI1OTE2NDI3MDA4NTU4NDIyNDYyIl0=\'|\'sid\':\'83838B44-8DC9-4DBF-A659-3F1D191B2CAB\'|\'les\':1652000060600|\'ssd\':1651998112933}|\'l\':false|\'5916427008558422462\':{\'fst\':1651998113156|\'ua\':\'eyJDTkJDTVBJRCI6IjU5MTY0MjcwMDg1NTg0MjI0NjIifQ==\'}|\'cu\':\'5916427008558422462\'}',
        'dnt': '1',
        'pragma': 'no-cache',
        'referer': 'https://www.cnbc.com/economy/',
        'sec-ch-ua': '" Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"',
        'sec-ch-ua-mobile': '?0',
        'sec-ch-ua-platform': '"Linux"',
        'sec-fetch-dest': 'document',
        'sec-fetch-mode': 'navigate',
        'sec-fetch-site': 'same-origin',
        'sec-fetch-user': '?1',
        'upgrade-insecure-requests': '1',
        'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.41 Safari/537.36',
    }

    return requests.get(url, cookies={}, headers=headers)


def make_string_pathsafe(st):
    keepcharacters = (' ','.','_', '-')
    return "".join(c for c in st if c.isalnum() or c in keepcharacters).rstrip()

# def cnbc(offset=35, page_size=24, include_native=False):

#     headers = {
#         'authority': 'webql-redesign.cnbcfm.com',
#         'accept': '*/*',
#         'accept-language': 'de-DE,de;q=0.9,en-US;q=0.8,en-DE;q=0.7,en;q=0.6',
#         'cache-control': 'no-cache',
#         'content-type': 'application/json',
#         'dnt': '1',
#         'origin': 'https://www.cnbc.com',
#         'partner': 'cnbc01',
#         'pragma': 'no-cache',
#         'referer': 'https://www.cnbc.com/',
#         'sec-ch-ua': '" Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"',
#         'sec-ch-ua-mobile': '?0',
#         'sec-ch-ua-platform': '"Linux"',
#         'sec-fetch-dest': 'empty',
#         'sec-fetch-mode': 'cors',
#         'sec-fetch-site': 'cross-site',
#         'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.41 Safari/537.36',
#     }

#     params = {
#         'operationName': 'getAssetList',
#         'variables': {"id":"20910258","offset":offset,"pageSize":page_size,"nonFilter":True,"includeNative":include_native,"include":[]},
#         'extensions': '{"persistedQuery":{"version":1,"sha256Hash":"d25c606d75821d8f214bb56a75be95550e0aeba8eb740a1a37b58bcbc104c890"}}',
#     }

#     return s.get('https://webql-redesign.cnbcfm.com/graphql', params=params, headers=headers)

```


```python
s = requests.Session()
```


```python
res = cnbc_front()
res
```




    <Response [200]>




```python
res = cnbc_init()
res
```




    <Response [200]>




```python
soup = BS(res.text)
sublinks = soup.find('div', {'class': 'nav-menu-navLinks'}).find_all('a', {'class':'nav-menu-subLink'})
print(sublinks)
for a in sublinks:
    res2 = cnbc_sublink(a['href'])
    if res2.status_code != 200:
        print(1, res2)
        break
    soup2 = BS(res2.text)
    content_id = soup2.find('meta', {'property':"pageNodeId"})['content']
    res2 = cnbc(content_id, i, page_size=30)
    if res2.status_code != 200:        
        print(2, res2)
        break
    display(JSON(res2.json()))
    break
    

```

    [<a class="nav-menu-subLink" href="/pre-markets/" tabindex="-1">Pre-Markets</a>, <a class="nav-menu-subLink" href="/us-markets/" tabindex="-1">U.S. Markets</a>, <a class="nav-menu-subLink" href="/markets-europe/" tabindex="-1">Europe Markets</a>, <a class="nav-menu-subLink" href="/china-markets/" tabindex="-1">China Markets</a>, <a class="nav-menu-subLink" href="/markets-asia-pacific/" tabindex="-1">Asia Markets</a>, <a class="nav-menu-subLink" href="/world-markets/" tabindex="-1">World Markets</a>, <a class="nav-menu-subLink" href="/currencies/" tabindex="-1">Currencies</a>, <a class="nav-menu-subLink" href="/cryptocurrency/" tabindex="-1">Cryptocurrency</a>, <a class="nav-menu-subLink" href="/futures-and-commodities/" tabindex="-1">Futures &amp; Commodities</a>, <a class="nav-menu-subLink" href="/bonds/" tabindex="-1">Bonds</a>, <a class="nav-menu-subLink" href="/funds-and-etfs/" tabindex="-1">Funds &amp; ETFs</a>, <a class="nav-menu-subLink" href="/economy/" tabindex="-1">Economy</a>, <a class="nav-menu-subLink" href="/finance/" tabindex="-1">Finance</a>, <a class="nav-menu-subLink" href="/health-and-science/" tabindex="-1">Health &amp; Science</a>, <a class="nav-menu-subLink" href="/media/" tabindex="-1">Media</a>, <a class="nav-menu-subLink" href="/real-estate/" tabindex="-1">Real Estate</a>, <a class="nav-menu-subLink" href="/energy/" tabindex="-1">Energy</a>, <a class="nav-menu-subLink" href="/climate/" tabindex="-1">Climate</a>, <a class="nav-menu-subLink" href="/transportation/" tabindex="-1">Transportation</a>, <a class="nav-menu-subLink" href="/industrials/" tabindex="-1">Industrials</a>, <a class="nav-menu-subLink" href="/retail/" tabindex="-1">Retail</a>, <a class="nav-menu-subLink" href="/wealth/" tabindex="-1">Wealth</a>, <a class="nav-menu-subLink" href="/life/" tabindex="-1">Life</a>, <a class="nav-menu-subLink" href="/small-business/" tabindex="-1">Small Business</a>, <a class="nav-menu-subLink" href="/invest-in-you/" tabindex="-1">Invest In You</a>, <a class="nav-menu-subLink" href="/personal-finance/" tabindex="-1">Personal Finance</a>, <a class="nav-menu-subLink" href="/fintech/" tabindex="-1">Fintech</a>, <a class="nav-menu-subLink" href="/financial-advisors/" tabindex="-1">Financial Advisors</a>, <a class="nav-menu-subLink" href="/options-action/" tabindex="-1">Options Action</a>, <a class="nav-menu-subLink" href="/etf-street/" tabindex="-1">ETF Street</a>, <a class="nav-menu-subLink" href="https://buffett.cnbc.com" tabindex="-1" target="_blank">Buffett Archive</a>, <a class="nav-menu-subLink" href="/earnings/" tabindex="-1">Earnings</a>, <a class="nav-menu-subLink" href="/trader-talk/" tabindex="-1">Trader Talk</a>, <a class="nav-menu-subLink" href="/cybersecurity/" tabindex="-1">Cybersecurity</a>, <a class="nav-menu-subLink" href="/enterprise/" tabindex="-1">Enterprise</a>, <a class="nav-menu-subLink" href="/internet/" tabindex="-1">Internet</a>, <a class="nav-menu-subLink" href="/media/" tabindex="-1">Media</a>, <a class="nav-menu-subLink" href="/mobile/" tabindex="-1">Mobile</a>, <a class="nav-menu-subLink" href="/social-media/" tabindex="-1">Social Media</a>, <a class="nav-menu-subLink" href="/cnbc-disruptors/" tabindex="-1">CNBC Disruptor 50</a>, <a class="nav-menu-subLink" href="/tech-guide/" tabindex="-1">Tech Guide</a>, <a class="nav-menu-subLink" href="/white-house/" tabindex="-1">White House</a>, <a class="nav-menu-subLink" href="/policy/" tabindex="-1">Policy</a>, <a class="nav-menu-subLink" href="/defense/" tabindex="-1">Defense</a>, <a class="nav-menu-subLink" href="/congress/" tabindex="-1">Congress</a>, <a class="nav-menu-subLink" href="/equity-opportunity/" tabindex="-1">Equity and Opportunity</a>, <a class="nav-menu-subLink" href="/europe-politics/" tabindex="-1">Europe Politics</a>, <a class="nav-menu-subLink" href="/china-politics/" tabindex="-1">China Politics</a>, <a class="nav-menu-subLink" href="/asia-politics/" tabindex="-1">Asia Politics</a>, <a class="nav-menu-subLink" href="/world-politics/" tabindex="-1">World Politics</a>, <a class="nav-menu-subLink" href="/live-audio/" tabindex="-1">Live Audio</a>, <a class="nav-menu-subLink" href="/latest-video/" tabindex="-1">Latest Video</a>, <a class="nav-menu-subLink" href="/top-video/" tabindex="-1">Top Video</a>, <a class="nav-menu-subLink" href="/video-ceo-interviews/" tabindex="-1">CEO Interviews</a>, <a class="nav-menu-subLink" href="/europe-television/" tabindex="-1">Europe TV</a>, <a class="nav-menu-subLink" href="/asia-business-day/" tabindex="-1">Asia TV</a>, <a class="nav-menu-subLink" href="/podcast/" tabindex="-1">CNBC Podcasts</a>, <a class="nav-menu-subLink" href="/digital-original/" tabindex="-1">Digital Originals</a>, <a class="nav-menu-subLink" href="/investingclub/newsletter/" tabindex="-1">Newsletter</a>, <a class="nav-menu-subLink" href="/investingclub/morning-meeting/" tabindex="-1">Morning Meeting</a>, <a class="nav-menu-subLink" href="/investingclub/cramer-trade-alert/" tabindex="-1">Trade Alerts</a>, <a class="nav-menu-subLink" href="/investingclub/charitable-trust/" tabindex="-1">Trust Portfolio</a>, <a class="nav-menu-subLink" href="/pro/news/" tabindex="-1">Pro News</a>, <a class="nav-menu-subLink" href="/pro/" tabindex="-1">Pro Live</a>, <a class="nav-menu-subLink" href="#" tabindex="-1">Subscribe</a>, <a class="nav-menu-subLink" href="#" tabindex="-1">Sign In</a>]



    <IPython.core.display.JSON object>



```python
from IPython.display import JSON
```


```python
JSON(res.json())
```




    <IPython.core.display.JSON object>




```python
embedding = TransformerDocumentEmbeddings('bert-base-uncased')
front_page = cnbc_front()
front_soup = BS(front_page.text)
sublinks = front_soup.find('div', {'class': 'nav-menu-navLinks'}).find_all('a', {'class':'nav-menu-subLink'})
for a in sublinks:
    if 'buffet' in a['href']:
        continue
    category_page = cnbc_sublink(a['href'])
    print("Category", a['href'])
    if category_page.status_code != 200:
        print(1, category_page)
        continue
    
    
    category_soup = BS(category_page.text)
    content_id = category_soup.find('meta', {'property':"pageNodeId"})
    if not content_id:
        break
    content_id = content_id['content']
    print(content_id)
    i = 0
    retry = 0
    while i < 1000 and retry < 2:
        print(i)
        res = cnbc(content_id, i, page_size=30)
        if res.status_code != 200:
            print(res)
            break
        articles_json = res.json()
        try:
            if 'assets' in articles_json['data']['assetList']:
                articles = articles_json['data']['assetList']['assets']
            else:
                articles = []
        except KeyError:
            print('key not found')
            display(JSON(articles_json))
            retry += 1
            continue
                      
        if len(articles) == 0:
            break
        with open(f"/media/julian/data/data/news/cnbc_{make_string_pathsafe(a['href'])}_{i}_{i+len(articles)}.json", 'w') as f:
                json.dump(articles_json, f)
        i += len(articles)
        retry = 0
        for ai, article in enumerate(articles):
            if "news" in article['type']:
                url = article['url']
                if os.path.exists(f"/media/julian/data/data/news/{article['datePublished']}_{make_string_pathsafe(article['title'])}.txt"):
                    continue
                res = cnbc_article(url)
                if res.status_code != 200:
                    continue
                soup = BS(res.text)
                main_content = soup.find(id='MainContentContainer') #.get_text()
                bad = main_content.find('div', {'class':'WatchLiveRightRail-inline WatchLiveRightRail-container'})
                if bad:
                    bad.decompose()
                bad = main_content.find('div', {'class':'SidebarArticle-sidebar PageBuilder-sidebar'})
                if bad:
                    bad.decompose()
                bad = main_content.find('div', {'class':'InlineVideo-videoEmbed'})
                if bad:
                    bad.decompose()
                text = main_content.get_text(' ')
                with open(f"/media/julian/data/data/news/{article['datePublished']}_{make_string_pathsafe(article['title'])}.txt", 'w') as f:
                    f.write(text)
```

    Category /pre-markets/
    17689937
    0
    12
    Category /us-markets/
    100003242
    0
    30
    60
    90
    120
    150
    180
    210
    Category /markets-europe/
    10000528
    0
    30
    60
    90
    120
    150
    180
    210
    Category /china-markets/
    105200300
    0
    30
    60
    90
    120
    key not found



    <IPython.core.display.JSON object>


    120
    key not found



    <IPython.core.display.JSON object>


    Category /markets-asia-pacific/
    10000527
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /world-markets/
    100003241
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /currencies/
    15839178
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /cryptocurrency/
    104924378
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /futures-and-commodities/
    15839171
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /bonds/
    15839203
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /funds-and-etfs/
    10000946
    0
    30
    60
    90
    120
    150
    180
    200
    Category /economy/
    20910258
    0
    30
    60
    90
    120
    150
    180
    200
    Category /finance/
    10000664
    0
    30
    60
    90
    120
    150
    180
    200
    Category /health-and-science/
    10000108
    0
    30
    60
    90
    120
    150
    180
    200
    Category /media/
    10000110
    0
    30
    60
    90
    120
    150
    171
    Category /real-estate/
    10000115
    0
    30
    60
    90
    120
    150
    180
    200
    Category /energy/
    19836768
    0
    30
    60
    90
    120
    150
    180
    200
    Category /climate/
    106915556
    0
    30
    60
    90
    120
    150
    180
    200
    Category /transportation/
    10000142
    0
    30
    60
    90
    120
    150
    180
    200
    Category /industrials/
    10000098
    0
    30
    60
    90
    120
    150
    180
    200
    Category /retail/
    10000116
    0
    30
    60
    90
    120
    150
    180
    200
    Category /wealth/
    10001054
    0
    30
    60
    90
    120
    150
    180
    200
    Category /life/
    10001150
    0
    30
    60
    90
    120
    150
    180
    200
    key not found



    <IPython.core.display.JSON object>


    200
    key not found



    <IPython.core.display.JSON object>


    Category /small-business/
    44877279
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /invest-in-you/
    105684944
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /personal-finance/
    21324812
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /fintech/
    104199263
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /financial-advisors/
    100646281
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /options-action/
    28282083
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /etf-street/
    104338521
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /earnings/
    15839135
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /trader-talk/
    20398120
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /cybersecurity/
    100807029
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /enterprise/
    101004656
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /internet/
    10000026
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /media/
    10000110
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /mobile/
    10000066
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /social-media/
    100397778
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /cnbc-disruptors/
    102602634
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /tech-guide/
    104368876
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /white-house/
    10001079
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /policy/
    105056552
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /defense/
    10001059
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /congress/
    10000887
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /equity-opportunity/
    106658224
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /europe-politics/
    105229267
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /china-politics/
    105229289
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /asia-politics/
    105229295
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /world-politics/
    105229305
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /live-audio/
    105574584
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /latest-video/
    100004038
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /top-video/
    15839263
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /video-ceo-interviews/
    100004032
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /europe-television/
    15838668
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /asia-business-day/
    15838789
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /podcast/
    105025127
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /digital-original/
    104266428
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /investingclub/newsletter/
    106972187
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /investingclub/morning-meeting/
    107004558
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /investingclub/cramer-trade-alert/
    106983828
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /investingclub/charitable-trust/
    107001319
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /pro/news/
    102138233
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category /pro/
    103620081
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category #
    100727362
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>


    Category #
    100727362
    0
    key not found



    <IPython.core.display.JSON object>


    0
    key not found



    <IPython.core.display.JSON object>



```python
import glob

for file in glob.glob('/media/julian/data/data/news/*.txt'):
    filename = os.path.splitext(file)[0]+'.pt'
    if not os.path.exists(filename):
        with open(file, 'r') as f:
            text = f.readlines()
            sentence = Sentence(text)
            embedding.embed(sentence)
            doc_emb = sentence.embedding
            filename = os.path.splitext(f.name)[0]+'.pt'
            torch.save(doc_emb, f"/media/julian/data/data/news/{name}")
```


```python
res = cnbc(15839171, 0, page_size=30)
display(JSON(articles_json))
```




    <Response [400]>




```python

```




    '<html><title>400 Bad Request.</title><body><h1>400 Bad Request.</h1><hr /><p>aiCache</p></body></html>'




```python
from flair.data import Sentence
from flair.models import SequenceTagger

# make a sentence
sentence = Sentence(main_content.get_text())

# load the NER tagger
tagger = SequenceTagger.load('ner')

# run NER over sentence
tagger.predict(sentence)
```

    2022-05-08 12:34:54,898 loading file /home/julian/.flair/models/ner-english/4f4cdab26f24cb98b732b389e6cebc646c36f54cfd6e0b7d3b90b25656e4262f.8baa8ae8795f4df80b28e7f7b61d788ecbb057d1dc85aacb316f1bd02837a4a4
    2022-05-08 12:34:56,285 SequenceTagger predicts: Dictionary with 20 tags: <unk>, O, S-ORG, S-MISC, B-PER, E-PER, S-LOC, B-ORG, E-ORG, I-PER, S-PER, B-MISC, I-MISC, E-MISC, I-ORG, B-LOC, E-LOC, I-LOC, <START>, <STOP>



```python
sentence
```




    Sentence: "Federal ReservePowell says ' inflation is much too high ' and the Fed will take ' necessary steps' to addressPublished Mon , Mar 21 202212:30 PM EDTUpdated Mon , Mar 21 20225:27 PM EDTJeff Cox @ jeff.cox.7528 @ JeffCoxCNBCcomWATCH LIVEKey PointsFed Chairman Jerome Powell vowed tough action on inflation , which he said jeopardizes the recovery.Powell said the Fed will continue to hike rates until inflation comes under control , and could get even more aggressive than last week 's increase , which was the first in more than three years.He noted those rate rises could go from the traditional 25 basis point moves to more aggressive 50 basis point increases if necessary.Federal Reserve Board Chairman Jerome Powell testifies during a hearing before Senate Banking , Housing and Urban Affairs Committee on Capitol Hill November 30 , 2021 in Washington , DC.Alex Wong | Getty Images News | Getty ImagesFederal Reserve Chairman Jerome Powell on Monday vowed tough action on inflation , which he said jeopardizes an otherwise strong economic recovery. " The labor market is very strong , and inflation is much too high ," the central bank leader said in prepared remarks for the National Association for Business Economics.The speech comes less than a week after the Fed raised interest rates for the first time in more than three years in an attempt to battle inflation that is running at its highest level in 40 years.Reiterating a position the Federal Open Market Committee made Wednesday in its post-meeting statement , Powell said interest rate hikes would continue until inflation is under control . He said the increases could be even higher if necessary than the quarter-percentage point move approved at the meeting. " We will take the necessary steps to ensure a return to price stability ," he said . " In particular , if we conclude that it is appropriate to move more aggressively by raising the federal funds rate by more than 25 basis points at a meeting or meetings , we will do so . And if we determine that we need to tighten beyond common measures of neutral and into a more restrictive stance , we will do that as well. " A basis point is equal to 0.01 % . FOMC officials indicated that 25 basis point increases are likely at each of their remaining six meetings this year . However , markets are pricing in about a 50-50 chance the next hike , at the May meeting , could be 50 basis points.Stocks slipped to their lows of the session after Powell 's remarks while Treasury yields rose. ' Widely underestimated ' inflationThe sudden policy tightening comes with inflation as measured by the consumer price index running at 7.9 % on a 12-month basis . A gauge that the Fed prefers still has prices up 5.2 % , well above the central bank 's 2 % target.As he has before , Powell ascribed much of the pressures coming from Covid pandemic-specific factors , in particular escalated demand for goods over services that supply could not meet . He conceded that Fed officials and many economists " widely underestimated " how long those pressures would last.While those aggravating factors have persisted , the Fed and Congress provided more than $ 10 trillion in fiscal and monetary stimulus since the pandemic 's start . Powell said he continues to believe that inflation will drift back to the Fed 's target , but it 's time for the historically easy policies to end. " It continues to seem likely that hoped-for supply-side healing will come over time as the world ultimately settles into some new normal , but the timing and scope of that relief are highly uncertain ," said Powell , whose official title now is chairman pro tempore as he awaits Senate confirmation for a second term . " In the meantime , as we set policy , we will be looking to actual progress on these issues and not assuming significant near-term supply-side relief. " Powell also addressed the Russian invasion of Ukraine , saying it is adding to supply chain and inflation pressures . Under normal circumstances , the Fed generally would look through those types of events and not alter policy . However , with the outcome unclear , he said policymakers have to be wary of the situation. " In normal times , when employment and inflation are close to our objectives , monetary policy would look through a brief burst of inflation associated with commodity price shocks ," he said . " However , the risk is rising that an extended period of high inflation could push longer-term expectations uncomfortably higher , which underscores the need for the Committee to move expeditiously as I have described. " Powell had indicated last week that the FOMC also is prepared to begin running off some of the nearly $ 9 trillion in assets on its balance sheet . He noted the process could begin as soon as May , but no firm decision has been made.VIDEO1:0501:05Powell says there is an obvious need for the Fed to move expeditiouslyHalftime Report" → ["Federal ReservePowell"/ORG, "Fed"/ORG, "Jerome Powell"/PER, "Fed"/ORG, "Reserve Board"/ORG, "Jerome Powell"/PER, "Senate Banking , Housing and Urban Affairs Committee"/ORG, "Capitol Hill"/LOC, "Washington"/LOC, "DC.Alex Wong"/PER, "Getty Images News"/ORG, "Getty ImagesFederal Reserve"/ORG, "Jerome Powell"/PER, "National Association for Business Economics.The"/ORG, "Fed"/ORG, "Federal Open Market Committee"/ORG, "Powell"/PER, "FOMC"/ORG, "Powell"/PER, "Treasury"/ORG, "Fed"/ORG, "Powell"/PER, "Covid"/MISC, "Fed"/ORG, "Fed"/ORG, "Congress"/ORG, "Powell"/PER, "Fed"/ORG, "Powell"/PER, "Senate"/ORG, "Powell"/PER, "Russian"/MISC, "Ukraine"/LOC, "Fed"/ORG, "Committee"/ORG, "Powell"/PER, "FOMC"/ORG, "Fed"/ORG]




```python
# load the NER tagger
tagger = SequenceTagger.load('pos')

# run NER over sentence
tagger.predict(sentence)
```


    Downloading:   0%|          | 0.00/249M [00:00<?, ?B/s]


    2022-05-08 12:30:06,105 loading file /home/julian/.flair/models/pos-english/a9a73f6cd878edce8a0fa518db76f441f1cc49c2525b2b4557af278ec2f0659e.121306ea62993d04cd1978398b68396931a39eb47754c8a06a87f325ea70ac63
    2022-05-08 12:30:06,226 SequenceTagger predicts: Dictionary with 53 tags: <unk>, O, UH, ,, VBD, PRP, VB, PRP$, NN, RB, ., DT, JJ, VBP, VBG, IN, CD, NNS, NNP, WRB, VBZ, WDT, CC, TO, MD, VBN, WP, :, RP, EX, JJR, FW, XX, HYPH, POS, RBR, JJS, PDT, NNPS, RBS, AFX, WP$, -LRB-, -RRB-, ``, '', LS, $, SYM, ADD

