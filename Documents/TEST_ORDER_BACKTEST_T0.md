

```python
import QUANTAXIS as QA
```

    QUANTAXIS>> start QUANTAXIS
    QUANTAXIS>> Welcome to QUANTAXIS, the Version is 1.0.46
    QUANTAXIS>>  
     ```````````````````````````````````````````````````````````````````````````````````````````````````````````````````````` 
      ``########`````##````````##``````````##`````````####````````##```##########````````#``````##``````###```##`````######`` 
      `##``````## ```##````````##`````````####````````##`##```````##```````##```````````###``````##````##`````##```##`````##` 
      ##````````##```##````````##````````##`##````````##``##``````##```````##``````````####```````#```##``````##```##``````## 
      ##````````##```##````````##```````##```##```````##```##`````##```````##`````````##`##```````##`##```````##````##``````` 
      ##````````##```##````````##``````##`````##``````##````##````##```````##````````##``###```````###````````##`````##`````` 
      ##````````##```##````````##``````##``````##`````##`````##```##```````##```````##````##```````###````````##``````###```` 
      ##````````##```##````````##`````##````````##````##``````##``##```````##``````##``````##`````##`##```````##````````##``` 
      ##````````##```##````````##````#############````##```````##`##```````##`````###########`````##``##``````##`````````##`` 
      ###```````##```##````````##```##```````````##```##```````##`##```````##````##`````````##```##```##``````##```##`````##` 
      `##``````###````##``````###``##`````````````##``##````````####```````##```##``````````##``###````##`````##````##`````## 
      ``#########``````########```##``````````````###`##``````````##```````##``##````````````##`##``````##````##`````###``### 
      ````````#####`````````````````````````````````````````````````````````````````````````````````````````````````````##`` 
      ``````````````````````````````````````````````````````````````````````````````````````````````````````````````````````` 
      ``````````````````````````Copyright``yutiansut``2018``````QUANTITATIVE FINANCIAL FRAMEWORK````````````````````````````` 
      ``````````````````````````````````````````````````````````````````````````````````````````````````````````````````````` 
     ```````````````````````````````````````````````````````````````````````````````````````````````````````````````````````` 
     ```````````````````````````````````````````````````````````````````````````````````````````````````````````````````````` 
     
    

# ??????????????????????????? ??????/??????/?????????????????????

??????????????????????????????????????????????????????



```python
 # ???????????????account
Account=QA.QA_Account()

# ????????????????????????
B = QA.QA_BacktestBroker()

```

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:2: DeprecationWarning: QUANTAXIS 1.0.47 has changed the init_assets ==> init_cash, please pay attention to this change if you using init_cash to initial an account class,                
      
    

?????????????????????,???????????? 000001


```python
# ????????????'000001'

Order=Account.send_order(code='000001',
                        price=11,
                        money=Account.cash_available,
                        time='2018-05-09',
                        towards=QA.ORDER_DIRECTION.BUY,
                        order_model=QA.ORDER_MODEL.MARKET,
                        amount_model=QA.AMOUNT_MODEL.BY_MONEY
                        )



```


```python

