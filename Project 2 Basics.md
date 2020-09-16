

```python
from csv import reader
opened_file=open("hacker_news.csv")
hn=reader(opened_file)
hn=list(hn)
print(hn[:4])
```

    [['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at'], ['12224879', 'Interactive Dynamic Video', 'http://www.interactivedynamicvideo.com/', '386', '52', 'ne0phyte', '8/4/2016 11:52'], ['10975351', 'How to Use Open Source and Shut the Fuck Up at the Same Time', 'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/', '39', '10', 'josep2', '1/26/2016 19:30'], ['11964716', "Florida DJs May Face Felony for April Fools' Water Joke", 'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/', '2', '1', 'vezycash', '6/23/2016 22:20']]



```python
headers=hn[0]
hn=hn[1:]
print(headers)
print(hn[:4])
```

    ['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at']
    [['12224879', 'Interactive Dynamic Video', 'http://www.interactivedynamicvideo.com/', '386', '52', 'ne0phyte', '8/4/2016 11:52'], ['10975351', 'How to Use Open Source and Shut the Fuck Up at the Same Time', 'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/', '39', '10', 'josep2', '1/26/2016 19:30'], ['11964716', "Florida DJs May Face Felony for April Fools' Water Joke", 'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/', '2', '1', 'vezycash', '6/23/2016 22:20'], ['11919867', 'Technology ventures: From Idea to Enterprise', 'https://www.amazon.com/Technology-Ventures-Enterprise-Thomas-Byers/dp/0073523429', '3', '1', 'hswarna', '6/17/2016 0:01']]



```python
ask_posts=[]
show_posts=[]
other_posts=[]
for row in hn:
    title=row[1]
    title=title.lower()
    if title.startswith('ask hn'):
        ask_posts.append(row)
    elif title.startswith('show hn'):
        show_posts.append(row)
    else:
        other_posts.append(row)
print(len(ask_posts))
print(len(show_posts))
print(len(other_posts))
```

    1744
    1162
    17194



```python
total_ask_comments=0
for row in ask_posts:
    comments=int(row[4])
    total_ask_comments+=comments
avg_ask_comments=total_ask_comments/len(ask_posts)
print(avg_ask_comments)

total_show_comments=0
for row in show_posts:
    comments=int(row[4])
    total_show_comments+=comments
avg_show_comments=total_show_comments/len(show_posts)

print(avg_show_comments)

```

    14.038417431192661
    10.31669535283993



```python
import datetime as dt
result_list=[]
for row in ask_posts:
    time_comments_list=[]
    created_at=row[6]
    time_comments_list.append(created_at)
    comments=int(row[4])
    time_comments_list.append(comments)
    result_list.append(time_comments_list)
counts_by_hour={}
comments_by_hour={}
for row in result_list:
    date=dt.datetime.strptime(row[0], "%m/%d/%Y %H:%M")
    hour=date.hour
    if hour not in counts_by_hour:
        counts_by_hour[hour]=1
        comments_by_hour[hour]=row[1]
    else:
        counts_by_hour[hour]+=1
        comments_by_hour[hour]+=row[1]
    
```


```python
avg_by_hour=[]

for hour_item in counts_by_hour:
    hour_avgcom_list=[]
    hour_avgcom_list.append(hour_item)
    average_comments=comments_by_hour[hour_item]/counts_by_hour[hour_item]
    hour_avgcom_list.append(average_comments)
    avg_by_hour.append(hour_avgcom_list)

print(avg_by_hour)
```

    [[0, 8.127272727272727], [1, 11.383333333333333], [2, 23.810344827586206], [3, 7.796296296296297], [4, 7.170212765957447], [5, 10.08695652173913], [6, 9.022727272727273], [7, 7.852941176470588], [8, 10.25], [9, 5.5777777777777775], [10, 13.440677966101696], [11, 11.051724137931034], [12, 9.41095890410959], [13, 14.741176470588234], [14, 13.233644859813085], [15, 38.5948275862069], [16, 16.796296296296298], [17, 11.46], [18, 13.20183486238532], [19, 10.8], [20, 21.525], [21, 16.009174311926607], [22, 6.746478873239437], [23, 7.985294117647059]]



```python
swap_avg_by_hour=[]

for row in avg_by_hour:
    swapped_list=[]
    swapped_list.append(row[1])
    swapped_list.append(row[0])
    swap_avg_by_hour.append(swapped_list)
print(swap_avg_by_hour)

sorted_swap=sorted(swap_avg_by_hour, reverse=True)
print(sorted_swap)

print("Top 5 Hours for Ask Post Comments")

for row in sorted_swap[0:5]:
    the_hour=str(row[1])
    datetime_hour=dt.datetime.strptime(the_hour, "%H")
    formatted_hour= datetime_hour.strftime("%H:%M")
    print(formatted_hour)
    print("{hour}: {comments:.2f} average comments per post".format(hour=formatted_hour, comments=row[0]))
```

    [[8.127272727272727, 0], [11.383333333333333, 1], [23.810344827586206, 2], [7.796296296296297, 3], [7.170212765957447, 4], [10.08695652173913, 5], [9.022727272727273, 6], [7.852941176470588, 7], [10.25, 8], [5.5777777777777775, 9], [13.440677966101696, 10], [11.051724137931034, 11], [9.41095890410959, 12], [14.741176470588234, 13], [13.233644859813085, 14], [38.5948275862069, 15], [16.796296296296298, 16], [11.46, 17], [13.20183486238532, 18], [10.8, 19], [21.525, 20], [16.009174311926607, 21], [6.746478873239437, 22], [7.985294117647059, 23]]
    [[38.5948275862069, 15], [23.810344827586206, 2], [21.525, 20], [16.796296296296298, 16], [16.009174311926607, 21], [14.741176470588234, 13], [13.440677966101696, 10], [13.233644859813085, 14], [13.20183486238532, 18], [11.46, 17], [11.383333333333333, 1], [11.051724137931034, 11], [10.8, 19], [10.25, 8], [10.08695652173913, 5], [9.41095890410959, 12], [9.022727272727273, 6], [8.127272727272727, 0], [7.985294117647059, 23], [7.852941176470588, 7], [7.796296296296297, 3], [7.170212765957447, 4], [6.746478873239437, 22], [5.5777777777777775, 9]]
    Top 5 Hours for Ask Post Comments
    15:00
    15:00: 38.59 average comments per post
    02:00
    02:00: 23.81 average comments per post
    20:00
    20:00: 21.52 average comments per post
    16:00
    16:00: 16.80 average comments per post
    21:00
    21:00: 16.01 average comments per post



```python

```
