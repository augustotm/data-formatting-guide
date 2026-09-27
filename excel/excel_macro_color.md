[Back to home](../README.md)

# Excel: Macro — RGB and Hex

This set of VBA snippets is useful when you need to color cells dynamically from numeric values or convert values between RGB and hexadecimal notation. It is especially handy for dashboards, business reports, and custom formatting rules.

## How to use
These functions are intended to be used inside Excel VBA. The first macro colors a selected cell based on R, G, and B values; the others convert between color formats when you need a consistent representation across formulas or scripts.

## Color cell using RGB
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

## Convert Hex to RGB

Use this function when your source data contains color codes like `#FF00AA` and you need to extract one component individually.

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

## Convert RGB to Hex

Use this function when you want to generate a color code in hexadecimal format from red, green, and blue values.

```vba
Function GetHexFromRGB(R As Integer, G As Integer, B As Integer) As String
    If R < 0 Or R > 255 Or G < 0 Or G > 255 Or B < 0 Or B > 255 Then
        GetHexFromRGB = "Error: Values must be between 0 and 255"
    Else
        GetHexFromRGB = "#" & Right("00" & Hex(R), 2) & Right("00" & Hex(G), 2) & Right("00" & Hex(B), 2)
    End If
End Function
```