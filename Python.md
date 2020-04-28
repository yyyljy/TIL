# 파이썬

## 기본

1. 소문자 : string.lower()

2. 대문자 : string.upper()

3. 배열 생성 : lang = []

4. 배열 요소 추가 : lang.append("test")

5. for문

   for i in range(5):
       print(i)

6. 양 끝 공백제거 : Erase_space.strip()
7. 문자열 연결 : "-".join(Charaters)
8. 문자열 존재 확인 : "isThere?" in senetence

## pandas

> 참고 페이지 : [10 minutes to pandas](https://pandas.pydata.org/docs/getting_started/10min.html?highlight=minutes)
>
> cheet sheet : [치트시트.pdf](https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf)

1. import

   ```python
   import pandas as pd
   ```

2. dataframe 만들기

   ```python
   df = pd.DataFrame(
   {"a" : [4 ,5, 6],
   "b" : [7, 8, 9],
   "c" : [10, 11, 12]},
   index = [1, 2, 3])
   ```

   ![image-20200423231454006](C:\Users\Lee\AppData\Roaming\Typora\typora-user-images\image-20200423231454006.png)

![image-20200423231731467](C:\Users\Lee\AppData\Roaming\Typora\typora-user-images\image-20200423231731467.png)



```
df.groupby(["a"])["b"].mean()
```



## chart(도표) 그리기

```
df.plot.density()
df.plot.bar()
df.plot.area()
df.plot()
```

![image-20200423232928443](C:\Users\Lee\AppData\Roaming\Typora\typora-user-images\image-20200423232928443.png)



## csv

```python
pd.read_csv("data/filename", encoding="cp949")
pd.read_csv("directory/filename", encoding="euc-kr")
```

