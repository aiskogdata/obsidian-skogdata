1 poly 4 point full precision no props = 315 bytes
1poly 4 point full precision average props = 479 bytes
1poly 4 point half precision no props = 246 bytes

1 poly 4 point = 219 bytes
1poly 1 point = 68 bytes
1 point raw = 37 bytes

metadata only = 192 bytes


Har funnet reele filstørrelser og gjort litt matte. Om vi antar at alle aktører og kommuner inneholder de max 50 tegna de er tillatt i DB så er dette resultatet: 

```
full presisjon (16 desimaler)
        max punkt for et enestående polygon: 708488.5675675676
        max totalt antall punkt ved 400 polygoner: 708154.2702702703
        'max' punkt per polygon ved 400 polygoner: 1770.3856756756757
halv presisjon (8 desimaler)
        max punkt for et enestående polygon: 1248289.380952381
        max totalt antall punkt ved 400 polygoner: 1247700.380952381
        'max' punkt per polygon ved 400 polygoner: 3119.2509523809526
```

Feil marginen ved 5 desimaler er påstålig  maks 1.1 meter https://en.wikipedia.org/wiki/Decimal_degrees så med mindre kundene har noe problem med det, eller har en annen standard ville jeg jo si vi fint kan foreslå en begrensing på 8 desimaler