```py
import urllib.error, urllib.request, urllib.parse
import http.cookiejar

LOGIN_URL = 'http://******.kiwisns.com/postLogin'
#get_url为使用cookie所登陆的网址，该网址必须先登录才可
get_url = 'https://******.kiwisns.com/admin/affiliate/pending'
values = {'email':'******@user.com','password':'123456','auth':'admin'}
postdata = urllib.parse.urlencode(values).encode()
user_agent = r'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) AppleWebKit/537.36' \
             r' (KHTML, like Gecko) Chrome/61.0.3163.79 Safari/537.36'
headers = {'User-Agent':user_agent, 'Connection':'keep-alive'}
#将cookie保存在本地，并命名为cookie.txt
cookie_filename = 'cookie.txt'
cookie_aff = http.cookiejar.MozillaCookieJar(cookie_filename)
handler = urllib.request.HTTPCookieProcessor(cookie_aff)
opener = urllib.request.build_opener(handler)

request = urllib.request.Request(LOGIN_URL, postdata, headers)
try:
    response = opener.open(request)
except urllib.error.URLError as e:
    print(e.reason)

cookie_aff.save(ignore_discard=True, ignore_expires=True)

for item in cookie_aff:
    print('Name ='+ item.name)
    print('Value ='+ item.value)
#使用cookie登陆get_url
get_request = urllib.request.Request(get_url,headers=headers)
get_response = opener.open(get_request)
print(get_response.read().decode())
```

反复使用cookie

```py
import urllib.request, urllib.parse
import http.cookiejar

get_url = 'https://******.kiwisns.com/admin/affiliate/pending'
cookie_filename = 'cookie.txt'
cookie_aff = http.cookiejar.MozillaCookieJar(cookie_filename)
cookie_aff.load(cookie_filename,ignore_discard=True,ignore_expires=True)

handler = urllib.request.HTTPCookieProcessor(cookie_aff)
opener = urllib.request.build_opener(handler)
#使用cookie登陆get_url
get_request = urllib.request.Request(get_url)
get_response = opener.open(get_request)
print(get_response.read().decode())
```



