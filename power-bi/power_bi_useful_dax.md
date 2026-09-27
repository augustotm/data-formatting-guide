[Back to home](../README.md)

# Power BI: Useful DAX functions

These examples show reusable patterns for building a date table and week-based calculations in Power BI. They are useful when you need consistent time-intelligence logic across reports and dashboards.

## Use case
This section is especially helpful when your model needs a reliable calendar dimension and weekly grouping fields for trend analysis, period comparisons, and reporting slices.

## 1. Calendar table
- Name: `0_Calendar`
- Type: `Calculated Table`
- Description: creates a calendar table covering the minimum and maximum dates found across three source tables.

```dax
0_Calendar =

VAR _min_tb_1 = MIN('TB_1'[date])
VAR _min_tb_2 = MIN('TB_2'[date])
VAR _min_tb_3 = MIN('TB_3'[date])
VAR _min_all = MINX({(_min_tb_1), (_min_tb_2), (_min_tb_3)}, [Value])

VAR _max_tb_1 = MAX('TB_1'[date])
VAR _max_tb_2 = MAX('TB_2'[date])
VAR _max_tb_3 = MAX('TB_3'[date])
VAR _max_all = MAXX({(_max_tb_1), (_max_tb_2), (_max_tb_3)}, [Value])

RETURN
CALENDAR(_min_all, _max_all)
```

## 2. Week-related columns

This section creates fields that help group data by week and identify the start and end dates for each period. It is especially useful for weekly reports and comparisons across date ranges.

### 2.1 `0_week_number`
- Name: `0_week_number`
- Type: `Calculated Column`
- Description: calculates the week number based on the date and the reference year.

```dax
0_week_number =

VAR _year_pivot = 1900
VAR _date = 'Calendar'[Date]

RETURN
WEEKNUM(_date, 1) + 52 * (YEAR(_date) - _year_pivot)
```

### 2.2 `0_week_start`
- Name: `0_week_start`
- Type: `Calculated Column`
- Description: calculates the first date of each week.

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

### 2.3 `0_week_end`
- Name: `0_week_end`
- Type: `Calculated Column`
- Description: calculates the last date of each week.

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

## Notes
- Replace the example table and column names with your actual model names.
- These fields are best used in a dedicated date table, not in the fact table.
- This pattern is a solid base for weekly reports, KPI tracking, and time-based comparisons.