## ??????order???????????????
print('ORDER???????????????: {}'.format((Order.amount*Order.price)*(1+Account.commission_coeff)))
```

    ORDER???????????????: 999049.7000000001
    


```python
# ??????????????????
print('?????????????????? :{}'.format(Account.cash_available))
```

    ?????????????????? :0
    


```python
Account.hold
```




    Series([], Name: amount, dtype: float64)




```python
Account.init_hold
```




    Series([], Name: amount, dtype: float64)




```python
Account.hold_available
```




    Series([], Name: amount, dtype: float64)



???????????????cash????????????,??????????????????????????????(????????? ???????????????)

?????????????????????,?????????????????????


```python
rec_mes=B.receive_order(QA.QA_Event(order=Order))
print(rec_mes)
```

    {'header': {'source': 'market', 'status': 200, 'code': '000001', 'session': {'user': None, 'strategy': None, 'account': 'Acc_8wab3yjO'}, 'order_id': 'Order_WH05kvAI', 'trade_id': 'Trade_NQG2ZDtF'}, 'body': {'order': {'price': 10.96, 'code': '000001', 'amount': 90800, 'date': '2018-05-09', 'datetime': '2018-05-09 09:30:00', 'towards': 1}, 'fee': {'commission': 248.67849999999999, 'tax': 0}}}
    

?????????????????????????????????????????????,????????????


```python
Account.receive_deal(rec_mes)
```




    {'source': 'account',
     'account_cookie': 'Acc_8wab3yjO',
     'portfolio_cookie': None,
     'user_cookie': None,
     'broker': 'backtest',
     'market_type': 'stock_cn',
     'strategy_name': None,
     'current_time': None,
     'allow_sellopen': False,
     'allow_t0': False,
     'margin_level': False,
     'init_assets': {'cash': 1000000, 'hold': {}},
     'commission_coeff': 0.00025,
     'tax_coeff': 0.0015,
     'cash': [1000000, 4583.321499999845],
     'history': [['2018-05-09 09:30:00',
       '000001',
       10.96,
       90800.0,
       'Order_WH05kvAI',
       'Trade_NQG2ZDtF',
       'Acc_8wab3yjO',
       248.67849999999999,
       0.0]],
     'trade_index': ['2018-05-09 09:30:00'],
     'running_time': datetime.datetime(2018, 6, 11, 22, 2, 41, 739082),
     'quantaxis_version': '1.0.46',
     'running_environment': 'backtest'}



?????????????????????????????????????????????(?????????????????????????????????????????? ????????????000001??????,?????????????????????)


```python
print('????????????????????? {}'.format(Account.cash_available))
```

    ????????????????????? 4583.321499999845
    


```python
Account.hold
```




    code
    000001    90800.0
    Name: amount, dtype: float64




```python
Account.hold_available
```




    code
    000001    90800.0
    Name: amount, dtype: float64




```python
Account.init_hold.index_name='code'
```


```python
import pandas as pd
```


```python
pd.concat([Account.hold_available,Account.init_hold])
```




    code
    000001    90800.0
    Name: amount, dtype: float64




```python
+Account.hold_available
```




    code
    000001    90800.0
    Name: amount, dtype: float64



??????????????? ??????????????????????????????,??????????????????950.2999999999302???,??????????????????,???????????????3339.9289999998837???,????????????????????????????????????????????????(QA.ORDER_MODEL.MARKET),????????????????????????10.96???

???????????? ??????????????????90800??? 000001


```python
Account.hold
```




    code
    000001    90800.0
    Name: amount, dtype: float64



?????????????????????????????????


```python
Account.cash
```




    [1000000, 4583.321499999845]



?????????t+1???A?????????,????????????????????????0


```python
Account.sell_available
```




    Series([], Name: amount, dtype: float64)



# ????????????


```python
Account.settle()
```

# ?????????


```python
Account.cash
```




    [1000000, 4583.321499999845]




```python
Account.cash_available
```




    4583.321499999845




```python
Account.sell_available
```




    code
    000001    90800.0
    Name: amount, dtype: float64




```python
Account.hold
```




    code
    000001    90800.0
    Name: amount, dtype: float64



# ??????????????????

??????????????????: 000001 90800???


```python
holdnum=Account.sell_available.get('000001',0)
```


```python
holdnum
```




    90800.0



?????????????????????,?????????????????????


```python
Order=Account.send_order(code='000001',
                        price=11,
                        amount=holdnum,
                        time='2018-05-10',
                        towards=QA.ORDER_DIRECTION.SELL,
                        order_model=QA.ORDER_MODEL.MARKET,
                        amount_model=QA.AMOUNT_MODEL.BY_AMOUNT
                        )

```


```python
Order
```




    < QA_Order datetime:2018-05-10 09:31:00 code:000001 amount:90800.0 price:11 towards:-1 btype:stock_cn order_id:Order_3Eo6b9wO account:Acc_8wab3yjO status:300 >




```python
Account.cash_available  # ???????????????????????????????????? ??????????????????
```




    4583.321499999845




```python
rec_mes=B.receive_order(QA.QA_Event(order=Order))
print(rec_mes)
```

    {'header': {'source': 'market', 'status': 200, 'code': '000001', 'session': {'user': None, 'strategy': None, 'account': 'Acc_8wab3yjO'}, 'order_id': 'Order_3Eo6b9wO', 'trade_id': 'Trade_6ns5CruI'}, 'body': {'order': {'price': 11.0, 'code': '000001', 'amount': 90800.0, 'date': '2018-05-10', 'datetime': '2018-05-10 09:30:00', 'towards': -1}, 'fee': {'commission': 249.7, 'tax': 1498.2}}}
    


```python
Account.receive_deal(rec_mes)
```




    {'source': 'account',
     'account_cookie': 'Acc_8wab3yjO',
     'portfolio_cookie': None,
     'user_cookie': None,
     'broker': 'backtest',
     'market_type': 'stock_cn',
     'strategy_name': None,
     'current_time': None,
     'allow_sellopen': False,
     'allow_t0': False,
     'margin_level': False,
     'init_assets': {'cash': 1000000, 'hold': {}},
     'commission_coeff': 0.00025,
     'tax_coeff': 0.0015,
     'cash': [1000000, 4583.321499999845, 1001635.4214999999],
     'history': [['2018-05-09 09:30:00',
       '000001',
       10.96,
       90800.0,
       'Order_WH05kvAI',
       'Trade_NQG2ZDtF',
       'Acc_8wab3yjO',
       248.67849999999999,
       0.0],
      ['2018-05-10 09:30:00',
       '000001',
       11.0,
       -90800.0,
       'Order_3Eo6b9wO',
       'Trade_6ns5CruI',
       'Acc_8wab3yjO',
       249.7,
       1498.2]],
     'trade_index': ['2018-05-09 09:30:00', '2018-05-10 09:30:00'],
     'running_time': datetime.datetime(2018, 6, 11, 22, 2, 41, 964236),
     'quantaxis_version': '1.0.46',
     'running_environment': 'backtest'}




```python
Account.cash_available # ????????????????????? cash_available????????????
```




    1001635.4214999999




```python
Account.history_table
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>datetime</th>
      <th>code</th>
      <th>price</th>
      <th>amount</th>
      <th>order_id</th>
      <th>trade_id</th>
      <th>account_cookie</th>
      <th>commission</th>
      <th>tax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-05-09 09:30:00</td>
      <td>000001</td>
      <td>10.96</td>
      <td>90800.0</td>
      <td>Order_WH05kvAI</td>
      <td>Trade_NQG2ZDtF</td>
      <td>Acc_8wab3yjO</td>
      <td>248.6785</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-05-10 09:30:00</td>
      <td>000001</td>
      <td>11.00</td>
      <td>-90800.0</td>
      <td>Order_3Eo6b9wO</td>
      <td>Trade_6ns5CruI</td>
      <td>Acc_8wab3yjO</td>
      <td>249.7000</td>
      <td>1498.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
