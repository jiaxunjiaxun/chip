# PHP Token Based Authentication JWT (JSON Web Tokens)

## Introduction

Anyone interested in computer security would realize that user authentication is a most controversial topic. Ranging from usability to mechanisms, one can find a broad spectrum of study areas. It is surprising, however, that JSON Web Tokens (JWT) is not as debated a topic as it deserves to be. In this article, we will try to demonstrate the process of PHP token based authentication integration in an API authentication mechanism.

## Versus Sessions

Some time ago, giving out credentials into an application was only way of authenticating yourself and presented following problems

- **Data stored in plain text on the server** is easily accessible to anyone.

- **Multiple file system read/write requests** may cause in slow servers especially if a lot of users are using it.

**JSON Web Token (JWT)** is a JSON-based open standard used for passing claims between two parties in the context of web application environment. These token are specially designed to be very compact and URL safe. Their usability in the context of web browser single sign-on is also remarkable. JWT claims are useful for passing **identities’ verification** between service providers and identity providers. In addition to these, any other claims’ authentication can be provided as required by the business model. The tokens themselves are **encrypted** and authenticated

JSON based token (JWT) were introduced in December 2010 and have following properties.

- Helpful in space constrained environments.
- Data transmission in JavaScript Object Notation format (JSON)
- The data got to be payload of JSON Web Signature (JWS)
- Representation is Base64 URL encoding

**Appearance of a JWT (JSON Web Tokens)**

Following is an example of JWT

> eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE0MTY5MjkxMDksImp0aSI6ImFhN2Y4ZDBhOTVjIiwic2NvcGVzIjpbInJlcG8iLCJwdWJsaWNfcmVwbyJdfQ.XCEwpBGvOLma4TCoh36FU7XhUbcskygS81HE1uHLf0E

Although it might appear as just random characters connected together, a closer observation, on the other hand, would reveal that these are 3 strings which are separated by a dot.The first two strings are Base64 URL encoded JSON strings. Following results would be revealed upon decoding of these.

``` json
{
    "alg": "HS256",
    "typ": "JWT"
}

{
    "iat": 1416929109,
    "jti": "aa7f8d0a95c",
    "scopes": [
        "repo",
        "public_repo"
    ]
}
```

JWS header is in the first string also stating the cryptographic algorithms which were used for generating the signature and payload type. The payload is actually in the second string. Some standard fields are also passed along and also the data which is wished to send in the token. Third-string contains a cryptographic signature for being decoded to binary data.

It is interesting to note about the signature that cryptographic algorithm demand a secret key. This secret key is a string which needs to be known only by the issuer application exclusively and should be guarded. Hence, when an application receives a token the signature can be verified against the contents of the token only by means of said secret key. Failure of signature verification shall be an indicative that the data in token has been tempered and needs to be discarded.

Jwt.io is a nice place where you can get resources for refining the encoding and decoding JWT’s.

## Let’s Play

So, how we can apply this to a **PHP app**? Let’s say we’ve a login module/snippet that uses session and cookies to store data about user’s login state within the web application. Here, I want to clear that JWT’s primary purpose was not to replacing of session/cookies. Anyhow, for this instance, we have two services:

1. The first one that generates a JWT based on the provided username and password
2. The second one will fetch a secured resource if we give it a valid JWT.

After signing up, we can get a protected resource from this application. First, we will install php-jwt . We will use a composer command like composer require firebase/php-jwt.

So, let’s suppose that login form uses AJAX to submit data to JWT service, where all the information will be validated against our database and after validating the valid credentials, we will build our token. For this, make an array first:

