# Digi-ID

This is the javascript implementation of the Digi-ID Open Authentication protocol.

Basically, this module builds a message challenge and then verifies the signature.

## Installation

This application works under Node.Js, so need to install it from: `https://nodejs.org/en/`
Then:

    $ npm install digicore-message
    $ npm install should

Optional, to make tests:

    $ npm install mocha

## Usage

    Run in the site root folder.

Start: 
```
node index.js
```

Tests in test/index.js:
```
mocha
```

### Challenge

To build a challenge, you need to initialize a `DigiID` object with a `nonce` and a `callback`.

```
var digiid = new DigiID({nonce:nonce, callback:callback});
```

`nonce` is a random string associated with the user's session id.
`callback` is the url without the scheme where the wallet will post the challenge's signature.

One example of callback could be `www.site.com/callback`. A callback cannot have parameters. By
default the POST call will be done using `https`. If you need to tell the wallet to POST on
`http` then you need to add `unsecure:true`.

Please note: Unsecure should NOT be used in production and is only for testing

```
var digiid = new DigiID({nonce:nonce, callback:callback, unsecure:true});
```

Once the `DigiID` object is initialized, you have access to the following methods :

```
digiid.uri
```

This is the uri which will trigger the wallet when clicked (or scanned as QRcode). For instance :

```
digiid://digiid.digibyteprojects.com/callback?x=987f20277c015ce7
```

If you added `unsecure:true` when initializing the object then uri will be like :

```
digiid://digiid.digibyteprojects.com/callback?x=987f20277c015ce7&u=1
```

To get the uri as QRcode :

```
digiid.qrcode
```

This is actually a URL pointing to the QRcode image.

### Verification

When getting the callback from the wallet, you must initialize a `DigiID` object with the received 
parameters `address`, `uri`, `signature` as well as the excpected `callback` :

```
var digiid = new DigiID(address:address, uri:uri, signature:signature, callback:callback);
```

You can after call the following methods :

```
digiid.nonce
```

Return the `nonce`, which would get you the user's session.

```
digiid.uriValid()
```

Returns `true` if the submitted URI is valid and corresponds to the correct `callback` url.

```
digiid.signatureValid()
```

If returns `true`, then you can authenticate the user's session with `address` (public
DigiByte address used to sign the challenge).


## Integration example

JavaScript application using the Digi-ID lib: https://github.com/digibyte/digiid-js

Live demonstration: http://digiid.digibyteprojects.com