Account.orders
```




    < QA_OrderQueue AMOUNT 2 WAITING TRADE 2 >




```python
Account.orders.queue_df.query('date=="2018-05-09"')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>account_cookie</th>
      <th>amount</th>
      <th>amount_model</th>
      <th>callback</th>
      <th>code</th>
      <th>commission_coeff</th>
      <th>date</th>
      <th>datetime</th>
      <th>frequence</th>
      <th>market_type</th>
      <th>...</th>
      <th>price</th>
      <th>sending_time</th>
      <th>status</th>
      <th>strategy</th>
      <th>tax_coeff</th>
      <th>towards</th>
      <th>trade_id</th>
      <th>transact_time</th>
      <th>type</th>
      <th>user</th>
    </tr>
    <tr>
      <th>order_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Order_WH05kvAI</th>
      <td>Acc_8wab3yjO</td>
      <td>90800.0</td>
      <td>by_money</td>
      <td>&lt;bound method QA_Account.receive_deal of &lt; QA_...</td>
      <td>000001</td>
      <td>0.00025</td>
      <td>2018-05-09</td>
      <td>2018-05-09 09:31:00</td>
      <td>day</td>
      <td>stock_cn</td>
      <td>...</td>
      <td>11</td>
      <td>2018-05-09 09:31:00</td>
      <td>300</td>
      <td>None</td>
      <td>0.0015</td>
      <td>1</td>
      <td>None</td>
      <td>None</td>
      <td>stock_cn</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>1 rows ?? 23 columns</p>
</div>




```python
# ??????T0??????
```


```python
 # ???????????????account
AccountT0=QA.QA_Account(running_environment=QA.RUNNING_ENVIRONMENT.TZERO,init_hold={'000001':10000},init_cash=200000)

# ????????????????????????
B = QA.QA_BacktestBroker()
```

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:2: DeprecationWarning: QUANTAXIS 1.0.47 has changed the init_assets ==> init_cash, please pay attention to this change if you using init_cash to initial an account class,                
      
    


```python
AccountT0.init_assets
```




    {'cash': 200000, 'hold': {'000001': 10000}}




```python
AccountT0.init_hold
```




    code
    000001    10000
    Name: amount, dtype: int64




```python
AccountT0.hold_available
```




    Series([], Name: amount, dtype: float64)




```python
AccountT0.sell_available
```




    code
    000001    10000
    Name: amount, dtype: int64




```python
Order=AccountT0.send_order(code='000001',
                        price=11,
                        amount=AccountT0.sell_available.get('000001',0),
                        time='2018-05-10',
                        towards=QA.ORDER_DIRECTION.SELL,
                        order_model=QA.ORDER_MODEL.MARKET,
                        amount_model=QA.AMOUNT_MODEL.BY_AMOUNT
                        )

```


```python
Order
```




    < QA_Order datetime:2018-05-10 09:31:00 code:000001 amount:10000 price:11 towards:-1 btype:stock_cn order_id:Order_CoB3PaZW account:Acc_Qkl8bKjf status:300 >




