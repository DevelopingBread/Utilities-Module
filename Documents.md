# Information

__What you'll be learning__
* Installation
  * Utility
  * Http Service
    * Functions
    * Discord Webhooks
    * Reddit API (TDB)

## How to install
TODO: put something here

---
# Utility Module

This is a very basic module that allows you to get services.

__Example__
```lua
local Utility = require( game:WaitForChild('ReplicatedStorage'):WaitForChild('Utility') )
```

__If using require__
```lua
local Utility = require(000000000) -- need to get an id
```

---
# Http Service Module

* Properties
  * HttpEnabled
* Methods
  * GenerateGUID
  * Encode
  * Decode
  * UrlEncode
  * UrlEncodeList
* Http
  * GetAsync
  * PostAsync
  * GetAsyncAPI
  * PostAsyncAPI_UrlEncoded
  * PostAsyncAPI_Json

__How to get service__
```lua
local Http = Utility:GetService('Http')
```

---
## Properties
### HttpEnabled <sub style='color: #1589F0'>void
This is not accessible by any script. <sub><sub>I think...

---
## Methods
### GenerateGUID <sub style='color: #1589F0'>string
A randomly generated UUID __(universally unique identifier)__ string.

This is represented by a 32 hexadecimal (base 16) digits. They are displayed in
5 groups with hyphens separating them. You'll get a total of 36 character, example: `12345-67890-abcde-fghij-klmno`

__Parameters__
<b>wrapped<sub style='color: #1589F0'>boolean</sub></b> Whether the returned string should be wrapped in ___{curly braces}___ | Defualt value: true

__Returns__
<b style='color: #1589F0'>string</b> The randomly generated UUIID. 

__Code Example__
```lua
local Http = Utility:GetService('Http')

local UUID = Http.Methods:GenerateGUID()

print(UUID) --> Expected output: {12345-67890-abcde-fghij-klmno}
```

