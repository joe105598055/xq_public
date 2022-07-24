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

### sherry 交易多
```
input: profit_point(30, "停利(點)");
input: loss_point(30, "停損(點)");
var: long_condition(false);
var: stop_condition(false);
input: Length1(13,"低MA");
input: Length2(34,"高MA");
value1 = Average(Getfield("Close","5"),Length1);
value2 = Average(Getfield("Close","5"),Length2);
value3 = Average(Getfield("Close","5"), 20);
value4 = K_Value(9,3);
value5 = D_Value(9,3);

// 13MA 突破 34MA 且兩條均線都要在布林均線上, K > D 
long_condition = value1 cross over value2 and value1 > value3 and value2 > value3 and value4 > value5;
stop_condition = Close < value1 and Close < value2; // 停損條件

if Position = 0 and long_condition then begin
	SetPosition(1, MARKET);	// 市價買進
end;

if Position = 1 and Filled = 1 then begin
	if profit_point > 0 and Close >= FilledAvgPrice + profit_point then begin // 依照成本價格設定停損/停利
		SetPosition(0); // 停利
	end else if stop_condition then begin	
		SetPosition(0); // 停損
	end;
end;
```

### sherry 交易空
```
input: profit_point(30, "停利(點)");
input: loss_point(30, "停損(點)");
var: long_condition(false);
var: stop_condition(false);
input: Length1(13,"低MA");
input: Length2(34,"高MA");
value1 = Average(Getfield("Close","5"),Length1);
value2 = Average(Getfield("Close","5"),Length2);
value3 = Average(Getfield("Close","5"), 20);
value4 = K_Value(9,3);
value5 = D_Value(9,3);

// 13MA 慣破 34MA 且兩條均線都要在布林均線下, K < D 
long_condition = value1 cross under value2 and value1 < value3 and value2 < value3 and value4 < value5;
stop_condition = Close > value1 and Close > value2; // 停損條件

if Position = 0 and long_condition then begin
	SetPosition(-1, MARKET);	// 市價買進
end;

if Position = -1 and Filled = -1 then begin
	if profit_point > 0 and Close <= FilledAvgPrice - profit_point then begin // 依照成本價格設定停損/停利
		SetPosition(0); // 停利
	end else if stop_condition then begin	
		SetPosition(0); // 停損
	end;
end;
```