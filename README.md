## UCI CINEMAS API (Private EndPoints)

Hello guys, today i have scraping all private api from uci cinemas application, excluded web api (there are is basically public, you can find it by "inspect element" lmao), and then i write this docs, for every devs want works with it, if you want you are free to use it but credit me for this instead ;) 

# Get Auth Token (OAuth 2.0)

Basically following normal oAuth flux...

```curl
curl --location 'https://login.ucicinemas.it/token' \
--header 'Authorization: Basic YXBwLXVjaWNpbmVtYXM6bDhnb3YxNDc5eHdzYzBzNGNjZzAwY2NjNHN3ZzRjdw==' \
--header 'User-Agent: Dalvik/2.1.0 (Linux; U; Android 12; SM-G973F Build/SP1A.210812.016)' \
--header 'Connection: Keep-Alive' \
--header 'Accept: application/json' \
--header 'Accept-Encoding: gzip' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'code={FILL_YOUR_CODE_HERE}' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'redirect_uri=ucicinemas://login' \
--data-urlencode 'code_verifier=jO_pNRq7xg64FLcGoi1-qrNkRRZL2zBV5QL4YyKoAQ9PGBBvkeV-2JR-0gSKpNSoLoNRWvY3nF124ZnjaPOmnA'
```

# Get UserData EndPoint 

```curl
curl --location 'https://mobile.webtic.it/mobile/WSC_Iphone.asmx/_UciLoginToken' \
--header 'Authorization: Basic bW9iX3VjaTpVemlVemkhMTgyMA==' \
--header 'Connection: Keep-Alive' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Cookie: ASP.NET_SessionId=s4tm5qvff1r5uxxjpkkae1w4' \
--data-urlencode 'accessToken={PUT_HERE_YOUR_JWT}'
```

Require JWT

### Description 

This endpoint return CSV file contentent your current user datas like email, number etc..


# Get All Cinemas EndPoint

```curl
curl --location 'https://www.ucicinemas.it/rest/v3/cinemas/' \
--header 'authorization: Bearer SkAkzoScIbhb3uNcGdk8UL0XMIbvs5' \
--header 'referer: https://www.ucicinemas.it/'
```

### Description 

This endpoint return JSON file contentent all cinemas avable information, like source_id (used as ```idcinema```), name, longitude, latitude etc..

# Get All Films EndPoint

```curl
curl --location 'https://mobile.webtic.it/mobile/WSC_Iphone.asmx/_getAllFilmsFromGroupOrCinemaAndDate?idgruppo=0&idcinema=5363' \
--header 'Authorization: Basic bW9iX3VjaTpVemlVemkhMTgyMA==' \
--header 'Connection: Keep-Alive' \
--header 'Cookie: ASP.NET_SessionId=xkxnldvwv5a40jwa2h2pb2z1'
````

### Parameter (IN URL)

- idcinema ```this is cinema id you can get (in my case "Bergamo Orio") ```

### Description 

This endpoint return CSV file contentent all information of films in spcific cinema, can also watch movies that haven't been released yet etc.., BUT in this specific request you can read the hash of title called ```HashTitoloFilm``` (this paramter is fundamental to do stuff with the show you choose)


# Get Film Information EndPoint

```curl
curl --location 'https://mobile.webtic.it/mobile/WSC_Iphone.asmx/_getInfoFilmFromHash?hash_titolo=551ea56b61ec65cdfa2b20e4511be254' \
--header 'Authorization: Basic bW9iX3VjaTpVemlVemkhMTgyMA==' \
--header 'Connection: Keep-Alive' \
--header 'Cookie: ASP.NET_SessionId=senoc5kferbrfrp1hhsl4cta'
```

### Parameter (IN URL)

- hash_titolo ```this is string hash of current film title (in my case "VENOM: THE LAST DANCE") ```

### Description 

This endpoint return CSV file contentent all information of film current film you pass in url with HASH name, like director, categories, actors, trailer etc...


# Get Single Film Programmation Info EndPoint

```curl
curl --location 'https://mobile.webtic.it/mobile/WSC_Iphone.asmx/_getProgrammazioneFromFilm?id_cinema=5363&hash_titolo=551ea56b61ec65cdfa2b20e4511be254' \
--header 'Authorization: Basic bW9iX3VjaTpVemlVemkhMTgyMA==' \
--header 'Connection: Keep-Alive' \
--header 'Cookie: ASP.NET_SessionId=44vf5tt5rrj5fitgx5zhvc50'
```

### Parameter (IN URL)

- id_cinema ```this is cinema id you can get (in my case "Bergamo Orio") ```
- hash_titolo ```hash of current film title (in my case "VENOM: THE LAST DANCE") ```

### Description 

This endpoint are useful because return CSV file contentent infromation about rate code, room of film, hour of show beginning, cinecard film id like code and some usefull stuff like price etc.. (but with this you can see event code and if you can pushcase or not)


# Add Reservation

```curl
curl --location 'https://mobile.webtic.it/mobile/WSC_Iphone.asmx/setBloccoPostiIPH' \
--header 'Authorization: Basic bW9iX3VjaTpVemlVemkhMTgyMA==' \
--header 'Connection: Keep-Alive' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'idcinema=5363' \
--data-urlencode 'iCodEvento=241709' \
--data-urlencode 'sSessionId=d19b7b60-aa70-4781-9e33-e61456df7426' \
--data-urlencode 'listaPosti=F/10;F/9'
```

### Parameter (HEADERS)

- idcinema ```this is cinema id you can get (in my case "Bergamo Orio") ```
- iCodEvento ```event id you can retrive in "Get Single Film Programmation Info EndPoint" ```
- listaPosti ```seat you choosed```

### Description 

This endpoint return CSV with state if state is 0 this process has success, and seat is locked for now on, if return -1 probably is reserved from another user, unvable for this show or full...

# Get Current Order ID 

```curl
curl --location 'https://checkout.webtic.it/iphone/WSC_Payment_SSB.asmx/getIdOrdine?idcinema=5363' \
--header 'Connection: Keep-Alive' \
--header 'Cookie: ASP.NET_SessionId=pldl22qdojuvlbfyopdslk5s' 
```
### Parameter (IN URL)

- id_cinema ```this is cinema id you can get (in my case "Bergamo Orio") ```

then session (in this case it is setting up for venom last dance)

### Description 

This endpoint return CSV with your order id created with reservation (basically is what people working for uci print on ticket in bar code eheh)


# Get All Reservation For Film EndPoint

```curl
curl --location 'https://mobile.webtic.it/mobile/WSC_Iphone.asmx/_getMappaPerformanceIPHST?idcinema=5363&icodevento=241709&reservation_type=2' \
--header 'Authorization: Basic bW9iX3VjaTpVemlVemkhMTgyMA==' \
--header 'Connection: Keep-Alive' \
--header 'Cookie: ASP.NET_SessionId=bd1mmvfcnqqlkrklns2lohdb'
```

### Parameter (HEADERS)

- idcinema ```this is cinema id you can get (in my case "Bergamo Orio") ```
- icodevento ```event id you can retrive in "Get Single Film Programmation Info EndPoint" ```
- reservation_type ```all info```


# Get UCI PayPal Token

```curl
curl --location 'https://checkout.webtic.it/iphone/WSC_Payment_SSB.asmx/_getPaypalToken?idcinema=5363' \
--header 'Connection: Keep-Alive' \
--header 'Cookie: ASP.NET_SessionId=pldl22qdojuvlbfyopdslk5s'
```

### Description 

This endpoint return CSV with paypal jwt token of uci cinemas store
