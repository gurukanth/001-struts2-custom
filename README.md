001-struts2-custom
==================

1. Async tag in servlet 3.0 configuration
2. Apache-cxf transports (Standalone http, servlet http transport)
3. Apache-CXF embedding into spring framework.
4. RSA Encryption of request messages
5. XMLBeans
6. I ahve multiple http jaxrs server endpoints and would like to use a local transport for communication between 2 endpoints. Possible?
7. Removing WS-Addressing headers from fault response
8. Adding additional info to soap response and representing the same in WSDL for client purpose.
9. 


****************************************************************************************************
Program for 3DES Encryption
****************************************************************************************************
import java.security.MessageDigest;
import java.util.Arrays;

import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;



class ZiggyTest2{


        public static void main(String[] args) throws Exception{  
            String text = "I am sunil";

            byte[] codedtext = new ZiggyTest2().encrypt(text);
            String decodedtext = new ZiggyTest2().decrypt(codedtext);

            System.out.println(codedtext); // this is a byte array, you'll just see a reference to an array
            System.out.println(decodedtext); // This correctly shows "kyle boon"
        }

        public byte[] encrypt(String message) throws Exception {
            MessageDigest md = MessageDigest.getInstance("md5");
            byte[] digestOfPassword = md.digest("ABCDEABCDE"
                            .getBytes("utf-8"));
            byte[] keyBytes = Arrays.copyOf(digestOfPassword, 24);
            for (int j = 0, k = 16; j < 8;) {
                    keyBytes[k++] = keyBytes[j++];
            }

            SecretKey key = new SecretKeySpec(keyBytes, "DESede");
            IvParameterSpec iv = new IvParameterSpec(new byte[8]);
            Cipher cipher = Cipher.getInstance("DESede/CBC/PKCS5Padding");
            cipher.init(Cipher.ENCRYPT_MODE, key, iv);

            byte[] plainTextBytes = message.getBytes("utf-8");
            byte[] cipherText = cipher.doFinal(plainTextBytes);
            // String encodedCipherText = new sun.misc.BASE64Encoder()
            // .encode(cipherText);

            return cipherText;
        }

        public String decrypt(byte[] message) throws Exception {
            MessageDigest md = MessageDigest.getInstance("md5");
            byte[] digestOfPassword = md.digest("ABCDEABCDE"
                            .getBytes("utf-8"));
            byte[] keyBytes = Arrays.copyOf(digestOfPassword, 24);
            for (int j = 0, k = 16; j < 8;) {
                    keyBytes[k++] = keyBytes[j++];
            }

            SecretKey key = new SecretKeySpec(keyBytes, "DESede");
            IvParameterSpec iv = new IvParameterSpec(new byte[8]);
            Cipher decipher = Cipher.getInstance("DESede/CBC/PKCS5Padding");
            decipher.init(Cipher.DECRYPT_MODE, key, iv);

            byte[] plainText = decipher.doFinal(message);

            return new String(plainText, "UTF-8");
        }
    }









import java.io.IOException;  
import java.security.InvalidKeyException;  
import java.security.NoSuchAlgorithmException;  
import java.security.spec.InvalidKeySpecException;  
   
import javax.crypto.Cipher;  
import javax.crypto.SecretKey;  
import javax.crypto.SecretKeyFactory;  
import javax.crypto.spec.DESedeKeySpec;  
   
public class EncryptTripleDes  
{  
   
private static byte[] encrypt(byte[] inpBytes, SecretKey key) throws Exception  
{  
Cipher cipher = Cipher.getInstance("DESede");  
cipher.init(Cipher.ENCRYPT_MODE, key);  
return cipher.doFinal(inpBytes);  
}  
   
private static byte[] decrypt(byte[] inpBytes, SecretKey key) throws Exception  
{  
Cipher cipher = Cipher.getInstance("DESede");  
cipher.init(Cipher.DECRYPT_MODE, key);  
return cipher.doFinal(inpBytes);  
}  
   
/** Read a TripleDES secret key from the specified file */  
private static SecretKey readKey(String keyStr) throws IOException, NoSuchAlgorithmException, InvalidKeyException,  
InvalidKeySpecException  
{  
// Convert Key Str to bytes  
byte[] rawkey = new byte[1024];  
rawkey = keyStr.getBytes("UTF-8");  
   
// Convert the raw bytes to a secret key like this  
DESedeKeySpec keyspec = new DESedeKeySpec(rawkey);  
SecretKeyFactory keyfactory = SecretKeyFactory.getInstance("DESede");  
SecretKey key = keyfactory.generateSecret(keyspec);  
return key;  
}  
  
public byte[] encryptPin(String pin) {  
  
byte[] pinBytes = pin.getBytes(); // pin = "1234567890123456";  
byte[] encBytes= null;  
byte[] decBytes= null;  
byte[] encPin = null;  
  
try  
{  
SecretKey key1 = readKey("6194E4CD35B5E4342036D45258F7304BA37136BE90808912");  
SecretKey key2 = readKey("23BCA1FB74D2C557506223336F5404CF36555ADD90808915");  
SecretKey key3 = readKey("919EE5FB75B3E337406462648F5404BF37155BE908089138");  
encBytes = encrypt(pinBytes, key1);  
decBytes = decrypt(encBytes, key2);  
encPin = encrypt(decBytes, key3);  
}  
catch (Exception e)  
{  
e.printStackTrace();  
}  
  
return encPin;  
  
}  
}
