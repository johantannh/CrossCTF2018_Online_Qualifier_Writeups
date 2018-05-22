# CrossCTF2018 Online Qualifier Writeups
## Team J2K
[Jun Hao](https://github.com/xephersteel) and [Kelvin Chu](https://github.com/x3sphiorx)
## Challenges
<h3 align="center">WEB</h3>

---

### Contents
[1. QuirkyScript 1](#1. QuirkyScript 1)
2. QuirkyScript 2
3. QuirkyScript 3
4. QuirkyScript 4
5. QuirkyScript 5
6. BabyWeb

#### 1. QuirkyScript 1
##### Problem
```javascript
var flag=require("./flag.js");
var express=require('express')
var app=express()
app.get('/flag',function(req,res){
  if(req.query.first){
    if(req.query.first.length==8&&req.query.first==",,,,,,,"){
        res.send(flag.flag);
        return;
    }
  }
  res.send("Try to solve this.");
});
app.listen(31337)
```
##### Solution
We first try to find out what is this script all about. A quick google search will allow you to find out that it is ExpressJS. From there we read the API under https://expressjs.com/en/api.html to find out how it works.

After analysis of the code, we learn that we are required to trigger the condition to get the flag. 

For this case we have to find out how to get length 8 with only 7 commas???


#### 3. QuirkyScript 3
##### Problem
```javascript
var flag = require("./flag.js");
var express = require('express')
var app = express()

app.get('/flag', function (req, res) {
    if (req.query.third) {
        if (Array.isArray(req.query.third)) {
            third = req.query.third;
            third_sorted = req.query.third.sort();
            if (Number.parseInt(third[0]) > Number.parseInt(third[1]) && third_sorted[0] == third[0] && third_sorted[1] == third[1]) {
                res.send(flag.flag);
                return;
            }
        }
    }
    res.send("Try to solve this.");
});

app.listen(31337)
```
##### Solution
As in previous QuirkyScripts, this problem also uses a get request therefore we have to append a ? behind the 
given website link (http://ctf.pwn.sg:8083/flag) and add a third variable (third=), therefore the whole website link will be 
http://ctf.pwn.sg:8083/flag?third=  . After which, we will have to find the correct value for third so that the flag can be obtained.

After analysis of the above js, we found out that there is a third_sorted variable being declared and assigned by the method sort(). 
Also after sorting, the 1st and 2nd elements of both arrays (third and third_sorted)  has to be equal. But first element of the third variable (array) has to have a larger numerical value 
than the 2nd element. After taking a look at the [specs for sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort),
we found out that the sorting order is according to string unicode points. Hence, by using a value of 20 for the first element and 3 for the 2nd element,
(20 has a smaller unicode value than 3 but has a larger numerical value than 3), we are able to get the flag.
Other value pairs such as 11 and 2 will work as well.

Final link to get flag: >http://ctf.pwn.sg:8083/flag?third=20&third=3


Flag: >CrossCTF{th4t_r0ck3t_1s_hug3}



#### 5. QuirkyScript 5
##### Problem
```javascript
var flag = require("./flag.js");
var express = require('express')
var app = express()

app.get('/flag', function (req, res) {
    var re = new RegExp('^I_AM_ELEET_HAX0R$', 'g');
    if (re.test(req.query.fifth)) {
        if (req.query.fifth === req.query.six && !re.test(req.query.six)) {
            res.send(flag.flag);
        }
    }
    res.send("Try to solve this.");
});

app.listen(31337)
```
##### Solution
As in previous QuirkyScripts, this problem also uses a get request therefore we have to append a ? behind the 
given website link (http://ctf.pwn.sg:8085/flag) and add a 'fifth' and 'six' variable (fifth=&six=), therefore the whole website link will be 
http://ctf.pwn.sg:8083/flag?fifth=&six=  . After which, we will have to find the correct value for fifth and siz so that the flag can be obtained.

After analysis of the above js, we found out that a regular expression is being used to test for the value of 
fifth and six. 

Regexp info [source] (http://www.regular-expressions.info/anchors.html)
```
^ - matches the position before the first character in the string. 
$ - matches right after the last character in the string 
g - global match; find all matches rather than stopping after the first match

Apparently the Regexp object keeps track of the last index where the match occured when the global (g) flag is used.
therfore, when the test method is executed the second time, it will return false even though the string being tested
is still the same. [More info](https://stackoverflow.com/questions/1520800/why-does-a-regexp-with-global-flag-give-wrong-results)



Final link to get flag: >http://ctf.pwn.sg:8085/flag?fifth=I_AM_ELEET_HAX0R&six=I_AM_ELEET_HAX0R


Flag: >CrossCTF{1_am_n1k0las_ray_zhizihizhao}

Team J2K

