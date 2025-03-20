# Mnemonic_CPP
 Tool for Working with Mnemonics THIS TOOL IS INTENDED SOLELY FOR TESTING THE SECURITY OF YOUR OWN MNEMONICS AND/OR IDENTIFYING OTHER VULNERABILITIES TO ENSURE SECURE ACCESS TO CRYPTOCURRENCIES. THE PRODUCT IS PROVIDED "AS IS".
# Donation:  
ETH: 0xDE85c1Ef7874A1D94578f11332e8fa9A6a0eE853  
BTC: bc1q063pks7ex93eka56zyumvutdt6zs9dj959pe9p  
LTC: ltc1qysumht4lxafwvmcu4ruxzuztc2xmj8tz986fmm  
TRX: TTZ3oL16BVNzU46MSJvaoKYAhvtwdTUcnz  
TON: UQC7eqLN_NlVz82YzsjzAo4iOzKjH3t095-CMtqTJ5aoqo0l  
DOT: 1jen89F5v6TbdQsRaKxsCqhNp9qAdeHeZyEUWjgrM8mW6hs  
ADA: addr1qx7qrlcy37xe7j58hjxmyhyqfgu0ppeqxzs43dayjjcgde973lzxgtgqzxdvfq3rswmngapc4sp528dpzfg7huam8v9san7h6z  
DASH: Xms41jaD967XMf2FAfEwGUxYKKhYQuok9T  
SOLANA: BvDQDEgq3kbNT7VQFQRQPjc4Ta5k7d5s7GdcgoKnq3KG  

# ENG  
## 26.05.2024 - version 1.0.0  
✅ Implemented mnemonic functionality in C++  
✅ Most operations are custom-written (except for hmac and pbkdf2, as the OpenSSL library showed better performance, so it was retained)  
✅ Added support for Bloom filters (same type as used by brainflayer)  
✅ Integrated with external generators and files containing mnemonics or entropy  
✅ Fully implemented SLIP-0010 (for secp256k1 and ed25519 curves) - https://slips.readthedocs.io/en/latest/slip-0010/  
✅ Support for the ed25519 curve and Solana cryptocurrency in particular  
✅ Support for invalid mnemonics and custom derivation paths  
✅ Capability to generate mnemonics using a custom 2048-word dictionary  
✅ Ability to save found hashes directly as cryptocurrency addresses  
✅ Built-in fast and cryptographically secure entropy generator, fetching bytes from the processor (verified through testing various generators for quality and speed of generated values)  
✅ Code adapted for 3 systems: Linux, Windows, OSX (MacOS) - added projects for Visual Studio, Xcode, Makefile (for Linux)  

## Launch Argument Description:  
`-h` -- Displays help  
`-b file.blf` -- Bloom filter, can specify multiple files using `-b file.blf -b file2.blf ... -b file99.blf`  
`-d file.txt` -- Load derivation paths from file  
`-deep number` -- Derivation depth (optional if the derivation file contains full paths)  
`-c mode` -- Select hash search modes, multiple can be specified, e.g., `-c cuse` or `-c eS`  
                        u - uncompressed address  
                        c - compressed address  
                        s - segwit address  
                        e - ethereum address  
                        x - x coordinate (20-byte public key)  
                        S - Solana address  
`-custom file.txt` -- Specify your own 2048-word dictionary (one word per line, does not work with the `-lang` parameter)  
`-in` -- Use an external generator or file with mnemonics or entropy  
________________________________________________________________________________  
(These parameters work only with `-in`)  
`-f file.txt` -- Read from file  
`-entropy` -- Indicates that the input stream is entropy (works with `-lang` and `-custom`)  
`-n` -- Additionally adds the step value to the incoming entropy the specified number of times  
`-step` -- Step for `-n`, can be negative, default is 1  
For example, if we specify `-in -f ent.txt -entropy -n 10 -step 16`, and the file ent.txt contains this entropy - `00000000`, the program will check all these entropies:  
`00000000`, `00000010`, `00000020`, `00000030`, `00000040`, `00000050`, `00000060`, `00000070`, `00000080`, `00000090`, `00000100`  
________________________________________________________________________________  

