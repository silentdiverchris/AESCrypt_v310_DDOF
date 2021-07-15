Version 310 DDOF - Don't Delete Output File : A very minor change to stop it deleting the output file if it finds the input file to have been altered, whether through random corruption or intentional tampering. 

Original source copied from source 'AES Crypt source code (Windows)' on the AES Crypt web site at https://www.aescrypt.com/download/ - thanks to Packetizer at https://www.packetizer.com/, the authors of this great open source software.

If vanilla v310 finds the input to be inconsistent with the checksum it will report an error and delete the output file, which may well not be what the user wants.

This version extends the error message to give ample warning that the output file may not be what was originally encrypyted but not delete the output, allowing the user to decide whether they want to look at or trust whatever it's managed to dig out of the encrypted source.

The error message for this eventuality has been changed;
From: Message has been altered and should not be trusted.
To: The source has been altered either by random data corruption or intentional tampering, the resulting file may not be identical to what was originally encrypted.

The original Setup project is no longer supported by VS2019, it has been removed so this will not have a friendly installer. The executables can be manually deployed or just overwritten on an install of the original v310.

You'll need to install optional feature 'C++ MFC for latest v142 build tools (x86 & x64)' to build it locally, use Visual Studio Installer and 'Modify' your existing install.

Three changed files to deploy listed below, these are the only files in my C:\Program Files\AESCrypt, optionally rename the existing files or folder to keep the unaltered version, you may need to reboot to pick up the updated dll. These three files are included here under the 'binaries' folder but the safest option is always to build it locally.

AESCrypt.dll
aescrypt.exe
AESCrypt32.exe

Tested by encrypting and decrypting half a dozen files from 200k up to 6Gb created with both the existing v310 and with this DDOF version.

========= Original read.me below ============

AESCrypt File Format Description

Items in quotes are a literal string.  Words outside of quotes
are a textual description of the contents.  Fixed-valued octets are
written in hexidecimal form (e.g., 0x01).

The AESCrypt version 2 file format is as follows.
      3 Octets - 'AES'
      1 Octet  - 0x02 (Version)
      1 Octet  - Reserved
     .... Start of repeating extension block section
      2 Octet  - Length in octets (in network byte order) of an extension
                 identifier and contents.  If 0x0000, then no further
                 extensions exist and the next octet is the start of the
                 Initialization Vector (IV).  Following an extension,
                 this length indicator would appear again to indicate
                 presence or absense of another extension and the size of
                 any such extension.
     nn Octets - Extension identifier.  This is either a URI or an
                 identifier defined by the AES developer community and
                 documented on the standard extensions page, either
                 of which is terminated by a single 0x00 octet.  All
                 extension identifiers are case sensitive.
                   Examples of URIs:
                      http://www.aescrypt.com/extensions/creator/
                      urn:oid:1.3.6.1.4.1.17090.55.14
                      urn:uuid:85519EA3-1DA6-45b9-9041-8CD368D8C086
                   Note:
                      A URI was used to allow anybody to define extension
                      types, though we should strive to define a standard
                      set of extensions.
                   Examples of standard extension identifiers:
                      CREATED-DATE
                      CREATED-BY
                 A special extension is defined that has no name, but is
                 merely a "container" for extensions to be added after the
                 AES file is initially created.  Such an extension avoids
                 the need to read and re-write the entire file in order to
                 add a small extension.  Software tools that create AES
                 files should insert a 128-octet "container" extension,
                 placing a 0x00 in the first octet of the extension
                 identifier field.  Developers may then insert extensions
                 into this "container" area and reduce the size of this
                 "container" as necessary.  If larger extensions are added
                 or the "container" area is filled entirely, then reading
                 and re-writing the entire file would be necessary to add
                 additional extensions.
     nn Octets - The contents of the extension
     .... End of repeating extension block section
     16 Octets - Initialization Vector (IV) used for encrypting the
                 IV and symmetric key that is actually used to encrypt
                 the bulk of the plaintext file.
     48 Octets - Encrypted IV and 256-bit AES key used to encrypt the
                 bulk of the file
                 16 octets - Initialization Vector
                 32 octets - encryption key
     32 Octets - HMAC
     nn Octets - Encrypted message (2^64 octets max)
      1 Octet  - File size modulo 16 in least significant bit positions
     32 Octets - HMAC

     Thus, the footprint of the file is at least 136 octets.

The AESCrypt version 1 file format is as follows.
      3 Octets - 'AES'
      1 Octet  - 0x01 (Version)
      1 Octet  - Reserved
     16 Octets - Initialization Vector (IV) used for encrypting the
                 IV and symmetric key that is actually used to encrypt
                 the bulk of the plaintext file.
     48 Octets - Encrypted IV and 256-bit AES key used to encrypt the
                 bulk of the file
                 16 octets - Initialization Vector
                 32 octets - encryption key
     32 Octets - HMAC
     nn Octets - Encrypted message (2^64 octets max)
      1 Octet  - File size modulo 16 in least significant bit positions
     32 Octets - HMAC

     Thus, the footprint of the file is at least 134 octets.

The AESCrypt version 0 file format is as follows.
      3 Octets - 'AES'
      1 Octet  - 0x00 (Version)
      1 Octet  - File size modulo 16 in least significant bit positions
     16 Octets - Initialization Vector (IV)
     nn Octets - Encrypted message (2^64 octets max)
     32 Octets - HMAC

     Thus, the footprint of the file is at least 53 octets.

