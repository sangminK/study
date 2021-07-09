# [Oracle] DECODE와 CASE
쓸 때마다 찾아보게 되는 ***조건에 따른 컬럼값 추출과 집계 함수*** 정리 

## DECODE
> ***DECODE(expr, search, result <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
[, search, result ]... <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
[, default ])***

- expr이 search이면 result를 반환
- 일치하는 항목이 없으면 default를 반환
- default를 생략하면 오라클은 null을 반환
- DECODE 함수 안에 DECODE 함수를 중첩으로 사용 가능

<br>

## DECODE 예시
조건에 따른 컬럼값 추출하기 : 
category_id가 10이면 '상의', 20이면 '하의', 30이면 '악세사리', 그 이외는 '기타'로 출력

#### 쿼리
```
SELECT category_id, DECODE(category_id, 10 , '상의',
                                        20 , '하의' ,
                                        30 , '악세사리', '기타') name
  FROM category;
```
#### 결과
<table>
  <tr>
    <th>category_id</th>
    <th>name</th>
  </tr>
  <tr>
    <td>10</td>
    <td>상의</td>
  </tr>
  <tr>
    <td>20</td>
    <td>하의</td>
  </tr>
  <tr>
    <td>30</td>
    <td>악세사리</td>
  </tr>
  <tr>
    <td>40</td>
    <td>기타</td>
  </tr>  
</table>

<br>

## CASE
DECODE 함수에서 비교 연산을 수행하기 위해서는 다른 함수를 사용해야 하지만, CASE 함수는 조건 연산자를 사용 가능

<br>

## CASE 예시
#### 쿼리
```
 SELECT category_id, 
       CASE category_id
         WHEN 10 THEN '상의'
         WHEN 20 THEN '하의'
         WHEN 30 THEN '악세사리'
         ELSE '기타'
       END AS "name"
  FROM category;
```

#### 결과
위 DECODE와 동일

#### WHEN 절 다음에 연산자가 오는 경우
```
SELECT ename ,
       CASE
          WHEN sal < 1000  THEN sal+(sal*0.8)
          WHEN sal BETWEEN 1000 AND 2000 THEN sal+(sal*0.5)
          WHEN sal BETWEEN 2001 AND 3000 THEN sal+(sal*0.3)
          ELSE sal+(sal*0.1)
       END sal
  FROM emp; 
```
<br>

#### MySQL의 IF(예시)
```
SELECT no, name, IF(STRCMP(user_grade, 'F1') = 0, '베스트','일반') AS grade
  FROM users
;
```
<br>

## 문제
***테이블A*** <br>
컬럼 : P_ID, DATE, STATE, .... 기타 등등 엄청 많음

***테이블B(테이블 A를  P_ID, DATE, STATE로 group by 한 결과. 실제 테이블 아님)*** <br>
컬럼 : P_ID, DATE, STATE, CNT

![image](https://user-images.githubusercontent.com/47021861/125030313-e6c85580-e0c5-11eb-9d58-7deab342a7b5.png)

테이블 B를 아래와 같이 P_ID, DATE가 같으면 한 줄로 합쳐서 STATE 별로 필드를 만들어 COUNT값을 보여주려고 한다. <br>
(STATE는 A와 B, 두가지만 존재)

![image](https://user-images.githubusercontent.com/47021861/125030470-2131f280-e0c6-11eb-831f-32e5b42df138.png)

처음에 쿼리로 돌릴 생각을 못하고, 테이블 A의 group by 결과(테이블 B)를 자바에서 for문 돌리려고 했었다.(But, JDK 1.5 사용 중이라 stream()도 못써서 아득했다….) <br>

<br>

## 해결
조건에 따라 count를 하면 되겠다 싶어, DECODE 발견!
```
SELECT p_id
     , date
     , COUNT(DECODE(state, 'A', 1)) cnt_a
     , COUNT(DECODE(state, 'B', 1)) cnt_b
  FROM t
 GROUP BY p_id, date
;
```



<br><br><br>

> 🧐 출처 <br>
> http://www.gurubee.net/lecture/1028 <br>
> https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions040.htm

<br><br><br>



> 무언가를 모른다는 사실을 인지하고 있으면 공부하면 되지만, <br> 
> 모른다는 사실조차 모르는 것이 무섭다. <br>
> DECODE가 어마어마한 함수는 아니지만 그것조차 몰라서, 자바에서 일일이 그룹핑해서 카운트하려고 했었다.. <br>
> 모든 것에 꾸준히 관심을 갖자! <br>
