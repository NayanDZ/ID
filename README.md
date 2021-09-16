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

## How to exploit


## Prevention
- Deserialization of user input should be avoided unless absolutely necessary.
- Implement a digital signature to check the integrity of the data. but remember that any checks must take place before beginning the deserialization process
- Avoid using generic deserialization features. create your own class-specific serialization methods so that you can at least control which fields are exposed.

## Refrence
- https://portswigger.net/web-security/deserialization