```python
rec_mes=B.receive_order(QA.QA_Event(order=Order))
print(rec_mes)
```

    {'header': {'source': 'market', 'status': 200, 'code': '000001', 'session': {'user': None, 'strategy': None, 'account': 'Acc_Qkl8bKjf'}, 'order_id': 'Order_CoB3PaZW', 'trade_id': 'Trade_JyRSPIm2'}, 'body': {'order': {'price': 11.0, 'code': '000001', 'amount': 10000, 'date': '2018-05-10', 'datetime': '2018-05-10 09:30:00', 'towards': -1}, 'fee': {'commission': 27.5, 'tax': 165.0}}}
    


```python
AccountT0.receive_deal(rec_mes)
```




    {'source': 'account',
     'account_cookie': 'Acc_Qkl8bKjf',
     'portfolio_cookie': None,
     'user_cookie': None,
     'broker': 'backtest',
     'market_type': 'stock_cn',
     'strategy_name': None,
     'current_time': None,
     'allow_sellopen': False,
     'allow_t0': False,
     'margin_level': False,
     'init_assets': {'cash': 200000, 'hold': {'000001': 10000}},
     'commission_coeff': 0.00025,
     'tax_coeff': 0.0015,
     'cash': [200000, 309807.5],
     'history': [['2018-05-10 09:30:00',
       '000001',
       11.0,
       -10000.0,
       'Order_CoB3PaZW',
       'Trade_JyRSPIm2',
       'Acc_Qkl8bKjf',
       27.5,
       165.0]],
     'trade_index': ['2018-05-10 09:30:00'],
     'running_time': datetime.datetime(2018, 6, 11, 22, 2, 42, 131389),
     'quantaxis_version': '1.0.46',
     'running_environment': 't0'}




```python
AccountT0.sell_available
```




    code
    000001    0
    Name: amount, dtype: int64




```python
AccountT0.hold
```




    code
    000001    0.0
    Name: amount, dtype: float64




```python
AccountT0.hold_available.sum()
```




    -10000.0




```python
AccountT0.running_time
```




    datetime.datetime(2018, 6, 11, 22, 2, 42, 49052)




```python
AccountT0.datetime
```




    '2018-05-10 09:30:00'




```python
AccountT0.close_positions_order
```




    [< QA_Order datetime:2018-05-10 15:00:00 code:000001 amount:10000 price:0 towards:1 btype:stock_cn order_id:Order_9VYZWkov account:Acc_Qkl8bKjf status:300 >]




```python
for item in AccountT0.close_positions_order:
    rec_mes=B.receive_order(QA.QA_Event(order=item))
    print(rec_mes)
    AccountT0.receive_deal(rec_mes)
```

    {'header': {'source': 'market', 'status': 200, 'code': '000001', 'session': {'user': None, 'strategy': None, 'account': 'Acc_Qkl8bKjf'}, 'order_id': 'Order_rWlXR8ZS', 'trade_id': 'Trade_7IBiPSCN'}, 'body': {'order': {'price': 11.01, 'code': '000001', 'amount': 10000, 'date': '2018-05-10', 'datetime': '2018-05-10 15:00:00', 'towards': 1}, 'fee': {'commission': 27.525000000000002, 'tax': 0}}}
    


```python
AccountT0.cash
```




    [200000, 309807.5, 199679.975]




```python
AccountT0.sell_available
```




    code
    000001    0
    Name: amount, dtype: int64




```python
AccountT0.hold_available
```




    code
    000001    0.0
    Name: amount, dtype: float64




```python
AccountT0.hold
```




    code
    000001    10000.0
    Name: amount, dtype: float64




```python
AccountT0.settle()
```


```python
AccountT0.daily_hold
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>code</th>
      <th>000001</th>
    </tr>
    <tr>
      <th>date</th>
      <th>account_cookie</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018-05-10</th>
      <th>Acc_Qkl8bKjf</th>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
AccountT0.daily_cash
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>cash</th>
      <th>datetime</th>
      <th>date</th>
      <th>account_cookie</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th>account_cookie</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018-05-10 15:00:00</th>
      <th>Acc_Qkl8bKjf</th>
      <td>199680</td>
      <td>2018-05-10 15:00:00</td>
      <td>2018-05-10</td>
      <td>Acc_Qkl8bKjf</td>
    </tr>
  </tbody>
</table>
</div>




```python
AccountT0.hold_table()
```




    code
    000001    10000.0
    Name: amount, dtype: float64




```python
AccountT0.hold_price()
```




    code
    000001    (0, 0)
    dtype: object




```python
AccountT0.datetime
```




    '2018-05-11 09:30:00'




```python
AccountT0.sell_available
```




    code
    000001    10000.0
    Name: amount, dtype: float64


