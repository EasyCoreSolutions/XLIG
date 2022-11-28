
# 매크로 활용

앞서 매크로를 통해 엑셀에 값을 선택하여 불러오는 방법을 알아보았습니다.
해당 값들을 코드에 적용시키는법을 알아보겠습니다.

---

<h4>변수 입력</h4>
변수 입력은 필수와 선택조건을 선택하고 안에 셀 주소를 입력하여 사용합니다.

[`예시`](./)

```
[[@@셀주소@@]]
[##셀주소##]
```
<br>
<h5>조건 선택</
h5>
필수조건과 선택조건 2가지 방법이 있습니다.

<table><th>형태</th><th>타입</th><th>설명</th>
<tr><td align=center>[[  ]]</td><td>필수 조건</td><td>셀주소에 값이 반드시 있어야 한다.</td></tr>
<tr><td align=center>[  ]</td><td>선택 조건</td><td>셀주소에 값이 없다면 생략하고 실행된다.</td></tr>
</table>

[`예시`](./)

```
SELECT * FROM DUAL WHERE 1=1 [[조건 = @@B1@@]]
 - B1셀에 값이 없을 경우 실행이 되지않음

SELECT * FROM DUAL WHERE 1=1 [조건 = @@B1@@]
 - B1셀에 값이 없으면 [ ] 부분 생략되고 실행
```

<h5>변수 형태</h5>



<table><th>형태</th><th>타입</th><th>설명</th><th>셀 값 예시</th>
<tr><td align=center>##</td><td>숫자값</td><td>셀에 입력되는 값이 숫자일 경우</td><td>17</td></tr>
<tr><td align=center>@@</td><td>단일 문자</td><td>셀에 입력되는 값이 하나의 단어일 경우</td><td>'우수'</td></tr>
<tr><td align=center>$$</td><td>복수 문자</td><td>셀에 입력되는 값이 복수의 단어일 경우</td><td>'최우수','우수','일반'</td></tr>
</table>

[`예시`](./)

```
B1 = 202201, B2 = 202206
SELECT * FROM DUAL WHERE 1=1 [AND YYYYMM BETWEEN ##B1##] [AND ##B2##]
 -> SELECT * FROM DUAL WHERE 1=1 AND YYYYMM BETWEEN 202001 AND 202206

B1 = '우수'
SELECT * FROM DUAL WHERE 1=1 [AND GRADE = @@B1@@]
 -> SELECT * FROM DUAL WHERE 1=1 AND AND GRADE = '우수

 B1 = '최우수','우수','일반'
 SELECT * FROM DUAL WHERE 1=1 [AND GRADE IN ($$B1$$)]
 -> SELECT * FROM DUAL WHERE 1=1 AND GRADE IN ('최우수','우수','일반')
```
