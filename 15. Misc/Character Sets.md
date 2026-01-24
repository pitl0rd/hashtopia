Useful [[Concept Application - HashCat|Hashcat reference]], Incremental four character password examples.  

## Arabic  
UTF8 (d880-ddbf)  
```
hashcat -a 3 -m hash.type hash.txt --hex-charset -1 d8d9dadbdcdd -2 808182838485868788898a8b8c8d8e8f909192939495969798999a9b9c9d9e9fa0ala2a3a4a5a6a7a8a9aaabacadaeafb0blb2b3b4b5b6b7b8b9babbbcbdbebf -i ?1?2?1?2?1?2?1?2  
```
## Bengali  
UTF8 (e0a680-e0adbf)  
```
hashcat -a 3 -m hash.type hash.txt --hex-charset -1 e0 -2 a6a7a8a9aaabacad -3 808182838485868788898a8b8c8d8e8f909192939495969798999a9b9c9d9e9fa0ala2a3a4a5a6a7a8a9aaabacadaeafb0blb2b3b4b5b6b7b8b9babbbcbdbebf -i ?1?2?3?1?2?3?1?2?3?1?2?3 
```

## Chinese (Common Characters)  
UTF8 (e4b880-e4bbbf)  
```
hashcat -a 3 -m hash.type hash.txt --hex-charset -1 e4 -2 b8b9babb -3 808182838485868788898a8b8c8d8e8f909192939495969798999a9b9c9d9e9fa0ala2a3a4a5a6a7a8a9aaabacadaeafb0blb2b3b4b5b6b7b8b9babbbcbdbebf -i ?1?2?3?1?2?3?1?2?3?1?2?3  
```

## Japanese (Katakana & Hiragana)  
UTF8 (e38180-e3869f)  
```
hashcat -a 3 -m hash.type hash.txt --hex-charset -1 e3 -2 818283848586 -3 808182838485868788898a8b8c8d8e8f909192939495969798999a9b9c9d9e9fa0ala2a3a4a5a6a7a8a9aaabacadaeafb0blb2b3b4b5b6b7b8b9babbbcbdbebf -i ?1?2?3?1?2?3?1?2?3?1?2?3  
```

## Russian  
UTF8 (d080-d4bf)  
```
hashcat -a 3 -m hash.type hash.txt --hex-charset -1 d0dld2d3d4 -2 808182838485868788898a8b8c8d8e8f909192939495969798999a9b9c9d9e9fa0ala2a3a4a5a6a7a8a9aaabacadaeafb0blb2b3b4b5b6b7b8b9babbbcbdbebf -i ?1?2?1?2?1?2?1?2
```

[[Home]]

#tools 
#advanced 

