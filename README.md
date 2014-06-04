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