`-lang language` (EN, CT, CS, KO, JA, IT, FR, SP)  | Languages for mnemonics -  
English, ChineseTraditional, ChineseSimplified, Korean, Japanese, Italian, French, Spanish  
`-t number` -- Number of threads  
`-o file.txt` -- Save found results to the specified file  
`-save` -- Save found hashes as addresses  
`-w number of words` -- 3, 6, 9, 12, 15, 18, 21, 24 ... 48 - Number of words for mnemonic generator (default is 12)  
# Download builds   
-  https://github.com/XopMC/Mnemonic_CPP/releases/tag/v.1.0.0  
# Building from Source  
Download builds - https://github.com/XopMC/Mnemonic_CPP/releases/tag/v.1.0.0  
The release on GitHub already includes builds for 3 systems, but if you need to build it yourself, here are the steps:  
## Windows Build  
The files already contain a Visual Studio project - you just need to install the OpenSSL library for a successful build (this can be done using VCPKG)  
## OSX (MacOS) Build  
The files already include a ready-to-use Xcode project, but the OpenSSL library is required.  
To install it, I've added a script `install_openssl_macos.sh` - running it will install homebrew and OpenSSL  
## Linux Build  
The files already include a Makefile, which can be run with the command `make`  
Library installation is required, which can be done with `sudo apt install gcc g++ make openssl libssl-dev`  

# Examples of Running in Different Modes  
1) The program requires a derivation file to run, it will not start without it.  
No hard-coded derivations, only custom ones.  
The file must contain full derivations starting with m/  
Example:  
![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/6ddc74ed-5e09-4749-ad66-794b5c0b9bfb)  

2) Bloom filters containing hashes for search are required.  
To create them, in the GitHub Release menu for each system, I've included these programs:  
`base58-bech32_to_hash160` -- Program for converting different types of addresses into a 20-byte hash, tested on BTC, BCH, Tron, ETH, LTC addresses - other currencies need testing  
`Solana_to_hex` -- Converts Solana addresses into a 20-byte public key  
`hex_to_bloom` -- Creates Bloom filters from the specified file with hashes. The program itself will split the file into the necessary number of Bloom filters, controlled by the `-capacity` parameter - I recommend not setting more than 38000000, which is the number of hashes in one Bloom filter  
![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/44dc06d9-25f8-4085-bfec-fe50d46c5bca)  

3) Example of generating 12 random words:  
`MnemonicC.exe -c cus -d der.txt -b btc.blf -b btc_2.blf -w 12 -t 8`  
where `-c cus` -- search for Compressed, Uncompressed, and Segwit hashes  
`-d der.txt` -- derivation file  
`-b btc.blf -b btc_2.blf` -- Bloom filters  
`-w 12` -- number of words to generate  
`-t 8` -- use 8 processor threads  

![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/f1e27eb5-e5ec-4f1d-b868-61c4395d0e40)  

4) Example of running with an external mnemonic generator:  
`Generator.exe | MnemonicC.exe -c cus -d der.txt -b btc.blf -b btc_2.blf -t 8 -in`  
where `-in` -- indicates working with an external generator  

![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/27a2292d-c813-41a0-83ef-9ff0df05f134)  

5) Example of running from a file, with found results displayed  
`MnemonicC.exe -c cus -d der.txt -b btc.blf -b btc_2.blf -t 8 -in -f file.txt`
where `-f file.txt` -- file with mnemonics  
Adding the `-save` parameter will save hashes as addresses  

![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/9481c73c-48db-4326-959d-9552122a72d2)  

  For Ethereum and Solana respectively  

![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/45d5e30e-97c7-4e18-a93c-37a88e6f92c2)  

6) Entropy mode  
   `MnemonicC.exe -c cusexS -d der.txt -b bloom.blf -in -f test.txt -entropy -n 5 -step 1`  
![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/89c4cb2f-ac5a-490b-86a2-ffa016ddc0f3)  

7) Custom dictionary + entropy mode  
   `MnemonicC.exe -c cusexS -d der.txt -b bloom.blf -in -f test.txt -entropy -n 5 -step 1 -custom rus.txt`  

   ![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/959f954f-6937-4ef6-9a82-3445263ec4d5)  

# For Lucky Owners of AMD Threadripper Processors  
 The program can work with all cores and threads, it's important that the processor is unlocked in the BIOS, i.e., it can use more power than stock settings  
Example on Threadripper PRO 7995WX  
![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/f3bb9a04-93a7-47e3-ab2e-6d4eea6f30f5)  
![image](https://github.com/XopMC/Mnemonic_CPP/assets/89750173/50adc5d6-00cc-493c-adb1-fe180d3d9052)  





