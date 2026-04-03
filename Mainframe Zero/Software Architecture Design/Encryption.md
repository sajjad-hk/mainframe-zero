Encryption is process of converting plain text into code or ciphertext, decryption is the reverse, converting ciphertext back to plain text.

Encryption is a perfect example of a non-intrusive security measure. 
Strength of a key depends on its length, its entropy or randomness 

AES is block cipher encryption used across internet. The key range from 128 to 256 bits. AES using 256-bit key is considering unbreakable. 

* Symmetric encryption 
	* Faster
	* 1 key
* Asymmetric encryption
	* More time consuming
	* 2 keys
	* More secure
Symmetric and asymmetric encryption work together in SSL or TLS. SSL or Secure Socket Layer is an older form of TLS a protocol running on the application layer of the internet designed to secure web communication through TLS handshake. To stablish TLS connection the client either can make a request using port 443 which reserved for encrypted `https` traffic or request the server switch to TLS, if the server agrees the handshake is initiated. 
An asymmetric Cipher generates a session specific shared key.

## Password protection with bcrypt
Generally the best practice is to hash and salt passwords a using powerful time-tested algorithm like bcrypt. But still short passwords are susceptible to Brut force attacks. Salting solve both by adding a unique value to the end of the password before hashing. 
