# Insecure Deserialization

#### What is serialization?
Serialization is the process of converting complex data structures, such as objects and their fields, into a "flatter" format that can be sent and received as a sequential stream of bytes.
- Write complex data to inter-process memory, a file, or a database
- Send complex data over a network

#### What is deserialization?
Deserialization is the process of restoring this byte stream to a fully functional replica of the original object, in the exact state as when it was serialized.

### What is insecure deserialization?
When user-controllable data is deserialized by a website. This potentially enables an attacker to manipulate serialized objects in order to pass harmful data into the application code.

#### Why insecure deserialization vulnerabilities arise?
Because there is a general lack of understanding of how dangerous deserializing user-controllable data can be.

Ideally, User input should never be deserialized at all

Generaly developer think they are safe because they implement some form of additional check on the deserialized data. This approach is often ineffective because it is virtually impossible to implement validation or sanitization to account for every eventuality.

## Impact
Privilege escalation, Arbitrary file access, Remote code execution, Denial-of-service attacks. 

## How to Identify
Look at all data being passed into the website and try to identify anything that looks like serialized data.

### Serialized data format
#### ~ PHP serialization format
PHP uses a mostly human-readable string format, letters representing the data type and numbers representing the length of each entry. 

Example: consider a User object with the attributes:
```
$user->name = "nayan";
$user->isLoggedIn = true;
```
When serialized, this object may look something like this:
```
O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}
```
 This can be interpreted as follows:
    
   - O:4:"User" - An object with the 4-character class name "User"
   - 2 - the object has 2 attributes
   - s:4:"name" - The key of the first attribute is the 4-character string "name"
   - s:6:"carlos" - The value of the first attribute is the 6-character string "carlos"
   - s:10:"isLoggedIn" - The key of the second attribute is the 10-character string "isLoggedIn"
   - b:1 - The value of the second attribute is the boolean value true

The native methods for PHP serialization are `serialize()` and `unserialize()`.

If you have source code access, you should start by looking for `unserialize()` anywhere in the code and investigating further attack.

#### ~ Java serialization format
Java use binary serialization formats which is more difficult to read but you can still identify serialized data if you know how to recognize a few tell-tale signs.

Example: serialized Java objects always begin with the same bytes, which are encoded as `ac ed` in **hexadecimal** and `rO0` in **Base64**

Any class that implements the interface `java.io.Serializable` can be serialized and deserialized

If you have source code access, Any code that uses the `readObject()` method, which is used to read and deserialize data from an InputStream

## Prevention
- Deserialization of user input should be avoided unless absolutely necessary.
- Implement a digital signature to check the integrity of the data. but remember that any checks must take place before beginning the deserialization process
- Avoid using generic deserialization features. create your own class-specific serialization methods so that you can at least control which fields are exposed.

## Refrence
- https://portswigger.net/web-security/deserialization
