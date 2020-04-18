# FileData

### 📌 Read & Write FileData

```r
> no=c(1,2,3,4)
> name=c("Apple","Banana","Peach","Berry")
> price=c(500,200,300,400)
> qty=c(5,2,7,9)
> fruit=data.frame(No=no, Name=name, Price=price, Quantity=qty)
> fruit
  No   Name Price Quantity
1  1  Apple   500        5
2  2 Banana   200        2
3  3  Peach   300        7
4  4  Berry   400        9
> save(fruit, file="test.dat") # working directory 에 fruit데이터 저장
```

<img src="./screenshots/02_basic1.png" width="500">

working directory 에 ```test.dat``` 파일이 새로 생겼음을 확인할 수 있다.

```r
> rm(fruit) # fruit 데이터 삭제 
> fruit
Error: object 'fruit' not found
> load("test.dat") # 파일 읽기
> fruit
  No   Name Price Quantity
1  1  Apple   500        5
2  2 Banana   200        2
3  3  Peach   300        7
4  4  Berry   400        9
```

<br/>

### 📌 Read & Write CSV Data

```r
> write.csv(fruit, "fruit.csv")
```

<img src="./screenshots/02_basic2.png" width="500">

<img src="./screenshots/02_basic3.png" width="500">

working directory 에 ```fruit.csv``` 파일이 새로 생겼음을 확인할 수 있다.

```r
> rm(fruit)
> fruit
Error: object 'fruit' not found
> fruit=read.csv("fruit.csv")
> fruit
  X No   Name Price Quantity
1 1  1  Apple   500        5
2 2  2 Banana   200        2
3 3  3  Peach   300        7
4 4  4  Berry   400        9
```

위 명령올 csv 파일을 저장하고 불러올 수 있다.

 
