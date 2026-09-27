[Back to home](../README.md)

# Power BI: Useful DAX functions

### 1. Calendar table
- Name: `0_Calendar`
- Type: `Calculated Table`
- Description: this creates a calendar table covering the minimum and maximum dates found across three source tables.
```dax
0_Calendar =

VAR _min_tb_1 = MIN('TB_1'[date])
VAR _min_tb_2 = MIN('TB_2'[date])
VAR _min_tb_3 = MIN('TB_3'[date])
VAR _min_all = MINX({(_min_tb_1),(_min_tb_2),(_min_tb_3)},[Value])

VAR _max_tb_1 = MAX('TB_1'[date])
VAR _max_tb_2 = MAX('TB_2'[date])
VAR _max_tb_3 = MAX('TB_3'[date])
VAR _max_all = MAXX({(_max_tb_1),(_max_tb_2),(_max_tb_3)},[Value])

RETURN
CALENDAR(_min_all, _max_all)
```

### 2. Week related columns

- Name: `0_week_number`
- Type: `Calculated Column`
- Description: this calculates the week number based on the date and the year reference.
```dax
0_week_number =

VAR _year_pivot = 1900
VAR _date = 'Calendar'[Date]

RETURN
WEEKNUM(_date,1) + 52 * (YEAR(_date) - _year_pivot)
```

- Name: `0_week_start`
- Type: `Calculated Column`
- Description: this calculates the first date of each week.
```dax
0_week_start =

VAR _date = 'Calendar'[Date]

RETURN
CALCULATE(
    MIN(_date),
    ALLEXCEPT(
        'Calendar',
        'Calendar'[0_week_number]
    )
)
```

- Name: `0_week_end`
- Type: `Calculated Column`
- Description: this calculates the last date of each week.
```dax
0_week_end =

VAR _date = 'Calendar'[Date]

RETURN
CALCULATE(
    MAX(_date),
    ALLEXCEPT(
        'Calendar',
        'Calendar'[0_week_number]
    )
)
```