``` php
require_once('vendor/autoload.php');

use \Firebase\JWT\JWT;

/**
 * Secret key can be a random string and keep in secret from anyone
 */
define('SECRET_KEY', 'Your-Secret-Key');
/**
 * Algorithm used to sign the token,
 * see https://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms-40#section-3
 * Suppose you have submitted your form data here with username and password
 */
define('ALGORITHM', 'HS512');

$action = $_REQUEST['action']

if ($username && $password && 'login' == $action) {
    // if there is no error below code run
    $statement = $config->prepare("select * from login where name=:name" );
    $statement->execute([ ':name' => $_POST['username'], ]);
    $row = $statement->fetchAll(PDO::FETCH_ASSOC);
    $hashAndSalt = password_hash($password, PASSWORD_BCRYPT);

    if (count($row) > 0 && password_verify($row[0]['password'], $hashAndSalt)) {
        $tokenId = base64_encode(mcrypt_create_iv(32));
        $issuedAt = time();
        $notBefore = $issuedAt + 10; // Adding 10 seconds
        $expire = $notBefore + 7200; // Adding 2 hours
        $serverName = 'http://localhost/php-json/'; // Set your domain name

        /**
         * Create the token as an array
         */
        $data = [
            'iat' => $issuedAt, // Issued at: time when the token was generated
            'jti' => $tokenId, // Json Token Id: an unique identifier for the token
            'iss' => $serverName, // Issuer
            'nbf' => $notBefore, // Not before
            'exp' => $expire, // Expire
            'data' => [
                // Data related to the logged user you can set your required data
                'id' => $row[0]['id'], // id from the users table
                'name' => $row[0]['name'], // name
            ]
        ];

        $secretKey = base64_decode(SECRET_KEY);

        // Here we will transform this array into JWT:
        $jwt = JWT::encode(
            $data, // Data to be encoded in the JWT
            $secretKey, // The signing key
            ALGORITHM);

        $unencodedArray = [ 'jwt' => $jwt, ];
        echo '{"status": "success", "resp": ' . json_encode($unencodedArray) . '}';
    } else {
        echo '{"status": "error", "msg": "Invalid email or passowrd"}';
    }
}
```

**JWT::encode()** is the main function that will take care of everything including

1. Transforming the array to JSON
2. Manufacturing the headers
3. Sign language the payload and encryption the ultimate string

We will make our secret key a long and a binary stirng. Also, we will encode it in our config file and honestly we never will disclose/share with anyone.

### Payload (Claims)

In the context of JWT, a claim is outlined as associate degreenouncement regarding an entity (typically, the user), further as extra meta knowledge regarding the token itself. The claim contains the knowledge we would like to transmit, which the server will use to properly handle authentication. There square measure multiple claims we will provide; these embody registered claim names, public claim names and personal claim names.

### Registered Claims
- iat – timestamp of token supplying.
- jti – It will be unique string that can be used for validating the token, however goes against not having a centralized issuer authority
- iss – A string containing the name or symbol of the institution application. are often a website name and may be accustomed discard tokens from alternative applications.
- nbf – It is the timestamp when we start token should and considering it valid. It should be equal to or greater than iat. In this case , after issuing the token, it will be valid for 10 seconds.
- exp – Timestamp of once the token ought to stop to be valid. Ought to be larger than iat and nbf. During this case, the token can expire sixty seconds once being issued.

These claims are not necessary and needed but they can help us to confirm the token validition (more on this later). Here, the payload is inside the **data** claim, where the **id** and **name** values are being stored. Never ever include any sensitive information in JWT. Because it can be inspected on client side.

Now, we have a token, we can store it using JavaScript or any other mechanism. Here is an example using jQuery:

``` javascript
// Here is our login form that we are using to post username and password information.
$('#frmLogin').submit(function(e) {
    e.preventDefault();

    $.post('login.php?action=login', $('#frmLogin').serialize(), function(data) {
        var data = jQuery.parseJSON(data);
        // You can set returned token in cookie or session and can send with each request to authenticate user
        document.cookie = 'tokanVal=' + data['resp']['jwt'];
        window.location.reload(true);
    });
});

// Get your-token-value where you have set it in session, cookie or somewhere and send with each request that you want to authenticate.
$.post('login.php?action=authenticate&tokVal=your-token-value', function(resp) {
    console.log(resp);
    if (resp.success == true) {
        // If token authenticated successfully then get your data
    } else {
        // If token authentication failed
    }
}, 'json');
```

Let's try and decode the JWT currently. Keep in mind the secret key we used earlier to get the token? it’s a significant a part of the decoding method here:

``` php
/**
 * Decode the jwt using the key from config
 */
if ('authenticate' == $action) {
    try {
        $secretKey = base64_decode(SECRET_KEY);
        $DecodedDataArray = JWT::decode($_REQUEST['tokVal'], $secretKey, [ ALGORITHM, ]);
        echo '{"status": "success", "data": ' . json_encode($DecodedDataArray) . '}';
        die();
    } catch (Exception $e) {
        echo '{"status": "fail", "msg": "Unauthorized"}';
        die();
    }
}
```

## Conclusion

Although PHP Token Based Authentication with JWT is a relatively new concept but they have radicalized the authentication process making it hassle-free and user-friendly and increasing the efficiency, at the same time. We can implement it in our next API key and we can use different algorithms like RS256 or integrate it in a OAUTH2 authentication. PHP token based authentication is also very growing area these days and definitely, should be explored by PHP developers.
