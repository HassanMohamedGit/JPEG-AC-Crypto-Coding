# JPEG-AC-Crypto-Coding
This Repo contains the implementation of the paper titled: "**Joint Compression-Encryption Technique Based On Arithmetic Coding for JPEG Images**" [1]. The contents are as follows:
- **cjpeg-static-crypt**: The proposed JPEG encoder with the proposed joint-compresson-encryption in [1] with a randomly-generated fixed-compile-time key (as a proof of concept).
- **cjpeg-static-1bit**: The proposed JPEG encoder in [1] (same as ***cjpeg-static-crypt***) with single bit key different from ***cjpeg-static-crypt***.
- **cjpeg-static-orig**: The original unmodified standard JPEG encoder.
- **djpeg-static-crypt**: The JPEG decoder to correctly decode JPEG images encoded with ***cjpeg-static-crypt***.
- **djpeg-static-1bit**": The JPEG decoder that correctly decodes JPEG files encoded with ***cjpeg-static-1bit***.
- **djpeg-static-orig**: The original unmodified standard JPEG decoder.
- **jpegInsert**: This is a special implementation of JPEG encoder that uses a randomly generated sequence file (like ***rand.bin***) or any other sequence files to be inserted instead of the compressed DCT coefficients to generate the images as in Fig.5 in [1]. This program is implemented using the JPEG implementation in [2].
- **rand.bin**: This is a 10MB file generated by the following two Linux commands:

   `$$ dd if=/dev/zero of=Zeros bs=1M count=10`
  
   `$$ openssl enc -aes-256-ctr -in Zero -out rand.bin -K 0000000000000000000000000000000000000000000000000000000000000000 -iv 00000000000000000000000000000000`
- **Joint Compression-Encryption Technique Based On Arithmetic Coding for JPEG Images.pdf**: The final PDF version of [1].
-------------------------------------
### How to use the above programs
**The above programs run on a 64-bit Linux environment (Ubuntu 22.04). Here are some examples for usage:**
- To encrypt an image, you must first convert it to PNM format. The following example uses ***imagemagick*** package:

   `$$ convert sample-image.bmp sample-image.pnm`
- The second step to get an encrypted image is:

   `$$ ./cjpeg-static-crypt -arithmetic -progressive -outfile sample-image-encrypted.jpg -quality 95 sample-image.pnm`
  
you can change the quality factor (the number just after the flag ***-quality***) or use the optional flag ***-progressive*** for progressive scan, but using the flag ***-arithmetic*** is mandatory for encryption.
- For correctly decoding ***sample-image-encrypted.jpg***, use:

   `$$ ./djpeg-static-crypt -outfile sample-image-correctly-decoded.bmp sample-image-encrypted.jpg`
- To display the result of decoding the encrypted image ***sample-image-encrypted.jpg*** with the standard (original) JPEG decoder:

   `$$ ./djpeg-static-orig -outfile sample-image-incorrectly-decoded.bmp sample-image-encrypted.jpg`
- To display the result of decoding the encrypted image ***sample-image-encrypted.jpg*** with a single-bit key difference:

   `$$ ./djpeg-static-1bit -outfile sample-image-1BitErr-decoded.bmp sample-image-encrypted.jpg`
- The programs ***cjpeg-static-crypt***, ***cjpeg-static-orig***, ***cjpeg-static-1bit***, and *** jpegInsert*** have the same usages and same CMD flags.
- The programs ***djpeg-static-crypt***, ***djpeg-static-orig***, and ***djpeg-static-1bit*** have the same usages and same CMD flags.
- To use ***jpegInsert*** for generating images like Fig.5 in [1], a file named ***rand.bin*** must be in the same directory as ***jpegInsert***. and use the same command-line flags like ***cjpeg-static-crypt***, ***cjpeg-static-orig***, and ***cjpeg-static-1bit***.
To generate an encrypted image using AES, use the following steps:

   `$$ openssl enc -aes-256-ctr -in sample-image.bmp -out tmp.bmp -K 0000000000000000000000000000000000000000000000000000000000000000 -iv 00000000000000000000000000000000`  
   `$$ dd if=sample-image.bmp of=tmp.bmp bs=1 count=54 conv=notrunc`
  
   `$$ convert tmp.bmp sample-image-AES.bmp`

- It should be noted that the QM-coder (the Arithmetic coder for JPEG) is not implemented by default in most Windows applications and MATLAB (via ***imread*** command). For Linux, I have tried ***Shotwell*** image-viewer and CMD utilities of ***imagemagick*** package or other official JPEG packages such as ***libjpeg*** or ***libjpeg-turbo***. 

-------------------------------------
### References:
[1] Hassan Y. El-Arsh Mohamed, Amr Abdelaziz, Ahmed Elliethy, Hussein A. Aly and Alaa Eldin Rohiem Shehata, "Joint Compression-Encryption Technique Based On Arithmetic Coding for JPEG Images", Nilesconf2023, ***Not published yet***.

[2] https://github.com/thorfdbg/libjpeg

