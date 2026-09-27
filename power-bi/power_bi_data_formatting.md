[Back to home](../README.md)

# Power BI: Currency formatting

These format strings are useful when you need to display monetary values consistently in reports, especially for Brazilian and U.S. currency. They help define how values appear for positive, negative, and zero cases while keeping the numbers readable.

## When to use
Use these patterns in Power BI formatting settings or custom DAX expressions when the report must show values such as `R$ 1.200`, `US$ 2.5K`, or `R$ 1.2M` without changing the underlying numeric result.

These formats are especially helpful in business dashboards, where readability matters more than raw precision.

## Currency formatting — R$ (BRL)

### Default
```dax
"R$#,0;-R$#,0;R$#,0"
```

### Thousands
```dax
"R$#,0,.00 K;-R$#,0,.00 K;R$#,0,.00 K"
```

### Millions
```dax
"R$#,0,,.00 M;-R$#,0,,.00 M;R$#,0,,.00 M"
```

## Currency formatting — US$ (USD)

### Default
```dax
"\$#,0;(\$#,0);\$#,0"
```

### Thousands
```dax
"\$#,0,.00 K;(\$#,0,.00) K;\$#,0,.00"
```

### Millions
```dax
"\$#,0,,.00 M;(\$#,0,,.00) M;\$#,0,,.00"
```

### Dynamic formatting example

Use this pattern when the currency depends on a slicer or measure value and you want to assign a format string automatically.

This is useful when the same report needs to switch between BRL and USD based on user selection.

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

## Notes
- These format strings affect only the display, not the underlying numeric value.
- Use `K` and `M` when you want compact values in dashboards.
- Positive, negative, and zero formats should always be defined explicitly for clearer presentation.
- For more advanced scenarios, you can combine this with slicers or measures to switch currency dynamically.