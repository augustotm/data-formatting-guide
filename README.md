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

### :books: Excel

#### :art: Macro - RGB and Hex

##### Color cell using RGB
```vba
Option Explicit

Function myRGB(R, G, B)

    Dim clr As Long, src As Range, sht As String, f, v

    If IsEmpty(R) Or IsEmpty(G) Or IsEmpty(B) Then
        clr = vbWhite
    Else
        clr = RGB(R, G, B)
    End If

    Set src = Application.ThisCell
    sht = src.Parent.Name

    f = "Changeit(""" & sht & """,""" & _
                  src.Address(False, False) & """," & clr & ")"
    src.Parent.Evaluate f
    myRGB = ""
End Function

Sub ChangeIt(sht, c, clr As Long)
    ThisWorkbook.Sheets(sht).Range(c).Interior.Color = clr
End Sub
```

##### Convert Hex to RGB
```vba
Function GetRGBFromHex(hexColor As String, RGB As String) As String

hexColor = VBA.Replace(hexColor, "#", "")
hexColor = VBA.Right$("000000" & hexColor, 6)

Select Case RGB

    Case "B"
        GetRGBFromHex = VBA.Val("&H" & VBA.Mid(hexColor, 5, 2))

    Case "G"
        GetRGBFromHex = VBA.Val("&H" & VBA.Mid(hexColor, 3, 2))

    Case "R"
        GetRGBFromHex = VBA.Val("&H" & VBA.Mid(hexColor, 1, 2))

End Select

End Function
```

##### Convert RGB to Hex
```vba
Function GetHexFromRGB(R As Integer, G As Integer, B As Integer) As String
    If R < 0 Or R > 255 Or G < 0 Or G > 255 Or B < 0 Or B > 255 Then
        GetHexFromRGB = "Error: Values must be between 0 and 255"
    Else
        GetHexFromRGB = "#" & Right("00" & Hex(R), 2) & Right("00" & Hex(G), 2) & Right("00" & Hex(B), 2)
    End If
End Function
```
