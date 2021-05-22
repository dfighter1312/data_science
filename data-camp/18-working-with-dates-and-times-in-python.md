# Working with Dates and Times in Python

## Chapter 1: Dates and Calendars

### Dates in Python

**Creating date objects**
```python
# Import date
from datetime import date
# Create dates
two_dates = [date(2016, 10, 7), date(2017, 6, 21)]
# Access attributes
two_dates[0].year, two_dates[0].month, two_dates[0].day, two_dates[0].weekday()
```

Weekday count from `0` to `6` (Monday-Sunday)

### Math with dates

```python
# Get the least date
print(min(two_dates))

# Subtract two dates
delta = two_dates[1] - two_dates[0]
print(delta.days)

# Adding date
from datetime import timedelta
td = timedelta(days=5)
print(two_dates[0] + td)      #Output: 2016-10-12
```

### Turning dates into strings

```python
d = date(2017, 11, 5)

print(d)      #Output: 2017-11-05 (YYYY-MM-DD)
# Express the date in ISO 8601 format and put it in a list
print([d.isoformat()])    #Output: ['2017-11-05']

print(d.strftime("%Y"))         #Output: 2017
print(d.strftime("Year is %Y")  #Output: Year is 2017
print(d.strftime(%Y/%m/%d")     #Output: 2017/11/05
```

- `%a`	Tên ngày trong tuần viết tắt	Sun, Mon...
- `%A`	Tên ngày trong tuần viết đầy đủ	Sunday, Monday...
- `%w`	Ngày trong tuần, dạng giá trị số	0, 1, ..., 6
- `%d`	Ngày trong tháng, dạng giá trị số (có giá trị 0 làm đệm trước ngày có 1 chữ số)	01, 02, ..., 31
- `%-d`	Ngày trong tháng, dạng giá trị số	1, 2, ..., 30
- `%b`	Tên tháng viết tắt	Jan, Feb, ..., Dec
- `%B`	Tên tháng viết đầy đủ	January, February...
- `%m`	Tháng trong năm, dạng giá trị số (có giá trị 0 làm đệm trước tháng có 1 chữ số)	01, 02, ..., 12
- `%-m`	Tháng trong năm, dạng giá trị số	1, 2, ..., 12
- `%y`	Giá trị năm 2 chữ số (có giá trị 0 làm đệm trước năm có 1 chữ số)	00, 01, ..., 99
- `%-y`	Giá trị năm 2 chữ số	0, 1, ..., 99
- `%Y`	Giá trị năm đầy đủ	2013, 2019...
- `%H`	Giờ theo hệ 24 tiếng (có giá trị 0 làm đệm trước giờ có 1 chữ số)	00, 01, ..., 23
- `%-H`	Giờ theo hệ 24 tiếng, dạng giá trị số	0, 1, ..., 23
- `%I`	Giờ theo hệ 12 tiếng, dạng giá trị số (có giá trị 0 làm đệm trước giờ có 1 chữ số)	01, 02, ..., 12
- `%-I`	Giờ theo hệ 12 tiếng	1, 2, ..., 12
- `%p`	Giờ địa phương là AM hoặc PM.	AM, PM
- `%M`	Phút, dạng giá trị số (có giá trị 0 làm đệm trước phút có 1 chữ số)	00, 01, ..., 59
- `%-M`	Phút, dạng giá trị số	0, 1, ..., 59
- `%S`	Giây, dạng giá trị số (có giá trị 0 làm đệm trước giây có 1 chữ số)	00, 01, ..., 59
- `%-S`	Giây, dạng giá trị số	0, 1, ..., 59
- `%f`	Micro giây, dạng giá trị số (có giá trị 0 làm đệm trước giây có 1 chữ số)	000000 - 999999
- `%z`	Giờ bù UTC ở dạng +HHMM or -HHMM.	
- `%Z`	Tên múi giờ	
- `%j`	Ngày trong năm, dạng giá trị số (có giá trị 0, 00 làm đệm trước ngày có 1 và 2 chữ số)	001, 002, ..., 366
- `%-j`	Ngày trong năm, dạng giá trị số	1, 2, ..., 366
- `%U`	Số tuần trong năm (tính Chủ nhật là ngày đầu tuần). Tất cả các ngày trong năm mới trước Chủ nhật đầu tiên được coi là trong tuần 0.	00, 01, ..., 53
- `%W`	Số tuần trong năm (tính Thứ Hai là ngày đầu tuần). Tất cả các ngày trong năm mới trước Thứ Hai đầu tiên được coi là trong tuần 0.	00, 01, ..., 53
- `%c`	Trả về ngày giờ	Mon Sep 30 07:06:05 2013
- `%x`	Trả về ngày	09/30/13
- `%X`	Trả về giờ	07:06:05
- `%%`	Ký tự '%' theo nghĩa đen.	%

