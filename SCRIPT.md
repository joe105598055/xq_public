### 多 sherry
```
input: title("sherry多","策略名稱");
input: Length1(13,"低MA");
input: Length2(34,"高MA");
value1 = Average(Getfield("Close","5"),Length1);
value2 = Average(Getfield("Close","5"),Length2);
value3 = Average(Getfield("Close","5"), 20);
value4 = K_Value(9,3);
value5 = D_Value(9,3);

if(value1 cross over value2) and value1 > value3 and value2 > value3 and value4 > value5 Then
Begin
    Print(File("C:\Users\sherr\Desktop\XQ\xq.log"),"多單觸發",title,NumToStr(DateTime,0),symbol,NumToStr(Getfield("Close"),0));
    RetVal = 1;
End;
```

### 空 sherry
```
input: title("sherry空","策略名稱");
input: Length1(13,"低MA");
input: Length2(34,"高MA");
value1 = Average(Getfield("Close","5"),Length1);
value2 = Average(Getfield("Close","5"),Length2);
value3 = Average(Getfield("Close","5"), 20);
value4 = K_Value(9,3);
value5 = D_Value(9,3);

if(value1 cross under value2) and value1 < value3 and value2 < value3 and value4 < value5 Then
Begin
    Print(File("C:\Users\sherr\Desktop\XQ\xq.log"),"空單觸發",title,NumToStr(DateTime,0),symbol,NumToStr(Getfield("Close"),0));
    RetVal = 1;
End;
```

### 多 will (2022/07/12)
```
input: title("will多","策略名稱");
input: rvalue(10,"R 值區間");
value1 = MaxList(getField("high")[2],getField("high")[1]);
value2 = MinList(getField("low")[2],getField("low")[1]);
value3 = value1 - value2;
value4 = K_Value(9,3);
value5 = D_Value(9,3);

if(getField("high","1") > value1 and value3 <= rvalue and value4 > value5) Then
Begin
    Print(File("C:\Users\sherr\Desktop\XQ\xq.log"),"多單觸發",title,NumToStr(DateTime,0),symbol,NumToStr(Getfield("Close","tick"),0));
    RetVal = 1;
End;
```

### 空 will (2022/07/12)
```
input: title("will空","策略名稱");
input: rvalue(10,"R 值區間");
value1 = MaxList(getField("high")[2],getField("high")[1]);
value2 = MinList(getField("low")[2],getField("low")[1]);
value3 = value1 - value2;
value4 = K_Value(9,3);
value5 = D_Value(9,3);

if(getField("low","1") < value2 and value3 <= rvalue and value4 < value5) Then
Begin
    Print(File("C:\Users\sherr\Desktop\XQ\xq.log"),"空單觸發",title,NumToStr(DateTime,0),symbol,NumToStr(Getfield("Close","tick"),0));
    RetVal = 1;
End;
```
