## Date Partition
IF [Week Num] = DatePart('week',TODAY()) THEN "Current_Week"
ELSEIF [Week Num] = DatePart('week',TODAY())-1 THEN "Previous_Week"
ELSEIF [Week Num] > DatePart('week',TODAY()) THEN "Future"
//ELSEIF DatePart('week',TODAY())-2 = [Week Num] THEN "Present_Month"
ELSEIF [Week Num] > DatePart('week',TODAY())-5 THEN "Last_4_weeks"
ELSE "PAST"
END


## Date Selector
CASE [Parameters].[Date Selector]
WHEN 'Date' then [DAY_DATE (copy)]
WHEN 'Day' then [DAY_IN_FISCAL_YEAR_CODE]
WHEN 'Week' then [WEEK_NUM]
WHEN 'Month' then [FISCAL_MONTH]
WHEN 'Quarter' then [FISCAL_QUARTER]
WHEN 'Year' then [FISCAL_YEAR_CODE]
END

## WTD Flag
,WTD AS (
	SELECT
	FISCAL_DAY,
	1 YESTERDAY_FLAG
	FROM DATE_DIM_TABLE
	WHERE DATE = NOW()-1
)

CASE WHEN FISCAL_DAY = 2018 THEN CASE WHEN CAST(A.FISCAL_DAY AS INT) <= MAX(C.FISCAL_DAY) OVER()  THEN 1 ELSE 0 END 
ELSE 1 END WTD_FLAG,

LEFT JOIN WTD C ON A.FISCAL_DAY = C.FISCAL_DAY

## Joins
AND  istrue(length(translate(A.CARRIER_ID,'1234567890',''))=0 and length(A.CARRIER_ID) >0)

## Nested Case
CASE
        WHEN  ORDERS > 0 THEN
                        CASE
                            WHEN DATE > 2015-02-05
                                THEN 1
                            ELSE NULL
                        END
        ELSE NULL
    END AS FIRST_ORDER_FLAG

## Multiple Case
CASE WHEN I.JOIN_DATE < G.DAY_DATE THEN 1
        WHEN I.JOIN_DATE IS NULL AND I.CUSTOMER_ID IS NOT NULL THEN 1
        ELSE 0
    END CUSTOMER_FLAG