## Chapter 2: Combining Dates and Times

### Dates and times

Example: `October 1 2017, 3:23:24.5 PM`

```python
from datetime import datetime
dt = datetime(2017, 10, 1, 15, 23, 25, 500000)

# Replacing parts of a datetime
dt_hr = dt.replace(minute=0, second=0)
print(dt_hr)    #Output: '2017-10-1 15:00:00'
```

### Printing and parsing datetimes

**Printing**
```python
dt = datetime(2017, 12, 30, 15, 19, 13)

print(dt.strftime("%Y-%m-%d"))              #Output: '2017-12-30'
print(dt.strftime("%Y-%m-%d %H:%M:%S"))     #Output: '2017-12-30 15:19:13'
print(dt.strftime("%H:%M:%S on %d/%m/%Y"))  #Output: '15:19:13 on 30-12-2017'
```

**Parsing**
```python
from datetime import datetime

dt = datetime.strptime("12/30/2017 15:19:13", 
                       "%m/%d/%Y %H:%M:%S")
                       
print(type(dt))         #Output: <class 'datetime.datetime'>
```

Parsing a timestamp
```python
ts = 1514665153.0
print(datetime.fromtimestamp(ts))     #Output: '2017-12-30 15:19:13'
```

### Working with durations

```python
start = datetime(2017, 10, 8, 23, 46, 47)
end = datetime(2017, 10, 9, 0, 10, 57)

duration = end - start

print(duration.total_seconds())     #Output: 1450.0
```

```python
# Import timedelta
from datetime import timedelta
delta1 = timedelta(days=1, seconds=1)
print(start + delta1)     #Output: '2017-10-9 23:46:48'

delta2 = timedeltae(seconds=-1)
print(start + delta1)     #Output: '2017-10-8 23:46:46'
```

## Chapter 3: Time Zones and Daylight Saving

### UTC offsets

```python
from datetime import datetime, timedelta, timezone

ET = timezone(timedelta(hours=-5))
dt = datetime(2017, 12, 30, 15, 9, 3, tzinfo=ET)
print(dt)     #Output: '2017-12-30 15:09:03-05:00'

# Change between timezones
IST = timezone(timedelta(hours=5, minutes=30))
dt.astimezone(IST)      #Output: '2017-12-31 01:39:03+05:30'

#Changing tzinfo
dt.replace(tzinfo=timezone.utc))
dt.astimezone(timezone.utc)
```

### Time zone database

```python
from datetime import datetime
from dateutil import tz

et = tz.gettz('America/New_York')     # Format: 'Continent/City`
last = datetime(2017, 12, 30, 15, 9, 3, tzinfo=et)
print(last)       #Output: '2017-12-30 15:09:03-05:00'
```

### Starting daylight saving time

### Ending daylight saving time

## Chapter 4: Easy and Powerful: Dates and Times in Pandas

### Reading date and time data in Pandas

**Pandas example**
```python
import pandas as pd
df = pd.read_csv('a.csv', parse_dates=['Start date', 'End date'])

# Or:
df['Start date'] = pd.to_datetime(df['Start date'], format = "%Y-%m-%d %H:%M:%S")
```

### Summarizing datetime data in Pandas

- `.mean()`, `.sum()`
- `.resample('M', on='Start date')`   

### Additional datetime methods in Pandas

Timezones in Pandas

`df.dt.tz_localize('America/New_York', ambiguos='NaT')`

Datetime operation

`.dt.year`, `.dt.weekday_name`

Shift the indexes forward one, padding with NaT

`df['End date'].shift(1)`
