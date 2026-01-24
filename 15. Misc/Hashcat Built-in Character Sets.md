Useful resource for [[Concept Application - HashCat]]
Hashcat includes a lowercase and uppercase hex charset:  "
Hashcat ?h ?H
`?h 0123456789abcdef  
`?H = 0123456789ABCDEF

German  
`hashcat -a 3 -m <# type> hash.txt -1 charsets/German.hcchr -i ?l?l?l?l 

French  
`hashcat -a 3 -m <# type> hash.txt -1 charsets/French.hcchr -i ?l?l?l?l  

Portuguese  
`hashcat -a 3 -m <# type> hash.txt -1 charsets/Portuguese.hcchr -i ?l?l?l?l  

SUPPORTED LANGUAGE ENCODINGS  
`hashcat -a 3 -m <# type> hash.txt -1 charsets/<language>.hcchr -i ?l?l?l?l

Bulgarian, Castilian, Catalan, English, French, German, Greek, Greek Polytonic,  
Italian, Lithuanian, Polish, Portuguese, Russian, Slovak, Spanish


[[Home]]

#reference #tools 