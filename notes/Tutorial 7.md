---
title: "Tutorial 7"
created: '2019-09-15T23:36:51.143Z'
modified: '2019-09-15T23:56:44Z'
tags: [Notebooks/Computer Science Degree, Tutorials]
---

# Tutorial 7

### Part A

1. Describe the structure of the three dimension tables. What are the attributes of each of the dimension tables?

    * DESC TIME;
```
Name       Null? Type
---------- ----- -------
TIME_ID          CHAR(6)
TIME_YEAR        CHAR(4)
TIME_MONTH       CHAR(2)
```

    * DESC PILOT;
```
Name           Null? Type
-------------- ----- ----------
EMP_NUM              NUMBER(10)
PIL_LICENSE          CHAR(25)
PIL_RATINGS          CHAR(25)
PIL_MED_TYPE         CHAR(1)
PIL_MED_DATE         DATE
PIL_PT135_DATE       DATE
```

    * DESC MODEL;
```
Name             Null? Type
---------------- ----- ------------
MOD_CODE               CHAR(10)
MOD_MANUFACTURER       CHAR(15)
MOD_NAME               CHAR(20)
MOD_SEATS              FLOAT(126)
MOD_CHG_MILE           NUMBER(19,4)
MOD_CRUISE             FLOAT(126)
MOD_FUEL               FLOAT(126)
```


2. Describe the structure of the facts table - what attributes does it have?

    * DESC CHARTER_FACT;
```
Name           Null? Type
-------------- ----- -----------
TIME_ID              VARCHAR2(6)
MOD_CODE             CHAR(10)
EMP_NUM              NUMBER(10)
TOT_CHAR_HOURS       NUMBER
TOT_FUEL             NUMBER
REVENUE              NUMBER
```


3. Display the contents of each of the dimensions
4. Display the contents of the fact table

    * SELECT * FROM CHARTER_FACTS;
```
TIME_I MOD_CODE      EMP_NUM TOT_CHAR_HOURS   TOT_FUEL    REVENUE
------ ---------- ---------- -------------- ---------- ----------
199401 C-90A             104            4.4      233.6     2296.2
199401 PA23-250          106           12.3      396.8    3829.12
199401 C-90A             109            9.1      598.7    4723.23
199605 PA23-250          105            3.5        124     924.47
199605 PA31-350          109           14.9      647.1     6124.1
199606 C-90A             106            4.1      272.8    2152.02
199607 C-90A             105            6.6      459.9    4202.58
199607 PA31-350          105            5.3      234.6     2246.6
199608 PA31-350          109           10.3        464     4187.7
199608 C-90A             104            6.7      459.5    4392.15
199608 PA31-350          106            6.1      302.6     2199.6
```


```
select * from time;
select * from pilot;
select * from model;
```

 
```
TIME_I TIME TI
------ ---- --
199401 1994 1
199402 1994 2
199403 1994 3
199404 1994 4
199405 1994 5
199406 1994 6
199407 1994 7
199408 1994 8
199409 1994 9
199410 1994 10
199411 1994 11
```

 
```
TIME_I TIME TI
------ ---- --
199412 1994 12
199501 1995 1
199502 1995 2
199503 1995 3
199504 1995 4
199505 1995 5
199506 1995 6
199507 1995 7
199508 1995 8
199509 1995 9
199510 1995 10
```

 
```
42 rows selected.
```

 
 
```
EMP_NUM PIL_LICENSE               PIL_RATINGS               P PIL_MED_D PIL_PT135
---------- ------------------------- ------------------------- - --------- ---------
101 ATP                       SEL/MEL/Instr/CFII        1 12/APR/96 15/JUN/96
104 ATP                       SEL/MEL/Instr             1 10/JUN/96 23/MAR/96
105 COM                       SEL/MEL/Instr/CFI         2 25/FEB/96 12/FEB/96
106 COM                       SEL/MEL/Instr             2 02/APR/96 24/MAY/96
109 COM                       SEL/MEL/SES/Instr/CFII    1 14/APR/96 21/APR/96
```

 
 
```
MOD_CODE   MOD_MANUFACTURE MOD_NAME              MOD_SEATS MOD_CHG_MILE MOD_CRUISE   MOD_FUEL
---------- --------------- -------------------- ---------- ------------ ---------- ----------
C-90A      Beechcraft      KingAir                       8         2.67        195         66
PA23-250   Piper           Aztec                         6         1.93        160         32
PA31-350   Piper           Navajo Chieftain             10         2.35        180         40
```

### Part B

1. What is the total hours flown by each pilot?
2. Display the total hours flow by each pilot in a descending order.
```
SELECT
emp_num,
SUM(tot_char_hours)
FROM
charter_fact
GROUP BY
emp_num;
SELECT
emp_num,
SUM(tot_char_hours) AS total_hours
FROM
charter_fact
GROUP BY
emp_num
ORDER BY
total_hours DESC;
```