### Encode <sub style='color: #1589F0'>string
The Encode function will convert your luau table into a `JSON object` or an `array`. Follow these guidelines: (__From ROBLOX's documentation__)
  * Keys of the table must be either strings or numbers. If a table contains both, an array takes priority (string keys are ignored).
  * An empty Lua table `{}` generates an empty JSON array.
  * The value `nil` is never generated.
  * Cyclic table references cause an error.
  * This function allows values such as `inf` and `nan`, which are not valid JSON. This may cause problems if you want to use the outputted JSON elsewhere.

__Parameters__
<b>list<sub style='color: #1589F0'>Variant</sub></b> Your luau table

__Returns__
<b style='color: #1589F0'>string</b> The returned JSON string.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local JSONString = Http.Methods:Encode({ 
  ["Hello World!"] = "How are you today?" 
})

print(JSONString) --> Expected output: { "Hello World!": "How are you today?" }
```

### Decode <sub style='color: #1589F0'>string
The Encode function will convert your `JSON object` or an `array` table into a luau table. Follow these guidelines: (__From ROBLOX's documentation__)
  * Keys of the table are strings or numbers but not both. If a JSON object contains both, string keys are ignored.
  * An empty JSON object generates an empty Lua table `{}`.
  * If the input string is not a valid JSON object, this function will throw an error.


__Parameters__
<b>json_string<sub style='color: #1589F0'>string</sub></b> Your JSON string.

__Returns__
<b style='color: #1589F0'>Variant</b> The returned luau table.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local LuaUTable = Http.Methods:Decode('{ "Hello World!": "How are you today?" }')

print(LuaUTable) --> Expected output: { ["Hello World!"] = "How are you today?" }
```

### UrlEncode <sub style='color: #1589F0'>string
The UrlEncode function will make sure that the reserved characters in the string which was given is properly encoded with `%` with two hexadecimal characters.


__Parameters__
<b>data<sub style='color: #1589F0'>string</sub></b> The string `(URL)` to encode.

__Returns__
<b style='color: #1589F0'>string</b> The properly encoded string.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local Encoded = Http.Methods:UrlEncode('Hello World!')

print(Encoded) --> Expected output: Hello%20World
```

### UrlEncodeList <sub style='color: #1589F0'>string
The UrlEncodeList will convert your list into a properly encoded. Same functionality as above.


__Parameters__
<b>data<sub style='color: #1589F0'>Variant</sub></b> The luau list to be encoded.

__Returns__
<b style='color: #1589F0'>string</b> The properly encoded luau list.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local Encoded = Http.Methods:UrlEncode({
  'Hello',
  ['Lua'] = 'World!'
})

print(Encoded) --> Expected output: 1=Hello&Lua=World%21
```

---
## Http
### Constructers
`new()` Returns a `New Request` object.

__Parameters__
<b>url<sub style='color: #1589F0'>string</sub></b> The URL to be used for making `Get`/`Post` requests.

<b>getasync_data<sub style='color: #1589F0'>Variant</sub></b> Data to be used when making a `GetAsync` request.
* nocache
* headers

<b>postasync_data<sub style='color: #1589F0'>Variant</sub></b> Data to be used when making a `PostAsync` request.
* content_type
* compress
* headers
* data

<b style='color: #478C5C'>These values can be set before being used in a GetAsync/PostAsync request.</b>

### GetAsync <sub style='color: #1589F0'>Variant
The `GetAsync` will send out a HTTP GET request. This does all of the heavy lifting for you.

__Parameters__
<b>nocache<sub style='color: #1589F0'>boolean</sub></b> Weather if you want the request to cache the response. | Defualt value: false
<b>headers<sub style='color: #1589F0'>Variant</sub></b> Used to specify HTTP request headers.

__Returns__
<b style='color: #1589F0'>Variant</b> GET request's response data.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local HttpObject = Http.new('https://google.com')
local data = HttpObject:GetAsync()

print(data.success, data.data) --> Expected output: true, *html data of that webpage*
```

### PostAsync <sub style='color: #1589F0'>Variant
The `PostAsync` will send out a HTTP POST request. Very similar to `GetAsync`

__Parameters__
<b>data<sub style='color: #1589F0'>string</sub></b> Data to be sent to that specific URL. <b style='color: #FEDE00'>Data MUST be encoded before being used. Use Http.Methods:Encode(list) to help you.</b>
<b>content_type<sub style='color: #1589F0'>HttpContentType</sub></b> Modifies the value in the Content-Type header sent with the request. 
<b>compress<sub style='color: #1589F0'>boolean</sub></b> Determines whether the data is compressed (gzipped) when sent. | Defualt value: false
<b>headers<sub style='color: #1589F0'>Variant</sub></b> Used to specify HTTP request headers.

__Returns__
<b style='color: #1589F0'>Variant</b> POST request's request result.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local HttpObject = Http.new('https://google.com') -- I dont got a good example :/
local data = HttpObject:PostAsync(
  Http.Methods:Encode({
    ['Best Module?'] = 'Indeed',
    ['Do you know?'] = 'Hello world is a standard code when making a new code file. This can also help illstrate the language\'s basic syntax.'
  })
)

print(data.success, data.data) --> Expected output: true, *data returned from the website?*
```

### GetAsyncAPI <sub style='color: #1589F0'>Variant
This will send a `GetAsync` request to the URL, and decodes it right after. Very useful when you are getting JSON data from a website.

__Parameters__
<b style='color: #FEDE00'>Inherits the same parameters from GetAsync</b>

__Returns__
<b style='color: #1589F0'>Variant</b> A luau list returned from a API request.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local HttpObject = Http.new('https://google.com')
local data = HttpObject:GetAsync()

print(data) --> Expected output: { ['somevalue'] = 'something' ... }
```

### PostAsyncAPI_UrlEncoded <sub style='color: #1589F0'>Variant
Sends a `PostAsync` request with the data provided. Automatically URL converts the luau list.

__Parameters__
<b>data<sub style='color: #1589F0'>Variant</sub></b> A luau list to be POSTED to that URL. <b style='color: #FEDE00'>Already encodes the data.</b>

__Returns__
<b style='color: #1589F0'>Variant</b> Exact data returned from a `PostAsync` request.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local HttpObject = Http.new('https://google.com')
local data = HttpObject:PostAsyncAPI_UrlEncoded({
  ['mmmmmm'] = 'hot docs ngl'
})

print(data.success, data.data) --> Expected output: true, *data returned from the website?*
```

### PostAsyncAPI_JsonEncoded <sub style='color: #1589F0'>Variant
Sends a `PostAsync` request with the data provided. Automatically JSON converts the luau list.

__Parameters__
<b>data<sub style='color: #1589F0'>Variant</sub></b> A luau list to be POSTED to that URL. <b style='color: #FEDE00'>Already encodes the data.</b>

__Returns__
<b style='color: #1589F0'>Variant</b> Exact data returned from a `PostAsync` request.

__Code Example__
```lua
local Http = Utility:GetService('Http')

local HttpObject = Http.new('https://google.com')
local data = HttpObject:PostAsyncAPI_JsonEncoded({
  ['mmmmmm'] = 'hot docs ngl'
})

print(data.success, data.data) --> Expected output: true, *data returned from the website?*
```

# build info
markdown: kramdown
theme: midnight
