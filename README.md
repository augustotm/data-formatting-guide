# :blue_book: Data Formating Guide

### :bar_chart: Power BI

#### :money_with_wings:Currency formatting - R$ (BRL)

##### Default
```dax
"R$#,0;-R$#,0;R$#,0"
```
##### Thousands
```dax
"R$#,0,.00 K;-R$#,0,.00 K;R$#,0,.00 K"
```
##### Millions
```dax
"R$#,0,,.00 M;-R$#,0,,.00 M;R$#,0,,.00 M"
```

### :dollar: Currency formatting - US$ (USD)

##### Default
```dax
"\$#,0;(\$#,0);\$#,0"
```
##### Thousands
```dax
"\$#,0,.00 K;(\$#,0,.00) K;\$#,0,.00"
```
##### Millions
```dax
"\$#,0,,.00 M;(\$#,0,,.00) M;\$#,0,,.00"
```

```dax
var _currency = SELECTEDVALUE(D_Slicer_Currency[currency])
var _value_brl = ABS([m.rv_atual_brl])
var _value_usd = ABS([m.rv_atual_usd])


RETURN
SWITCH(
    TRUE(),
    _currency = "BRL",
    SWITCH(
        TRUE(),
        _value_brl >= 10^6, "R$#,0,,.00 M;-R$#,0,,.00 M;R$#,0,,.00 M",
        _value_brl >= 10^3, "R$#,0,.00 K;-R$#,0,.00 K;R$#,0,.00 K",
        "R$#,0;-R$#,0;R$#,0"
    )
    ,
    _currency = "USD",
    SWITCH(
        TRUE(),
        _value_usd >= 10^6, "\$#,0,,.00 M;(\$#,0,,.00) M;\$#,0,,.00",
        _value_usd >= 10^3, "\$#,0,.00 K;(\$#,0,.00) K;\$#,0,.00",
        "\$#,0;(\$#,0);\$#,0"
    )
)
```
