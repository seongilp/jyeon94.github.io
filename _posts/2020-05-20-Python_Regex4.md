---
title: "정규표현식4"
tags: [Python, TIL]
style:
color:
description: Last Part of Regex
---

참고:[위키독스](https://wikidocs.net/4309)   

### 전방탐색

전방 탐색 확장 구문을 사용하면 암호문처럼 알아보기 어렵게 바뀐다. <br/>
하지만, 이 전방 탐색이 꼭 필요한 경우가 있다.


```python
import re
```


```python
p = re.compile(".+:")
m = p.search("http://google.com")
print(m.group())
```

    http:
    

만약 http:라는 검색 결과에서 :을 제외하고 출력하려면 어떻게 해야 할까? <br/>
위 예는 간단하지만 훨씩 복잡한 정규식이어서 그룹핑이 불가능하다는 조건이 추가된다면 <br/>
이럴때 사용하는 것이 전방탐색이다.

전방 탐색에는 긍정(Positive)와 부정(Negative)의 2종류가 있다.

- 긍정형 전방 탐색((?=...)) - "..."에 해당되는 정규식과 매치되어야 하며 조건이 만족되어도 문자열이 소비되지 않는다.<br/>
- 부정형 전방 탐색(?!...) - "..."에 해당되는 정규식과 매치되지 않아야 하며 조건이 통과되어도 문자열이 소비되지 않는다. <br/>

#### 긍정형 전방 탐색


```python
p = re.compile(".+(?=:)")
m = p.search("http://google.com")
print(m.group())
```

    http
    

":"에 해당하는 문자열이 정규식 엔진에 의해 소비되지 않아 검색 결과에서는 ":"이 제거된 후 돌려준다.

#### 부정형 전방 탐색

foo.bar,autoexec.bat,sendmail.cf에서 bat인 파일은 제외해야 할 경우 전방형 보다 부정형 사용시 간단하게 처리된다.

밑의 정규식은 bat가 아닌 경우에만 통관된다.bat 문자열이 있는지 조사하는 과정에서 문자열이 소보되지 않는다.   
그러기 때문에 bat가 아니라고 판단되면 그 이후 정규식 매치가 진행된다.   
*[.](?!bat$).*$

### 문자열 바꾸기

sub method 사용시 매칭되는 부분을 다른 문자로 바꿀 수 있다.


```python
p = re.compile('(blue|white|red)')
p.sub('colour','blue socks and red shoes')
```




    'colour socks and colour shoes'



count를 추가하면 바꾸기 횟수를 제어할 수 있다.


```python
p.sub('colour','blue socks and red shoes',count=1)
```




    'colour socks and red shoes'



#### sub 메서드 사용 시 참조 구문 사용하기


```python
p = re.compile(r"(?P<name>\w+)\s+(?P<phone>(\d+)[-]\d+[-]\d+)")
print(p.sub("\g<phone> \g<name>","park 010-1234-1234"))
```

    010-1234-1234 park
    

sub의 바꿀 문자열 부분에 \g<그룹이름>을 사용하면 참조 가능하다.


```python
# 이름 대신 참조 번호를 사용해도 똑같은 결과를 반환한다.
p = re.compile(r"(?P<name>\w+)\s+(?P<phone>(\d+)[-]\d+[-]\d+)")
print(p.sub("\g<2> \g<1>","park 010-1234-1234"))
```

    010-1234-1234 park
    
