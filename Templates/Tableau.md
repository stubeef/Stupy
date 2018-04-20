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
	DAY_IN_FISCAL_YEAR_CODE,
        --FISCAL_YEAR_CODE,
	1 YESTERDAY_FLAG
	FROM CDMP..DATE_DIM
	WHERE DAY_DATE = NOW()-1
)

CASE WHEN FISCAL_YEAR_CODE = 2018 THEN CASE WHEN CAST(A.SESSION_START_DAY_IN_FISCAL_YEAR_CODE AS INT) <= MAX(C.DAY_IN_FISCAL_YEAR_CODE) OVER()  THEN 1 ELSE 0 END 
ELSE 1 END WTD_FLAG,

LEFT JOIN WTD C ON A.DAY_IN_FISCAL_YEAR_CODE = C.DAY_IN_FISCAL_YEAR_CODE



