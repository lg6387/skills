# Mt_Python - Zh

**Pages:** 33

---

## account_info

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5accountinfo_py

**Contents:**
- account_info

以指定元组结构(namedtuple)的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数返回在一次调用中使用AccountInfoInteger、 AccountInfoDouble和AccountInfoString可获得的所有数据。

import MetaTrader5 as mt5 import pandas as pd # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 连接到指定密码和服务器的交易账户 authorized=mt5.login(25115284, password="gqz0343lbdm") if authorized: account_info=mt5.account_info() if account_info!=None: # display trading account data 'as is' print(account_info) # display trading account data in the form of a dictionary print("Show account_info()._asdict():") account_info_dict = mt5.account_info()._asdict() for prop in account_info_dict: print(" {}={}".format(prop, account_info_dict[prop])) print() # 将词典转换为DataFrame和print df=pd.DataFrame(list(account_info_dict.items()),columns=['property','value']) print("account_info() as dataframe:") print(df) 其他： print("failed to connect to trade account 25115284 with password=gqz0343lbdm, error code =",mt5.last_error()) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 AccountInfo(login=25115284, trade_mode=0, leverage=100, limit_orders=200, margin_so_mode=0, .... Show account_info()._asdict(): login=25115284 trade_mode=0 leverage=100 limit_orders=200 margin_so_mode=0 trade_allowed=True trade_expert=True margin_mode=2 currency_digits=2 fifo_close=False balance=99511.4 credit=0.0 profit=41.82 equity=99553.22 margin=98.18 margin_free=99455.04 margin_level=101398.67590140559 margin_so_call=50.0 margin_so_so=30.0 margin_initial=0.0 margin_maintenance=0.0 assets=0.0 liabilities=0.0 commission_blocked=0.0 server=MetaQuotes-Demo currency=USD company=MetaQuotes Software Corp. account_info() as dataframe property value 0 login 25115284 1 trade_mode 0 2 leverage 100 3 limit_orders 200 4 margin_so_mode 0 5 trade_allowed True 6 trade_expert True 7 margin_mode 2 8 currency_digits 2 9 fifo_close False 10 balance 99588.3 11 credit 0 12 profit -45.13 13 equity 99543.2 14 margin 54.37 15 margin_free 99488.8 16 margin_level 183085 17 margin_so_call 50 18 margin_so_so 30 19 margin_initial 0 20 margin_maintenance 0 21 assets 0 22 liabilities 0 23 commission_blocked 0 24 name James Smith 25 server MetaQuotes-Demo 26 currency USD 27 company MetaQuotes Software Corp.

initialize、shutdown、login

---

## copy_rates_from

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5copyratesfrom_py

**Contents:**
- copy_rates_from

从指定日期开始从MetaTrader 5程序端获取柱形图。

copy_rates_from( symbol, // 交易品种名称 timeframe, // 时间周期 date_from, // 初始柱形图开盘日期 count // 柱形图数 )

[in] 交易品种名称，例如"EURUSD"。所需的未命名参数。

[in] 请求柱形图的时间周期。根据TIMEFRAME枚举的值设置。所需的未命名参数。

[in] 请求样本中的第一个柱形图的开盘日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

[in] 将要接收的柱形图数。所需的未命名参数。

返回numpy数据柱形图（包含以下指定列：时间、开盘价、最高价、最低价、收盘价、tick_volume、点差和real_volume）。错误情况下返回None。可使用last_error()获取错误信息。

参见CopyRates()函数获取更多信息。

只有日期小于（早于）或等于指定日期的数据。这意味着，任何柱的开盘时间始终少于或等于指定的值。

MetaTrader 5程序端仅在用户可用的图表历史记录中提供柱形图。可供用户使用的柱形图数量设置在“图表中最大数量(Max.)柱形图”参数中。

当创建'datetime'对象时，Python使用本地时区，而MetaTrader 5以UTC时区保存报价和柱形图开盘时间（没有时移）。 因此，'datetime'应在UTC时间内创建，用于执行使用时间的函数。从MetaTrader 5程序端接收到的数据使用UTC时间。

TIMEFRAME 是一个包含可能图表周期值的枚举

from datetime import datetime import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 导入'pandas'模块，用于以表格形式显示获得的数据 import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # 导入用于处理时区的pytz模块 import pytz # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 将时区设置为UTC timezone = pytz.timezone("Etc/UTC") # 以UTC时区创建'datetime'对象，以避免实现本地时区偏移 utc_from = datetime(2020, 1, 10, tzinfo=timezone) # 在UTC时区，获取01.10.2020开始的10个EURUSD H4柱形图 rates = mt5.copy_rates_from("EURUSD", mt5.TIMEFRAME_H4, utc_from, 10) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() # 在新行显示所获得数据的每个元素 print("Display obtained data 'as is'") for rate in rates: print(rate) # 从所获得的数据创建DataFrame rates_frame = pd.DataFrame(rates) # 将时间（以秒为单位）转换为日期时间格式 rates_frame['time']=pd.to_datetime(rates_frame['time'], unit='s') # 显示数据 print("\nDisplay dataframe with data") print(rates_frame) 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 将获得的数据显示为“保持原来状态” (1578484800, 1.11382, 1.11385, 1.1111, 1.11199, 9354, 1, 0) (1578499200, 1.11199, 1.11308, 1.11086, 1.11179, 10641, 1, 0) (1578513600, 1.11178, 1.11178, 1.11016, 1.11053, 4806, 1, 0) (1578528000, 1.11053, 1.11193, 1.11033, 1.11173, 3480, 1, 0) (1578542400, 1.11173, 1.11189, 1.11126, 1.11182, 2236, 1, 0) (1578556800, 1.11181, 1.11203, 1.10983, 1.10993, 7984, 1, 0) (1578571200, 1.10994, 1.11173, 1.10965, 1.11148, 7406, 1, 0) (1578585600, 1.11149, 1.11149, 1.10923, 1.11046, 7468, 1, 0) (1578600000, 1.11046, 1.11097, 1.11033, 1.11051, 3450, 1, 0) (1578614400, 1.11051, 1.11093, 1.11017, 1.11041, 2448, 1, 0) 显示带有数据的数据框 time open high low close tick_volume spread real_volume 0 2020-01-08 12:00:00 1.11382 1.11385 1.11110 1.11199 9354 1 0 1 2020-01-08 16:00:00 1.11199 1.11308 1.11086 1.11179 10641 1 0 2 2020-01-08 20:00:00 1.11178 1.11178 1.11016 1.11053 4806 1 0 3 2020-01-09 00:00:00 1.11053 1.11193 1.11033 1.11173 3480 1 0 4 2020-01-09 04:00:00 1.11173 1.11189 1.11126 1.11182 2236 1 0 5 2020-01-09 08:00:00 1.11181 1.11203 1.10983 1.10993 7984 1 0 6 2020-01-09 12:00:00 1.10994 1.11173 1.10965 1.11148 7406 1 0 7 2020-01-09 16:00:00 1.11149 1.11149 1.10923 1.11046 7468 1 0 8 2020-01-09 20:00:00 1.11046 1.11097 1.11033 1.11051 3450 1 0 9 2020-01-10 00:00:00 1.11051 1.11093 1.11017 1.11041 2448 1 0

CopyRates、copy_rates_from_pos、 copy_rates_range、copy_ticks_from、 copy_ticks_range

---

## copy_rates_from_pos

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5copyratesfrompos_py

**Contents:**
- copy_rates_from_pos

从指定索引开始从MetaTrader 5程序端获取柱形图。

copy_rates_from_pos( symbol, // 交易品种名称 timeframe, // 时间周期 start_pos, //初始柱形图索引 count // 柱形图数 )

[in] 交易品种名称，例如"EURUSD"。所需的未命名参数。

[in] 请求柱形图的时间周期。根据TIMEFRAME枚举的值设置。所需的未命名参数。

[in] 请求数据的初始柱形图索引。柱形图变化从现在到过去。因此，零柱形图意味着当前柱形图。所需的未命名参数。

[in] 将要接收的柱形图数。所需的未命名参数。

返回numpy数据柱形图（包含以下指定列：时间、开盘价、最高价、最低价、收盘价、tick_volume、点差和real_volume）。错误情况下返回None。可使用last_error()获取错误信息。

参见CopyRates()函数获取更多信息。

MetaTrader 5程序端仅在用户可用的图表历史记录中提供柱形图。可供用户使用的柱形图数量设置在“图表中最大数量(Max.)柱形图”参数中。

from datetime import datetime import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 导入'pandas'模块，用于以表格形式显示获得的数据 import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 从当日获取10个GBPUSD D1柱形图 rates = mt5.copy_rates_from_pos("GBPUSD", mt5.TIMEFRAME_D1, 0, 10) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() # 在新行显示所获得数据的每个元素 print("Display obtained data 'as is'") for rate in rates: print(rate) # 从所获得的数据创建DataFrame rates_frame = pd.DataFrame(rates) # 将时间（以秒为单位）转换为日期时间格式 rates_frame['time']=pd.to_datetime(rates_frame['time'], unit='s') # 显示数据 print("\nDisplay dataframe with data") print(rates_frame) 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 将获得的数据显示为“保持原来状态” (1581552000, 1.29568, 1.30692, 1.29441, 1.30412, 68228, 0, 0) (1581638400, 1.30385, 1.30631, 1.3001, 1.30471, 56498, 0, 0) (1581897600, 1.30324, 1.30536, 1.29975, 1.30039, 49400, 0, 0) (1581984000, 1.30039, 1.30486, 1.29705, 1.29952, 62288, 0, 0) (1582070400, 1.29952, 1.3023, 1.29075, 1.29187, 57909, 0, 0) (1582156800, 1.29186, 1.29281, 1.28489, 1.28792, 61033, 0, 0) (1582243200, 1.28802, 1.29805, 1.28746, 1.29566, 66386, 0, 0) (1582502400, 1.29426, 1.29547, 1.28865, 1.29283, 66933, 0, 0) (1582588800, 1.2929, 1.30178, 1.29142, 1.30037, 80121, 0, 0) (1582675200, 1.30036, 1.30078, 1.29136, 1.29374, 49286, 0, 0) 显示带有数据的数据框 time open high low close tick_volume spread real_volume 0 2020-02-13 1.29568 1.30692 1.29441 1.30412 68228 0 0 1 2020-02-14 1.30385 1.30631 1.30010 1.30471 56498 0 0 2 2020-02-17 1.30324 1.30536 1.29975 1.30039 49400 0 0 3 2020-02-18 1.30039 1.30486 1.29705 1.29952 62288 0 0 4 2020-02-19 1.29952 1.30230 1.29075 1.29187 57909 0 0 5 2020-02-20 1.29186 1.29281 1.28489 1.28792 61033 0 0 6 2020-02-21 1.28802 1.29805 1.28746 1.29566 66386 0 0 7 2020-02-24 1.29426 1.29547 1.28865 1.29283 66933 0 0 8 2020-02-25 1.29290 1.30178 1.29142 1.30037 80121 0 0 9 2020-02-26 1.30036 1.30078 1.29136 1.29374 49286 0 0

CopyRates、copy_rates_from、 copy_rates_range、copy_ticks_from、 copy_ticks_range

---

## copy_rates_range

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5copyratesrange_py

**Contents:**
- copy_rates_range

从MetaTrader 5程序端获取指定日期范围内的柱形图。

copy_rates_range( symbol, // 交易品种名称 timeframe, // 时间周期 date_from, // 请求柱形图的开始日期 date_to // 请求柱形图的结束日期 )

[in] 交易品种名称，例如"EURUSD"。所需的未命名参数。

[in] 请求柱形图的时间周期。根据TIMEFRAME枚举的值设置。所需的未命名参数。

[in] 请求柱形图的开始日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。包括开盘时间的柱形图 >= 返回date_from。所需的未命名参数。

[in] 请求柱形图的结束日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。包括开盘时间的柱形图 <= 返回date_to。所需的未命名参数。

返回numpy数据柱形图（包含以下指定列：时间、开盘价、最高价、最低价、收盘价、tick_volume、点差和real_volume）。错误情况下返回None。可使用last_error()获取错误信息。

参见CopyRates()函数获取更多信息。

MetaTrader 5程序端仅在用户可用的图表历史记录中提供柱形图。可供用户使用的柱形图数量设置在“图表中最大数量(Max.)柱形图”参数中。

当创建'datetime'对象时，Python使用本地时区，而MetaTrader 5以UTC时区保存报价和柱形图开盘时间（没有时移）。 因此，'datetime'应在UTC时间内创建，用于执行使用时间的函数。从MetaTrader 5程序端接收到的数据使用UTC时间。

from datetime import datetime import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # import the 'pandas' module for displaying data obtained in the tabular form import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # import pytz module for working with time zone import pytz # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # set time zone to UTC timezone = pytz.timezone("Etc/UTC") # create 'datetime' objects in UTC time zone to avoid the implementation of a local time zone offset utc_from = datetime(2020, 1, 10, tzinfo=timezone) utc_to = datetime(2020, 1, 11, hour = 13, tzinfo=timezone) # get bars from USDJPY M5 within the interval of 2020.01.10 00:00 - 2020.01.11 13:00 in UTC time zone rates = mt5.copy_rates_range("USDJPY", mt5.TIMEFRAME_M5, utc_from, utc_to) # shut down connection to the MetaTrader 5 terminal mt5.shutdown() # display each element of obtained data in a new line print("Display obtained data 'as is'") counter=0 for rate in rates: counter+=1 if counter<=10: print(rate) # create DataFrame out of the obtained data rates_frame = pd.DataFrame(rates) # convert time in seconds into the 'datetime' format rates_frame['time']=pd.to_datetime(rates_frame['time'], unit='s') # display data print("\nDisplay dataframe with data") print(rates_frame.head(10)) 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 将获得的数据显示为“保持原来状态” (1578614400, 109.513, 109.527, 109.505, 109.521, 43, 2, 0) (1578614700, 109.521, 109.549, 109.518, 109.543, 215, 8, 0) (1578615000, 109.543, 109.543, 109.466, 109.505, 98, 10, 0) (1578615300, 109.504, 109.534, 109.502, 109.517, 155, 8, 0) (1578615600, 109.517, 109.539, 109.513, 109.527, 71, 4, 0) (1578615900, 109.526, 109.537, 109.484, 109.52, 106, 9, 0) (1578616200, 109.52, 109.524, 109.508, 109.51, 205, 7, 0) (1578616500, 109.51, 109.51, 109.491, 109.496, 44, 8, 0) (1578616800, 109.496, 109.509, 109.487, 109.5, 85, 5, 0) (1578617100, 109.5, 109.504, 109.487, 109.489, 82, 7, 0) 显示带有数据的数据框 time open high low close tick_volume spread real_volume 0 2020-01-10 00:00:00 109.513 109.527 109.505 109.521 43 2 0 1 2020-01-10 00:05:00 109.521 109.549 109.518 109.543 215 8 0 2 2020-01-10 00:10:00 109.543 109.543 109.466 109.505 98 10 0 3 2020-01-10 00:15:00 109.504 109.534 109.502 109.517 155 8 0 4 2020-01-10 00:20:00 109.517 109.539 109.513 109.527 71 4 0 5 2020-01-10 00:25:00 109.526 109.537 109.484 109.520 106 9 0 6 2020-01-10 00:30:00 109.520 109.524 109.508 109.510 205 7 0 7 2020-01-10 00:35:00 109.510 109.510 109.491 109.496 44 8 0 8 2020-01-10 00:40:00 109.496 109.509 109.487 109.500 85 5 0 9 2020-01-10 00:45:00 109.500 109.504 109.487 109.489 82 7 0

CopyRates、copy_rates_from、 copy_rates_range、copy_ticks_from、 copy_ticks_range

---

## copy_ticks_from

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5copyticksfrom_py

**Contents:**
- copy_ticks_from

从指定日期开始从MetaTrader 5程序端获取报价。

copy_ticks_from( symbol, // 交易品种名称 date_from, // 请求报价的日期 count, // 请求的报价数 flags // 定义请求报价类型的标识组合 )

[in] 交易品种名称，例如"EURUSD"。所需的未命名参数。

[in] 请求报价的开始日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

[in] 将要接收的报价数。所需的未命名参数。

[in] 确定请求报价类型的标记。COPY_TICKS_INFO – 卖价和/或买价变化的报价， COPY_TICKS_TRADE – 最后价和交易量变化的报价， COPY_TICKS_ALL – 全部报价。标识值在COPY_TICKS枚举中描述。所需的未命名参数。

返回numpy数据报价（包含以下指定列：时间、卖价、买价、最后价、标识）。'flags'值可以是TICK_FLAG枚举中的标识组合。错误情况下返回None。可使用last_error()获取错误信息。

当创建'datetime'对象时，Python使用本地时区，而MetaTrader 5以UTC时区保存报价和柱形图开盘时间（没有时移）。 因此，'datetime'应在UTC时间内创建，用于执行使用时间的函数。从MetaTrader 5程序端接收到的数据使用UTC时间。

COPY_TICKS定义了可以使用copy_ticks_from()和copy_ticks_range()函数请求的报价类型。

包含卖价（Bid）和/或买价（Ask）价格变化的报价

包含最后价（Last）和/或交易量（Volume）价格变化的报价

TICK_FLAG 定义可能的报价标识。这些标识用于描述通过copy_ticks_from()和copy_ticks_range()函数获得的报价。

from datetime import datetime import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 导入'pandas'模块，用于以表格形式显示获得的数据 import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # 导入用于处理时区的pytz模块 import pytz # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # set time zone to UTC timezone = pytz.timezone("Etc/UTC") # create 'datetime' object in UTC time zone to avoid the implementation of a local time zone offset utc_from = datetime(2020, 1, 10, tzinfo=timezone) # request 100 000 EURUSD ticks starting from 10.01.2019 in UTC time zone ticks = mt5.copy_ticks_from("EURUSD", utc_from, 100000, mt5.COPY_TICKS_ALL) print("Ticks received:",len(ticks)) # shut down connection to the MetaTrader 5 terminal mt5.shutdown() # display data on each tick on a new line print("Display obtained ticks 'as is'") count = 0 for tick in ticks: count+=1 print(tick) if count >= 10: break # create DataFrame out of the obtained data ticks_frame = pd.DataFrame(ticks) # 将时间（以秒为单位）转换为日期时间格式 ticks_frame['time']=pd.to_datetime(ticks_frame['time'], unit='s') # display data print("\nDisplay dataframe with ticks") print(ticks_frame.head(10)) 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 已接收报价：100000 将获得的报价显示为“保持原来状态” (1578614400, 1.11051, 1.11069, 0., 0, 1578614400987, 134, 0.) (1578614402, 1.11049, 1.11067, 0., 0, 1578614402025, 134, 0.) (1578614404, 1.1105, 1.11066, 0., 0, 1578614404057, 134, 0.) (1578614404, 1.11049, 1.11067, 0., 0, 1578614404344, 134, 0.) (1578614412, 1.11052, 1.11064, 0., 0, 1578614412106, 134, 0.) (1578614418, 1.11039, 1.11051, 0., 0, 1578614418265, 134, 0.) (1578614418, 1.1104, 1.1105, 0., 0, 1578614418905, 134, 0.) (1578614419, 1.11039, 1.11051, 0., 0, 1578614419519, 134, 0.) (1578614456, 1.11037, 1.11065, 0., 0, 1578614456011, 134, 0.) (1578614456, 1.11039, 1.11051, 0., 0, 1578614456015, 134, 0.) 显示带有报价的数据框 time bid ask last volume time_msc flags volume_real 0 2020-01-10 00:00:00 1.11051 1.11069 0.0 0 1578614400987 134 0.0 1 2020-01-10 00:00:02 1.11049 1.11067 0.0 0 1578614402025 134 0.0 2 2020-01-10 00:00:04 1.11050 1.11066 0.0 0 1578614404057 134 0.0 3 2020-01-10 00:00:04 1.11049 1.11067 0.0 0 1578614404344 134 0.0 4 2020-01-10 00:00:12 1.11052 1.11064 0.0 0 1578614412106 134 0.0 5 2020-01-10 00:00:18 1.11039 1.11051 0.0 0 1578614418265 134 0.0 6 2020-01-10 00:00:18 1.11040 1.11050 0.0 0 1578614418905 134 0.0 7 2020-01-10 00:00:19 1.11039 1.11051 0.0 0 1578614419519 134 0.0 8 2020-01-10 00:00:56 1.11037 1.11065 0.0 0 1578614456011 134 0.0 9 2020-01-10 00:00:56 1.11039 1.11051 0.0 0 1578614456015 134 0.0

CopyRates、copy_rates_from_pos、 copy_rates_range、copy_ticks_from、 copy_ticks_range

---

## copy_ticks_range

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5copyticksrange_py

**Contents:**
- copy_ticks_range

从MetaTrader 5程序端获取指定日期范围内的报价。

copy_ticks_range( symbol, // 交易品种名称 date_from, // 请求报价的日期 date_to, // 请求报价的结束日期 flags // 定义请求报价类型的标识组合 )

[in] 交易品种名称，例如"EURUSD"。所需的未命名参数。

[in] 请求报价的开始日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

[in] 请求报价的结束日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

[in] 确定请求报价类型的标记。COPY_TICKS_INFO – 卖价和/或买价变化的报价， COPY_TICKS_TRADE – 最后价和交易量变化的报价， COPY_TICKS_ALL – 全部报价。标识值在COPY_TICKS枚举中描述。所需的未命名参数。

返回numpy数据报价（包含以下指定列：时间、卖价、买价、最后价、标识）。'flags'值可以是TICK_FLAG枚举中的标识组合。错误情况下返回None。可使用last_error()获取错误信息。

当创建'datetime'对象时，Python使用本地时区，而MetaTrader 5以UTC时区保存报价和柱形图开盘时间（没有时移）。 因此，'datetime'应在UTC时间内创建，用于执行使用时间的函数。从MetaTrader 5获得的数据有UTC时间，但Python在尝试打印时再次应用本地时移。 因此，所获得的数据也应进行校正，以便直观表示。

from datetime import datetime import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # import the 'pandas' module for displaying data obtained in the tabular form import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # import pytz module for working with time zone import pytz # establish connection to MetaTrader 5 terminal if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # set time zone to UTC timezone = pytz.timezone("Etc/UTC") # create 'datetime' objects in UTC time zone to avoid the implementation of a local time zone offset utc_from = datetime(2020, 1, 10, tzinfo=timezone) utc_to = datetime(2020, 1, 11, tzinfo=timezone) # request AUDUSD ticks within 11.01.2020 - 11.01.2020 ticks = mt5.copy_ticks_range("AUDUSD", utc_from, utc_to, mt5.COPY_TICKS_ALL) print("Ticks received:",len(ticks)) # shut down connection to the MetaTrader 5 terminal mt5.shutdown() # display data on each tick on a new line print("Display obtained ticks 'as is'") count = 0 for tick in ticks: count+=1 print(tick) if count >= 10: break # create DataFrame out of the obtained data ticks_frame = pd.DataFrame(ticks) # convert time in seconds into the datetime format ticks_frame['time']=pd.to_datetime(ticks_frame['time'], unit='s') # display data print("\nDisplay dataframe with ticks") print(ticks_frame.head(10)) 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 已接收报价：37008 将获得的报价显示为“保持原来状态” (1578614400, 0.68577, 0.68594, 0., 0, 1578614400820, 134, 0.) (1578614401, 0.68578, 0.68594, 0., 0, 1578614401128, 130, 0.) (1578614401, 0.68575, 0.68594, 0., 0, 1578614401128, 130, 0.) (1578614411, 0.68576, 0.68594, 0., 0, 1578614411388, 130, 0.) (1578614411, 0.68575, 0.68594, 0., 0, 1578614411560, 130, 0.) (1578614414, 0.68576, 0.68595, 0., 0, 1578614414973, 134, 0.) (1578614430, 0.68576, 0.68594, 0., 0, 1578614430188, 4, 0.) (1578614450, 0.68576, 0.68595, 0., 0, 1578614450408, 4, 0.) (1578614450, 0.68576, 0.68594, 0., 0, 1578614450519, 4, 0.) (1578614456, 0.68575, 0.68594, 0., 0, 1578614456363, 130, 0.) 显示带有报价的数据框 time bid ask last volume time_msc flags volume_real 0 2020-01-10 00:00:00 0.68577 0.68594 0.0 0 1578614400820 134 0.0 1 2020-01-10 00:00:01 0.68578 0.68594 0.0 0 1578614401128 130 0.0 2 2020-01-10 00:00:01 0.68575 0.68594 0.0 0 1578614401128 130 0.0 3 2020-01-10 00:00:11 0.68576 0.68594 0.0 0 1578614411388 130 0.0 4 2020-01-10 00:00:11 0.68575 0.68594 0.0 0 1578614411560 130 0.0 5 2020-01-10 00:00:14 0.68576 0.68595 0.0 0 1578614414973 134 0.0 6 2020-01-10 00:00:30 0.68576 0.68594 0.0 0 1578614430188 4 0.0 7 2020-01-10 00:00:50 0.68576 0.68595 0.0 0 1578614450408 4 0.0 8 2020-01-10 00:00:50 0.68576 0.68594 0.0 0 1578614450519 4 0.0 9 2020-01-10 00:00:56 0.68575 0.68594 0.0 0 1578614456363 130 0.0

CopyRates、copy_rates_from_pos、 copy_rates_range、copy_ticks_from、 copy_ticks_range

---

## history_deals_get

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5historydealsget_py

**Contents:**
- history_deals_get

获取可通过单号或持仓筛选的指定时间间隔内的交易历史中的交易。

调用指定时间间隔。返回指定时间间隔内的所有交易。

history_deals_get( date_from, // 请求交易的开始日期 date_to, // 请求交易的结束日期 group="GROUP" // 为交易品种选择交易的过滤器 )

调用指定订单单号。返回在DEAL_ORDER属性中具有指定订单单号的所有交易。

history_deals_get( ticket=TICKET //订单单号 )

调用指定持仓单号。返回在DEAL_POSITION_ID属性中具有指定持仓单号的所有交易。

history_deals_get( position=POSITION //持仓单号 )

[in] 请求订单的开始日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。首先指定所需的未命名参数。

[in] 请求订单的结束日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。其次指定所需的未命名参数。

[in] 用于跑了一组必要交易品种的过滤器。可选的命名参数。如果指定了组，则函数只返回满足交易品种名称指定条件的交易。

[in] 应接收所有交易的订单单号（存储在 DEAL_ORDER）。可选参数。如果没有指定，则不应用过滤器。

[in] 应接收所有交易的持仓单号（存储在DEAL_POSITION_ID）。可选参数。如果没有指定，则不应用过滤器。

以指定元组结构(namedtuple)的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数允许在一个类似于HistoryDealsTotal和HistoryDealSelect的串联调用中接收指定时间段内的所有历史交易。

group参数可以根据交易品种对交易进行分类。'*'可以用在字符串的开头和结尾。

group参数可以包含多个逗号分隔条件。条件可以使用'*'设置为掩码。逻辑否定字符'!'可以用来表示排除条件。所有条件依序应用，这意味着应该先指定包含到组中的条件，然后再指定排除条件。例如，group="*, !EUR"意味着应该先选择所有交易品种的交易，然后排除交易品种名称中包含"EUR"的交易。

import MetaTrader5 as mt5 from datetime import datetime import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) print() # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 获取历史中的交易数量 from_date=datetime(2020,1,1) to_date=datetime.now() # 获取指定时间间隔内且名称包含"GBP"的交易品种的交易 deals=mt5.history_deals_get(from_date, to_date, group="*GBP*") if deals==None: print("No deals with group=\"*USD*\", error code={}".format(mt5.last_error())) elif len(deals)> 0: print("history_deals_get({}, {}, group=\"*GBP*\")={}".format(from_date,to_date,len(deals))) # 获取名称中既不包含"EUR"也不包含"GBP"的交易品种的交易 deals = mt5.history_deals_get(from_date, to_date, group="*,!*EUR*,!*GBP*") if deals == None: print("No deals, error code={}".format(mt5.last_error())) elif len(deals) > 0: print("history_deals_get(from_date, to_date, group=\"*,!*EUR*,!*GBP*\") =", len(deals)) # display all obtained deals 'as is' for deal in deals: print(" ",deal) print() # display these deals as a table using pandas.DataFrame df=pd.DataFrame(list(deals),columns=deals[0]._asdict().keys()) df['time'] = pd.to_datetime(df['time'], unit='s') print(df) print("") # 获取#530218319持仓相关的所有交易 position_id=530218319 position_deals = mt5.history_deals_get(position=position_id) if position_deals == None: print("No deals with position #{}".format(position_id)) print("error code =", mt5.last_error()) elif len(position_deals) > 0: print("Deals with position id #{}: {}".format(position_id, len(position_deals))) # display these deals as a table using pandas.DataFrame df=pd.DataFrame(list(position_deals),columns=position_deals[0]._asdict().keys()) df['time'] = pd.to_datetime(df['time'], unit='s') print(df) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 history_deals_get(from_date, to_date, group="*GBP*") = 14 history_deals_get(from_date, to_date, group="*,!*EUR*,!*GBP*") = 7 TradeDeal(ticket=506966741, order=0, time=1582202125, time_msc=1582202125419, type=2, entry=0, magic=0, position_id=0, reason=0, volume=0.0, pri ... TradeDeal(ticket=507962919, order=530218319, time=1582303777, time_msc=1582303777582, type=0, entry=0, magic=0, position_id=530218319, reason=0, ... TradeDeal(ticket=513149059, order=535548147, time=1583176242, time_msc=1583176242265, type=1, entry=1, magic=0, position_id=530218319, reason=0, ... TradeDeal(ticket=516943494, order=539349382, time=1583510003, time_msc=1583510003895, type=1, entry=0, magic=0, position_id=539349382, reason=0, ... TradeDeal(ticket=516943915, order=539349802, time=1583510025, time_msc=1583510025054, type=0, entry=0, magic=0, position_id=539349802, reason=0, ... TradeDeal(ticket=517139682, order=539557870, time=1583520201, time_msc=1583520201227, type=0, entry=1, magic=0, position_id=539349382, reason=0, ... TradeDeal(ticket=517139716, order=539557909, time=1583520202, time_msc=1583520202971, type=1, entry=1, magic=0, position_id=539349802, reason=0, ... ticket order time time_msc type entry magic position_id reason volume price commission swap profit fee symbol comment external_id 0 506966741 0 2020-02-20 12:35:25 1582202125419 2 0 0 0 0 0.00 0.00000 0.0 0.0 100000.00 0.0 1 507962919 530218319 2020-02-21 16:49:37 1582303777582 0 0 0 530218319 0 0.01 0.97898 0.0 0.0 0.00 0.0 USDCHF 2 513149059 535548147 2020-03-02 19:10:42 1583176242265 1 1 0 530218319 0 0.01 0.95758 0.0 0.0 -22.35 0.0 USDCHF 3 516943494 539349382 2020-03-06 15:53:23 1583510003895 1 0 0 539349382 0 0.10 0.93475 0.0 0.0 0.00 0.0 USDCHF 4 516943915 539349802 2020-03-06 15:53:45 1583510025054 0 0 0 539349802 0 0.10 0.66336 0.0 0.0 0.00 0.0 AUDUSD 5 517139682 539557870 2020-03-06 18:43:21 1583520201227 0 1 0 539349382 0 0.10 0.93751 0.0 0.0 -29.44 0.0 USDCHF 6 517139716 539557909 2020-03-06 18:43:22 1583520202971 1 1 0 539349802 0 0.10 0.66327 0.0 0.0 -0.90 0.0 AUDUSD Deals with position id #530218319: 2 ticket order time time_msc type entry magic position_id reason volume price commission swap profit fee symbol comment external_id 0 507962919 530218319 2020-02-21 16:49:37 1582303777582 0 0 0 530218319 0 0.01 0.97898 0.0 0.0 0.00 0.0 USDCHF 1 513149059 535548147 2020-03-02 19:10:42 1583176242265 1 1 0 530218319 0 0.01 0.95758 0.0 0.0 -22.35 0.0 USDCHF

history_deals_total、history_orders_get

---

## history_deals_total

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5historydealstotal_py

**Contents:**
- history_deals_total

history_deals_total( date_from, // 请求交易的开始日期 date_to // 请求交易的结束日期 )

[in] 请求交易的开始日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

[in] 请求交易的结束日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

该函数类似于HistoryDealsTotal。

from datetime import datetime import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 获取历史中的交易数量 from_date=datetime(2020,1,1) to_date=datetime.now() deals=mt5.history_deals_total(from_date, to_date) if deals>0: print("Total deals=",deals) 其他： print("Deals not found in history") # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

history_deals_get、history_orders_total

---

## history_orders_get

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5historyordersget_py

**Contents:**
- history_orders_get

获取可通过单号或持仓筛选的交易历史中的订单。共有三个调用选项。

调用指定时间间隔。返回指定时间间隔内的所有订单。

history_orders_get( date_from, // 请求订单的开始日期 date_to, // 请求订单的结束日期 group="GROUP" // 根据交易品种选择订单的过滤器 )

调用指定订单单号。返回带有指定单号的订单。

history_orders_get( ticket=TICKET //订单单号 )

调用指定持仓单号。返回在ORDER_POSITION_ID属性中具有指定持仓单号的所有订单。

history_orders_get( position=POSITION //持仓单号 )

[in] 请求订单的开始日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。首先指定所需的未命名参数。

[in] 请求订单的结束日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。其次指定所需的未命名参数。

[in] 用于跑了一组必要交易品种的过滤器。可选的命名参数。如果指定了组，则函数只返回满足交易品种名称指定条件的订单。

[in] 应该接收的订单单号。可选参数。如果没有指定，则不应用过滤器。

[in] 应接收所有交易的持仓单号（存储在ORDER_POSITION_ID）。可选参数。如果没有指定，则不应用过滤器。

以指定元组结构(namedtuple)的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数允许在一个类似于HistoryOrdersTotal和HistoryOrderSelect的串联调用中接收指定时间段内的所有历史订单。

group参数可以包含多个逗号分隔条件。条件可以使用'*'设置为掩码。逻辑否定字符'!'可以用来表示排除条件。所有条件依序应用，这意味着应该先指定包含到组中的条件，然后再指定排除条件。例如，group="*, !EUR"意味着应该先选择所有交易品种的交易，然后排除交易品种名称中包含"EUR"的交易。

from datetime import datetime import MetaTrader5 as mt5 import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) print() # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 获取历史中的订单数量 from_date=datetime(2020,1,1) to_date=datetime.now() history_orders=mt5.history_orders_get(from_date, to_date, group="*GBP*") if history_orders==None: print("No history orders with group=\"*GBP*\", error code={}".format(mt5.last_error())) elif len(history_orders)>0: print("history_orders_get({}, {}, group=\"*GBP*\")={}".format(from_date,to_date,len(history_orders))) print() # 根据持仓单号显示所有历史订单 position_id=530218319 position_history_orders=mt5.history_orders_get(position=position_id) if position_history_orders==None: print("No orders with position #{}".format(position_id)) print("error code =",mt5.last_error()) elif len(position_history_orders)>0: print("Total history orders on position #{}: {}".format(position_id,len(position_history_orders))) # 显示带有指定持仓单号的所有历史订单 for position_order in position_history_orders: print(position_order) print() # display these orders as a table using pandas.DataFrame df=pd.DataFrame(list(position_history_orders),columns=position_history_orders[0]._asdict().keys()) df.drop(['time_expiration','type_time','state','position_by_id','reason','volume_current','price_stoplimit','sl','tp'], axis=1, inplace=True) df['time_setup'] = pd.to_datetime(df['time_setup'], unit='s') df['time_done'] = pd.to_datetime(df['time_done'], unit='s') print(df) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 history_orders_get(2020-01-01 00:00:00, 2020-03-25 17:17:32.058795, group="*GBP*")=14 Total history orders on position #530218319: 2 TradeOrder(ticket=530218319, time_setup=1582282114, time_setup_msc=1582282114681, time_done=1582303777, time_done_msc=1582303777582, time_expiration=0, ... TradeOrder(ticket=535548147, time_setup=1583176242, time_setup_msc=1583176242265, time_done=1583176242, time_done_msc=1583176242265, time_expiration=0, ... ticket time_setup time_setup_msc time_done time_done_msc type type_filling magic position_id volume_initial price_open price_current symbol comment external_id 0 530218319 2020-02-21 10:48:34 1582282114681 2020-02-21 16:49:37 1582303777582 2 2 0 530218319 0.01 0.97898 0.97863 USDCHF 1 535548147 2020-03-02 19:10:42 1583176242265 2020-03-02 19:10:42 1583176242265 1 0 0 530218319 0.01 0.95758 0.95758 USDCHF

history_deals_total、history_deals_get

---

## history_orders_total

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5historyorderstotal_py

**Contents:**
- history_orders_total

history_orders_total( date_from, // 请求订单的开始日期 date_to //请求订单的结束日期 )

[in] 请求订单的开始日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

[in] 请求订单的结束日期。通过'datetime'对象或根据1970.01.01以来过去的秒数设置。所需的未命名参数。

The function is similar to HistoryOrdersTotal.

from datetime import datetime import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 获取历史中的订单数量 from_date=datetime(2020,1,1) to_date=datetime.now() history_orders=mt5.history_orders_total(from_date, datetime.now()) if history_orders>0: print("Total history orders=",history_orders) 其他： print("Orders not found in history") # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

history_orders_get、history_deals_total

---

## initialize

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5initialize_py

**Contents:**
- initialize

建立与MetaTrader 5程序端的连接。共有三个调用选项。

调用指定要连接到MetaTrader 5程序端的路径。

initialize( path // 到MetaTrader 5程序端EXE文件的路径 )

initialize( path // 到MetaTrader 5程序端EXE文件的路径 timeout=TIMEOUT, // 超时 login=LOGIN, // 账号 password="PASSWORD", // 密码 server="SERVER" // 在程序端指定的服务器名称 )

[in] metatrader.exe或metatrader64.exe文件的路径。可选的未命名参数。首先显示，没有参数名称。如果没有指定此路径，模块会尝试自己查找可执行文件。

[in] 连接超时，以毫秒计算。可选的命名参数。如果没有指定，则应用60 000（60秒）的值。如果没有在指定时间内建立连接，则调用强制终止并生成异常。

[in] 交易账号。可选的命名参数。如果没有指定，则使用最后一个交易账户。

[in] 交易账号密码。可选的命名参数。如果没有设置密码，则自动应用在程序端数据库中保存的密码。

[in] 交易服务器名称。可选的命名参数。如果没有设置服务器，则自动应用最后使用的服务器。

成功连接MetaTrader 5程序端，返回True，否则，返回False。

如果有需要，当执行initialize()调用时，启动MetaTrader 5程序端建立连接。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立MetaTrader 5到指定交易账户的连接 if not mt5.initialize(login=25115284, server="MetaQuotes-Demo",password="4zatlbqx"): print("initialize() failed, error code =",mt5.last_error()) quit() # 显示有关连接状态、服务器名称和交易账户的数据 print(mt5.terminal_info()) # 显示有关MetaTrader 5版本的数据 print(mt5.version()) # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

shutdown、terminal_info、version

---

## last_error

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5lasterror_py

**Contents:**
- last_error

last_error()允许在MetaTrader 5程序库函数执行失败的情况下获取错误代码。它类似于GetLastError()。然而，它应用自己的错误代码。可能值：

RES_E_INVALID_VERSION

RES_E_AUTO_TRADING_DISABLED

RES_E_INTERNAL_FAIL_SEND

RES_E_INTERNAL_FAIL_RECEIVE

RES_E_INTERNAL_FAIL_INIT

RES_E_INTERNAL_FAIL_CONNECT

RES_E_INTERNAL_FAIL_TIMEOUT

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

---

## login

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5login_py

**Contents:**
- login

login( login, // 账号 password="PASSWORD", // 密码 server="SERVER", // 在程序端指定的服务器名称 timeout=TIMEOUT // 超时 )

[in] 交易账号密码。可选的命名参数。如果没有设置密码，则自动应用在程序端数据库中保存的密码。

[in] 交易服务器名称。可选的命名参数。如果没有设置服务器，则自动应用最后使用的服务器。

[in] 连接超时，以毫秒计算。可选的命名参数。如果没有指定，则应用60 000（60秒）的值。如果没有在指定时间内建立连接，则调用强制终止并生成异常。

成功连接交易账户，返回True，否则，返回False。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 显示有关MetaTrader 5版本的数据 print(mt5.version()) # 连接到指定密码和服务器的交易账户 account=17221085 authorized=mt5.login(account) # the terminal database password is applied if connection data is set to be remembered if authorized: print("connected to account #{}".format(account)) 其他： print("failed to connect at account #{}, error code: {}".format(account, mt5.last_error())) # 现在连接到指定密码的另一个交易账户 account=25115284 authorized=mt5.login(account, password="gqrtz0lbdm") if authorized: # display trading account data 'as is' print(mt5.account_info()) # display trading account data in the form of a list print("Show account_info()._asdict():") account_info_dict = mt5.account_info()._asdict() for prop in account_info_dict: print(" {}={}".format(prop, account_info_dict[prop])) 其他： print("failed to connect at account #{}, error code: {}".format(account, mt5.last_error())) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() Result: MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 [500, 2367, '23 Mar 2020'] connected to account #17221085 connected to account #25115284 AccountInfo(login=25115284, trade_mode=0, leverage=100, limit_orders=200, margin_so_mode=0, ... account properties: login=25115284 trade_mode=0 leverage=100 limit_orders=200 margin_so_mode=0 trade_allowed=True trade_expert=True margin_mode=2 currency_digits=2 fifo_close=False balance=99588.33 credit=0.0 profit=-45.23 equity=99543.1 margin=54.37 margin_free=99488.73 margin_level=183084.6054809638 margin_so_call=50.0 margin_so_so=30.0 margin_initial=0.0 margin_maintenance=0.0 assets=0.0 liabilities=0.0 commission_blocked=0.0 name=James Smith server=MetaQuotes-Demo currency=USD company=MetaQuotes Software Corp.

---

## market_book_add

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5marketbookadd_py

**Contents:**
- market_book_add

订阅MetaTrader 5程序端的指定交易品种的市场深度更改事件。

market_book_add( symbol // 交易品种名称 )

[in] 交易品种名称。所需的未命名参数。

market_book_get，market_book_release，市场深度结构

---

## market_book_get

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5marketbookget_py

**Contents:**
- market_book_get

从BookInfo返回一个元组，其中包含指定交易品种的市场深度条目。

market_book_get( symbol // 交易品种名称 )

[in] 交易品种名称。所需的未命名参数。

从BookInfo条目中以元组形式返回市场深度内容，其中包含订单类型、价格和手数交易量。BookInfo类似于MqlBookInfo结构。

错误情况下返回None。可使用last_error()获取错误信息。

应使用market_book_add()函数预先执行订阅市场深度更改事件。

import MetaTrader5 as mt5 import time # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) print("") # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() quit() # 订阅EURUSD（市场深度）的市场深度更新 if mt5.market_book_add('EURUSD'): # 循环获取10次市场深度数据 for i in range(10): # 获取市场深度内容（市场深度） items = mt5.market_book_get('EURUSD') # 以单个字符串'as is'（保持原来状态）显示整个市场深度 print(items) # 现在为了更加清晰而分别显示每个订单 if items: for it in items: # order content print(it._asdict()) # 暂停5秒后再请求下一个市场深度数据 time.sleep(5) # 取消订阅市场深度更新（市场深度） mt5.market_book_release('EURUSD') else: print("mt5.market_book_add('EURUSD') failed, error code =",mt5.last_error()) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.34 (BookInfo(type=1, price=1.20038, volume=250, volume_dbl=250.0), BookInfo(type=1, price=1.20032, volume=100, volume... {'type': 1, 'price': 1.20038, 'volume': 250, 'volume_dbl': 250.0} {'type': 1, 'price': 1.20032, 'volume': 100, 'volume_dbl': 100.0} {'type': 1, 'price': 1.2003, 'volume': 50, 'volume_dbl': 50.0} {'type': 1, 'price': 1.20028, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20026, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20025, 'volume': 50, 'volume_dbl': 50.0} {'type': 2, 'price': 1.20023, 'volume': 100, 'volume_dbl': 100.0} {'type': 2, 'price': 1.20017, 'volume': 250, 'volume_dbl': 250.0} (BookInfo(type=1, price=1.2004299999999999, volume=250, volume_dbl=250.0), BookInfo(type=1, price=1.20037, volume... {'type': 1, 'price': 1.2004299999999999, 'volume': 250, 'volume_dbl': 250.0} {'type': 1, 'price': 1.20037, 'volume': 100, 'volume_dbl': 100.0} {'type': 1, 'price': 1.20036, 'volume': 50, 'volume_dbl': 50.0} {'type': 1, 'price': 1.20034, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20031, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20029, 'volume': 50, 'volume_dbl': 50.0} {'type': 2, 'price': 1.20028, 'volume': 100, 'volume_dbl': 100.0} {'type': 2, 'price': 1.20022, 'volume': 250, 'volume_dbl': 250.0} (BookInfo(type=1, price=1.2004299999999999, volume=250, volume_dbl=250.0), BookInfo(type=1, price=1.20037, volume... {'type': 1, 'price': 1.2004299999999999, 'volume': 250, 'volume_dbl': 250.0} {'type': 1, 'price': 1.20037, 'volume': 100, 'volume_dbl': 100.0} {'type': 1, 'price': 1.20036, 'volume': 50, 'volume_dbl': 50.0} {'type': 1, 'price': 1.20034, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20031, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20029, 'volume': 50, 'volume_dbl': 50.0} {'type': 2, 'price': 1.20028, 'volume': 100, 'volume_dbl': 100.0} {'type': 2, 'price': 1.20022, 'volume': 250, 'volume_dbl': 250.0} (BookInfo(type=1, price=1.20036, volume=250, volume_dbl=250.0), BookInfo(type=1, price=1.20029, volume=100, volume... {'type': 1, 'price': 1.20036, 'volume': 250, 'volume_dbl': 250.0} {'type': 1, 'price': 1.20029, 'volume': 100, 'volume_dbl': 100.0} {'type': 1, 'price': 1.20028, 'volume': 50, 'volume_dbl': 50.0} {'type': 1, 'price': 1.20026, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20023, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20022, 'volume': 50, 'volume_dbl': 50.0} {'type': 2, 'price': 1.20021, 'volume': 100, 'volume_dbl': 100.0} {'type': 2, 'price': 1.20014, 'volume': 250, 'volume_dbl': 250.0} (BookInfo(type=1, price=1.20035, volume=250, volume_dbl=250.0), BookInfo(type=1, price=1.20029, volume=100, volume... {'type': 1, 'price': 1.20035, 'volume': 250, 'volume_dbl': 250.0} {'type': 1, 'price': 1.20029, 'volume': 100, 'volume_dbl': 100.0} {'type': 1, 'price': 1.20027, 'volume': 50, 'volume_dbl': 50.0} {'type': 1, 'price': 1.20025, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20023, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20022, 'volume': 50, 'volume_dbl': 50.0} {'type': 2, 'price': 1.20021, 'volume': 100, 'volume_dbl': 100.0} {'type': 2, 'price': 1.20014, 'volume': 250, 'volume_dbl': 250.0} (BookInfo(type=1, price=1.20037, volume=250, volume_dbl=250.0), BookInfo(type=1, price=1.20031, volume=100, volume... {'type': 1, 'price': 1.20037, 'volume': 250, 'volume_dbl': 250.0} {'type': 1, 'price': 1.20031, 'volume': 100, 'volume_dbl': 100.0} {'type': 1, 'price': 1.2003, 'volume': 50, 'volume_dbl': 50.0} {'type': 1, 'price': 1.20028, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20025, 'volume': 36, 'volume_dbl': 36.0} {'type': 2, 'price': 1.20023, 'volume': 50, 'volume_dbl': 50.0} {'type': 2, 'price': 1.20022, 'volume': 100, 'volume_dbl': 100.0} {'type': 2, 'price': 1.20016, 'volume': 250, 'volume_dbl': 250.0}

market_book_add，market_book_release， 市场深度结构

---

## market_book_release

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5marketbookrelease_py

**Contents:**
- market_book_release

取消订阅MetaTrader 5程序端的指定交易品种的市场深度更改事件。

market_book_release( symbol // 交易品种名称 )

[in] 交易品种名称。所需的未命名参数。

该函数类似于MarketBookRelease。

market_book_add，market_book_get，市场深度结构

---

## MetaTrader与Python集成的模块

**URL:** https://www.mql5.com/zh/docs/python_metatrader5

**Contents:**
- MetaTrader与Python集成的模块
  - 连接Python与MetaTrader 5的示例

MQL5语言专门用于开发金融市场中的高性能交易应用程序，是算法交易使用的其他专门语言中无法比拟的。MQL5程序的语法和速度尽可能接近C++语言，支持OpenCL和与MS Visual Studio的集成。也可使用统计，模糊逻辑和 ALGLIB程序库。MetaEditor开发环境通过"smart"函数导入对.NET程序库提供原生支持，消除开发特殊包装样式的需求。还可以使用第三方C++ DLL。 С++源代码文件(CPP和H)可以直接从编辑器中编辑并编译成DLL。安装在用户电脑上的Microsoft Visual Studio也可作此用途。

Python是一种用于开发脚本和应用程序的现代高水平编程语言。它包含用于机器学习、自动化处理以及数据分析和可视化的多个程序库。

MetaTrader Python程序包是为方便快速地从MetaTrader 5程序端直接通过处理器通信获得交易所数据而设计的。通过这种方式接收的数据可以进一步用于统计计算和机器学习。

pip安装 --更新MetaTrader5

用于集成MetaTrader 5和Python的函数

建立与MetaTrader 5程序端的连接

关于之前建立的与MetaTrader 5程序端的连接

获取所连接的MetaTrader 5程序端的状态和参数

获取MetaTrader 5程序端中所有交易品种的数量

获取MetaTrader 5程序端中的所有交易品种

在MarketWatch（市场报价）窗口中选择一个交易品种或从该窗口移除一个交易品种

订阅MetaTrader 5程序端的指定交易品种的市场深度更改事件

从BookInfo返回一个元组，其中包含指定交易品种的市场深度条目

取消订阅MetaTrader 5程序端的指定交易品种的市场深度更改事件

从指定日期开始从MetaTrader 5程序端获取柱形图

从指定索引开始从MetaTrader 5程序端获取柱形图

从MetaTrader 5程序端获取指定日期范围内的柱形图

从指定日期开始从MetaTrader 5程序端获取报价

从MetaTrader 5程序端获取指定日期范围内的报价

返回预付款（用账户货币表示）来执行指定的交易操作

获取可通过单号或持仓筛选的交易历史中的订单

获取可通过单号或持仓筛选的交易历史中的交易

pip安装matplotlib pip安装pandas

from datetime import datetime import matplotlib.pyplot as plt import pandas as pd from pandas.plotting import register_matplotlib_converters register_matplotlib_converters() import MetaTrader5 as mt5 # 连接到MetaTrader 5 if not mt5.initialize(): print("initialize() failed") mt5.shutdown() # 请求连接状态和参数 print(mt5.terminal_info()) # 获取有关MetaTrader 5版本的数据 print(mt5.version()) # 从EURAUD请求1000个报价 euraud_ticks = mt5.copy_ticks_from("EURAUD", datetime(2020,1,28,13), 1000, mt5.COPY_TICKS_ALL) # 请求2019.04.01 13:00 - 2019.04.02 13:00之间的AUDUSD报价 audusd_ticks = mt5.copy_ticks_range("AUDUSD", datetime(2020,1,27,13), datetime(2020,1,28,13), mt5.COPY_TICKS_ALL) # 通过多种方式获取不同交易品种的柱形图 eurusd_rates = mt5.copy_rates_from("EURUSD", mt5.TIMEFRAME_M1, datetime(2020,1,28,13), 1000) eurgbp_rates = mt5.copy_rates_from_pos("EURGBP", mt5.TIMEFRAME_M1, 0, 1000) eurcad_rates = mt5.copy_rates_range("EURCAD", mt5.TIMEFRAME_M1, datetime(2020,1,27,13), datetime(2020,1,28,13)) # 断开与MetaTrader 5的连接 mt5.shutdown() #DATA print('euraud_ticks(', len(euraud_ticks), ')') for val in euraud_ticks[:10]: print(val) print('audusd_ticks(', len(audusd_ticks), ')') for val in audusd_ticks[:10]: print(val) print('eurusd_rates(', len(eurusd_rates), ')') for val in eurusd_rates[:10]: print(val) print('eurgbp_rates(', len(eurgbp_rates), ')') for val in eurgbp_rates[:10]: print(val) print('eurcad_rates(', len(eurcad_rates), ')') for val in eurcad_rates[:10]: print(val) #PLOT # 从所获得的数据创建DataFrame ticks_frame = pd.DataFrame(euraud_ticks) # 将时间（以秒为单位）转换为日期时间格式 ticks_frame['time']=pd.to_datetime(ticks_frame['time'], unit='s') # 在图表上显示报价 plt.plot(ticks_frame['time'], ticks_frame['ask'], 'r-', label='ask') plt.plot(ticks_frame['time'], ticks_frame['bid'], 'b-', label='bid') # 显示图例 plt.legend(loc='upper left') # 添加标题 plt.title('EURAUD ticks') # 显示图表 plt.show()

[2, 'MetaQuotes-Demo', '16167573'] [500, 2325, '19 Feb 2020'] euraud_ticks( 1000 ) (1580209200, 1.63412, 1.63437, 0., 0, 1580209200067, 130, 0.) (1580209200, 1.63416, 1.63437, 0., 0, 1580209200785, 130, 0.) (1580209201, 1.63415, 1.63437, 0., 0, 1580209201980, 130, 0.) (1580209202, 1.63419, 1.63445, 0., 0, 1580209202192, 134, 0.) (1580209203, 1.6342, 1.63445, 0., 0, 1580209203004, 130, 0.) (1580209203, 1.63419, 1.63445, 0., 0, 1580209203487, 130, 0.) (1580209203, 1.6342, 1.63445, 0., 0, 1580209203694, 130, 0.) (1580209203, 1.63419, 1.63445, 0., 0, 1580209203990, 130, 0.) (1580209204, 1.63421, 1.63445, 0., 0, 1580209204194, 130, 0.) (1580209204, 1.63425, 1.63445, 0., 0, 1580209204392, 130, 0.) audusd_ticks( 40449 ) (1580122800, 0.67858, 0.67868, 0., 0, 1580122800244, 130, 0.) (1580122800, 0.67858, 0.67867, 0., 0, 1580122800429, 4, 0.) (1580122800, 0.67858, 0.67865, 0., 0, 1580122800817, 4, 0.) (1580122801, 0.67858, 0.67866, 0., 0, 1580122801618, 4, 0.) (1580122802, 0.67858, 0.67865, 0., 0, 1580122802928, 4, 0.) (1580122809, 0.67855, 0.67865, 0., 0, 1580122809526, 130, 0.) (1580122809, 0.67855, 0.67864, 0., 0, 1580122809699, 4, 0.) (1580122813, 0.67855, 0.67863, 0., 0, 1580122813576, 4, 0.) (1580122815, 0.67856, 0.67863, 0., 0, 1580122815190, 130, 0.) (1580122815, 0.67855, 0.67863, 0., 0, 1580122815479, 130, 0.) eurusd_rates( 1000 ) (1580149260, 1.10132, 1.10151, 1.10131, 1.10149, 44, 1, 0) (1580149320, 1.10149, 1.10161, 1.10143, 1.10154, 42, 1, 0) (1580149380, 1.10154, 1.10176, 1.10154, 1.10174, 40, 2, 0) (1580149440, 1.10174, 1.10189, 1.10168, 1.10187, 47, 1, 0) (1580149500, 1.10185, 1.10191, 1.1018, 1.10182, 53, 1, 0) (1580149560, 1.10182, 1.10184, 1.10176, 1.10183, 25, 3, 0) (1580149620, 1.10183, 1.10187, 1.10177, 1.10187, 49, 2, 0) (1580149680, 1.10187, 1.1019, 1.1018, 1.10187, 53, 1, 0) (1580149740, 1.10187, 1.10202, 1.10187, 1.10198, 28, 2, 0) (1580149800, 1.10198, 1.10198, 1.10183, 1.10188, 39, 2, 0) eurgbp_rates( 1000 ) (1582236360, 0.83767, 0.83767, 0.83764, 0.83765, 23, 9, 0) (1582236420, 0.83765, 0.83765, 0.83764, 0.83765, 15, 8, 0) (1582236480, 0.83765, 0.83766, 0.83762, 0.83765, 19, 7, 0) (1582236540, 0.83765, 0.83768, 0.83758, 0.83763, 39, 6, 0) (1582236600, 0.83763, 0.83768, 0.83763, 0.83767, 21, 6, 0) (1582236660, 0.83767, 0.83775, 0.83765, 0.83769, 63, 5, 0) (1582236720, 0.83769, 0.8377, 0.83758, 0.83764, 40, 7, 0) (1582236780, 0.83766, 0.83769, 0.8376, 0.83766, 37, 6, 0) (1582236840, 0.83766, 0.83772, 0.83763, 0.83772, 22, 6, 0) (1582236900, 0.83772, 0.83773, 0.83768, 0.8377, 36, 5, 0) eurcad_rates( 1441 ) (1580122800, 1.45321, 1.45329, 1.4526, 1.4528, 146, 15, 0) (1580122860, 1.4528, 1.45315, 1.45274, 1.45301, 93, 15, 0) (1580122920, 1.453, 1.45304, 1.45264, 1.45264, 82, 15, 0) (1580122980, 1.45263, 1.45279, 1.45231, 1.45277, 109, 15, 0) (1580123040, 1.45275, 1.4528, 1.45259, 1.45271, 53, 14, 0) (1580123100, 1.45273, 1.45285, 1.45269, 1.4528, 62, 16, 0) (1580123160, 1.4528, 1.45284, 1.45267, 1.45282, 64, 14, 0) (1580123220, 1.45282, 1.45299, 1.45261, 1.45272, 48, 14, 0) (1580123280, 1.45272, 1.45275, 1.45255, 1.45275, 74, 14, 0) (1580123340, 1.45275, 1.4528, 1.4526, 1.4528, 94, 13, 0)

---

## orders_get

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5ordersget_py

**Contents:**
- orders_get

获取可通过交易品种或单号筛选的活动订单。共有三个调用选项。

orders_get( symbol="SYMBOL" //交易品种名称 )

orders_get( group="GROUP" // 用于选择交易品种订单的过滤器 )

orders_get( ticket=TICKET // 单号 )

[in] 交易品种名称。可选的命名参数。如果指定交易品种，则忽略ticket参数。

[in] 用于跑了一组必要交易品种的过滤器。可选的命名参数。如果指定了组，则函数只返回满足交易品种名称指定条件的活动订单。

[in] 订单单号(ORDER_TICKET)。可选的命名参数。

以指定元组结构(namedtuple)的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数允许在一个类似于OrdersTotal和OrderSelect的串联调用中接收所有活动订单。

group参数可以根据交易品种对订单进行分类。'*'可以用在字符串的开头和结尾。

group参数可以包含多个逗号分隔条件。条件可以使用'*'设置为掩码。逻辑否定字符'!'可以用来表示排除条件。所有条件依序应用，这意味着应该先指定包含到组中的条件，然后再指定排除条件。例如，group="*, !EUR"意味着应该先选择所有交易品种的订单，然后排除交易品种名称中包含"EUR"的订单。

import MetaTrader5 as mt5 import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) print() # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 显示GBPUSD活动订单的数据 orders=mt5.orders_get(symbol="GBPUSD") if orders is None: print("No orders on GBPUSD, error code={}".format(mt5.last_error())) else: print("Total orders on GBPUSD:",len(orders)) # display all active orders for order in orders: print(order) print() # 获取名称包含"*GBP*"的交易品种的订单列表 gbp_orders=mt5.orders_get(group="*GBP*") if gbp_orders is None: print("No orders with group=\"*GBP*\", error code={}".format(mt5.last_error())) else: print("orders_get(group=\"*GBP*\")={}".format(len(gbp_orders))) # display these orders as a table using pandas.DataFrame df=pd.DataFrame(list(gbp_orders),columns=gbp_orders[0]._asdict().keys()) df.drop(['time_done', 'time_done_msc', 'position_id', 'position_by_id', 'reason', 'volume_initial', 'price_stoplimit'], axis=1, inplace=True) df['time_setup'] = pd.to_datetime(df['time_setup'], unit='s') print(df) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 Total orders on GBPUSD: 2 TradeOrder(ticket=554733548, time_setup=1585153667, time_setup_msc=1585153667718, time_done=0, time_done_msc=0, time_expiration=0, type=3, type_time=0, ... TradeOrder(ticket=554733621, time_setup=1585153671, time_setup_msc=1585153671419, time_done=0, time_done_msc=0, time_expiration=0, type=2, type_time=0, ... orders_get(group="*GBP*")=4 ticket time_setup time_setup_msc time_expiration type type_time type_filling state magic volume_current price_open sl tp price_current symbol comment external_id 0 554733548 2020-03-25 16:27:47 1585153667718 0 3 0 2 1 0 0.2 1.25379 0.0 0.0 1.16803 GBPUSD 1 554733621 2020-03-25 16:27:51 1585153671419 0 2 0 2 1 0 0.2 1.14370 0.0 0.0 1.16815 GBPUSD 2 554746664 2020-03-25 16:38:14 1585154294401 0 3 0 2 1 0 0.2 0.93851 0.0 0.0 0.92428 EURGBP 3 554746710 2020-03-25 16:38:17 1585154297022 0 2 0 2 1 0 0.2 0.90527 0.0 0.0 0.92449 EURGBP

orders_total、positions_get

---

## orders_total

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5orderstotal_py

**Contents:**
- orders_total

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 检查是否存在活动订单 orders=mt5.orders_total() if orders>0: print("Total orders=",orders) 其他： print("Orders not found") # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

orders_get、positions_total

---

## order_calc_margin

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5ordercalcmargin_py

**Contents:**
- order_calc_margin

返回预付款（用账户货币表示）来执行指定的交易操作。

order_calc_margin( action, // 订单类型 (ORDER_TYPE_BUY or ORDER_TYPE_SELL) symbol, // 交易品种名称 volume, // 交易量 price // 开盘价 )

[in] 从ORDER_TYPE枚举中获取值的订单类型。所需的未命名参数。

[in] 交易品种名称。所需的未命名参数。

[in] 交易操作的交易量。所需的未命名参数。

如果成功返回真实值，否则返回None。可以使用last_error()获取错误信息。

该功能允许评估当前账户和当前市场环境中指定订单类型所需的预付款，而无需考虑当前挂单和未结持仓。该函数类似于OrderCalcMargin。

ORDER_TYPE_SELL_LIMIT

ORDER_TYPE_BUY_STOP_LIMIT

在达到订单价格时，以StopLimit价格下单Buy Limit挂单

ORDER_TYPE_SELL_STOP_LIMIT

在达到订单价格时，以StopLimit价格下单Sell Limit挂单

import MetaTrader5 as mt5 # display data on the MetaTrader 5 package print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # establish connection to MetaTrader 5 terminal if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # get account currency account_currency=mt5.account_info().currency print("Account сurrency:",account_currency) # arrange the symbol list symbols=("EURUSD","GBPUSD","USDJPY", "USDCHF","EURJPY","GBPJPY") print("Symbols to check margin:", symbols) action=mt5.ORDER_TYPE_BUY lot=0.1 for symbol in symbols: symbol_info=mt5.symbol_info(symbol) if symbol_info is None: print(symbol,"not found, skipped") continue if not symbol_info.visible: print(symbol, "is not visible, trying to switch on") if not mt5.symbol_select(symbol,True): print("symbol_select({}}) failed, skipped",symbol) continue ask=mt5.symbol_info_tick(symbol).ask margin=mt5.order_calc_margin(action,symbol,lot,ask) if margin != None: print(" {} buy {} lot margin: {} {}".format(symbol,lot,margin,account_currency)); else: print("order_calc_margin failed: , error code =", mt5.last_error()) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() Result: MetaTrader5 package author: MetaQuotes Software Corp. MetaTrader5 package version: 5.0.29 Account сurrency: USD Symbols to check margin: ('EURUSD', 'GBPUSD', 'USDJPY', 'USDCHF', 'EURJPY', 'GBPJPY') EURUSD buy 0.1 lot margin: 109.91 USD GBPUSD buy 0.1 lot margin: 122.73 USD USDJPY buy 0.1 lot margin: 100.0 USD USDCHF buy 0.1 lot margin: 100.0 USD EURJPY buy 0.1 lot margin: 109.91 USD GBPJPY buy 0.1 lot margin: 122.73 USD

order_calc_profit、order_check

---

## order_calc_profit

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5ordercalcprofit_py

**Contents:**
- order_calc_profit

返回指定交易操作的盈利（用账户货币表示）。

order_calc_profit( action, // 订单类型 (ORDER_TYPE_BUY or ORDER_TYPE_SELL) symbol, // 交易品种名称 volume, // 交易量 price_open, // 开盘价 price_close // 收盘价 );

[in] 订单类型可以获取两个ORDER_TYPE枚举值其中一个：ORDER_TYPE_BUY或ORDER_TYPE_SELL。所需的未命名参数。

[in] 交易品种名称。所需的未命名参数。

[in] 交易操作的交易量。所需的未命名参数。

如果成功返回真实值，否则返回None。可以使用last_error()获取错误信息。

该函数允许评估当前账户和当前交易环境下的交易操作结果。该函数类似于OrderCalcProfit。

import MetaTrader5 as mt5 # display data on the MetaTrader 5 package print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # establish connection to MetaTrader 5 terminal if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # get account currency account_currency=mt5.account_info().currency print("Account сurrency:",account_currency) # arrange the symbol list symbols = ("EURUSD","GBPUSD","USDJPY") print("Symbols to check margin:", symbols) # estimate profit for buying and selling lot=1.0 distance=300 for symbol in symbols: symbol_info=mt5.symbol_info(symbol) if symbol_info is None: print(symbol,"not found, skipped") continue if not symbol_info.visible: print(symbol, "is not visible, trying to switch on") if not mt5.symbol_select(symbol,True): print("symbol_select({}}) failed, skipped",symbol) continue point=mt5.symbol_info(symbol).point symbol_tick=mt5.symbol_info_tick(symbol) ask=symbol_tick.ask bid=symbol_tick.bid buy_profit=mt5.order_calc_profit(mt5.ORDER_TYPE_BUY,symbol,lot,ask,ask+distance*point) if buy_profit!=None: print(" buy {} {} lot: profit on {} points => {} {}".format(symbol,lot,distance,buy_profit,account_currency)); else: print("order_calc_profit(ORDER_TYPE_BUY) failed, error code =",mt5.last_error()) sell_profit=mt5.order_calc_profit(mt5.ORDER_TYPE_SELL,symbol,lot,bid,bid-distance*point) if sell_profit!=None: print(" sell {} {} lots: profit on {} points => {} {}".format(symbol,lot,distance,sell_profit,account_currency)); else: print("order_calc_profit(ORDER_TYPE_SELL) failed, error code =",mt5.last_error()) print() # shut down connection to the MetaTrader 5 terminal mt5.shutdown() Result: MetaTrader5 package author: MetaQuotes Software Corp. MetaTrader5 package version: 5.0.29 Account сurrency: USD Symbols to check margin: ('EURUSD', 'GBPUSD', 'USDJPY') buy EURUSD 1.0 lot: profit on 300 points => 300.0 USD sell EURUSD 1.0 lot: profit on 300 points => 300.0 USD buy GBPUSD 1.0 lot: profit on 300 points => 300.0 USD sell GBPUSD 1.0 lot: profit on 300 points => 300.0 USD buy USDJPY 1.0 lot: profit on 300 points => 276.54 USD sell USDJPY 1.0 lot: profit on 300 points => 278.09 USD

order_calc_margin、order_check

---

## order_check

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5ordercheck_py

**Contents:**
- order_check

检查是否有足够的资金执行所需的交易操作。检查结果以MqlTradeCheckResult结构返回。

order_check( request // 请求结构 );

[in] 描述所需交易操作的MqlTradeRequest类型结构。所需的未命名参数。下面描述了填写请求和枚举内容的示例。

检查结果以MqlTradeCheckResult结构返回。回答中的request字段包含传递到order_check()的交易请求的结构。

成功发送请求不代表所请求的交易操作就可成功执行。order_check函数类似于OrderCheck。

TRADE_REQUEST_ACTIONS

TRADE_ACTION_CLOSE_BY

这个执行规则意味着一笔订单只能按照指定的交易量执行。如果金融产品的必要数额目前在市场上不能满足，则该订单将不会被执行。所需交易量可以由几个可用报价组成。

在订单中规定的交易量范围内，以市场最大交易量执行交易的协议。如果不能完全成交，则将执行可用交易量的订单，并取消其余交易量。

这个规则只适用于市价单（ORDER_TYPE_BUY和 ORDER_TYPE_SELL）、limit和stop limit订单（ORDER_TYPE_BUY_LIMIT、ORDER_TYPE_SELL_LIMIT、 ORDER_TYPE_BUY_STOP_LIMIT和ORDER_TYPE_SELL_STOP_LIMIT）以及市价执行和交易所执行模式的交易品种。如果部分成交，则剩余交易量的订单不会被取消，等待进一步处理。

在激活ORDER_TYPE_BUY_STOP_LIMIT和ORDER_TYPE_SELL_STOP_LIMIT订单期间，将创建一个对应的限价单ORDER_TYPE_BUY_LIMIT/ORDER_TYPE_SELL_LIMIT，并使用ORDER_FILLING_RETURN类型。

ORDER_TIME_SPECIFIED_DAY

订单在指定日23:59:59之前有效。如果此时间不在交易时段内，到期将在最近的交易时间内处理。

import MetaTrader5 as mt5 # display data on the MetaTrader 5 package print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # establish connection to MetaTrader 5 terminal if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # get account currency account_currency=mt5.account_info().currency print("Account сurrency:",account_currency) # 准备请求结构 symbol="USDJPY" symbol_info = mt5.symbol_info(symbol) if symbol_info is None: print(symbol, "not found, can not call order_check()") mt5.shutdown() quit() # 如果市场报价中没有此交易品种，请添加 if not symbol_info.visible: print(symbol, "is not visible, trying to switch on") if not mt5.symbol_select(symbol,True): print("symbol_select({}}) failed, exit",symbol) mt5.shutdown() quit() # prepare the request point=mt5.symbol_info(symbol).point request = { "action": mt5.TRADE_ACTION_DEAL, "symbol": symbol, "volume": 1.0, "type": mt5.ORDER_TYPE_BUY, "price": mt5.symbol_info_tick(symbol).ask, "sl": mt5.symbol_info_tick(symbol).ask-100*point, "tp": mt5.symbol_info_tick(symbol).ask+100*point, "deviation": 10, "magic": 234000, "comment": "python script", "type_time": mt5.ORDER_TIME_GTC, "type_filling": mt5.ORDER_FILLING_RETURN, } # 执行检查并显示结果'按原状' result = mt5.order_check(request) print(result); # request the result as a dictionary and display it element by element result_dict=result._asdict() for field in result_dict.keys(): print(" {}={}".format(field,result_dict[field])) # if this is a trading request structure, display it element by element as well if field=="request": traderequest_dict=result_dict[field]._asdict() for tradereq_filed in traderequest_dict: print(" traderequest: {}={}".format(tradereq_filed,traderequest_dict[tradereq_filed])) # shut down connection to the MetaTrader 5 terminal mt5.shutdown() Result: MetaTrader5 package author: MetaQuotes Software Corp. MetaTrader5 package version: 5.0.29 Account сurrency: USD retcode=0 balance=101300.53 equity=68319.53 profit=-32981.0 margin=51193.67 margin_free=17125.86 margin_level=133.45308121101692 comment=Done request=TradeRequest(action=1, magic=234000, order=0, symbol='USDJPY', volume=1.0, ... traderequest: action=1 traderequest: magic=234000 traderequest: order=0 traderequest: symbol=USDJPY traderequest: volume=1.0 traderequest: price=108.081 traderequest: stoplimit=0.0 traderequest: sl=107.98100000000001 traderequest: tp=108.181 traderequest: deviation=10 traderequest: type=0 traderequest: type_filling=2 traderequest: type_time=0 traderequest: expiration=0 traderequest: comment=python script traderequest: position=0 traderequest: position_by=0

order_send、 OrderCheck、交易操作类型、交易请求结构、 交易请求结构的检查结果、 交易请求结构的结果

---

## order_send

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5ordersend_py

**Contents:**
- order_send

从程序端向交易服务器发送请求来执行交易操作。该函数类似于OrderSend。

order_send( request // 请求结构 );

[in] 描述所需交易操作的MqlTradeRequest类型结构。所需的未命名参数。下面描述了填写请求和枚举内容的示例。

执行结果以MqlTradeResult结构返回。回答中的request字段包含传递到order_send()的交易请求的结构。可使用last_error()获取错误信息。

MqlTradeRequest交易请求结构

交易请求类型。该值可以是TRADE_REQUEST_ACTIONS其中一个枚举值

EA ID。可以安排交易订单的分析处理。每个EA交易都可以在发送交易请求时设置一个专有ID

要下单的交易品种名称。更改订单和平仓时不需要

请求的交易量（以手数表示）交易时的真实交易量取决于订单执行类型。

执行订单的价格。对于具有TRADE_ACTION_DEAL类型的“市价执行”(SYMBOL_TRADE_EXECUTION_MARKET)类型的交易品种的市价单，不设置价格

当价格达到'price'值时将设置Limit挂单（此条件是强制性的）。在此之前，挂单不会传递到交易系统中

请求价格的最大可接受偏差，在points中指定

订单类型。该值可以是ORDER_TYPE其中一个枚举值

订单成交类型。该值可以是ORDER_TYPE_FILLING其中一个值

订单到期类型。该值可以是ORDER_TYPE_TIME其中一个值

挂单到期时间（用于TIME_SPECIFIED类型订单）

持仓单号。当更改和关闭持仓时为清楚识别而填写。通常，它与持仓的订单单号相同。

反向持仓单号。它用于通过反向持仓平仓时（反向打开一个同名的交易品种）。

交易请求在交易服务器上通过多个验证阶段。首先，检查所有必要request字段的有效性。如果没有错误，服务器接受订单以进一步处理。有关执行交易操作的详细信息，请参阅OrderSend函数的描述。

import time import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ", mt5.__author__) print("MetaTrader5 package version: ", mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 准备买入请求结构 symbol = "USDJPY" symbol_info = mt5.symbol_info(symbol) if symbol_info is None: print(symbol, "not found, can not call order_check()") mt5.shutdown() quit() # 如果市场报价中没有此交易品种，请添加 if not symbol_info.visible: print(symbol, "is not visible, trying to switch on") if not mt5.symbol_select(symbol,True): print("symbol_select({}}) failed, exit",symbol) mt5.shutdown() quit() lot = 0.1 point = mt5.symbol_info(symbol).point price = mt5.symbol_info_tick(symbol).ask deviation = 20 request = { "action": mt5.TRADE_ACTION_DEAL, "symbol": symbol, "volume": lot, "type": mt5.ORDER_TYPE_BUY, "price": price, "sl": price - 100 * point, "tp": price + 100 * point, "deviation": deviation, "magic": 234000, "comment": "python script open", "type_time": mt5.ORDER_TIME_GTC, "type_filling": mt5.ORDER_FILLING_RETURN, } # 发送交易请求 result = mt5.order_send(request) # 检查执行结果 print("1. order_send(): by {} {} lots at {} with deviation={} points".format(symbol,lot,price,deviation)); if result.retcode != mt5.TRADE_RETCODE_DONE: print("2. order_send failed, retcode={}".format(result.retcode)) # 请求词典结果并逐个元素显示 result_dict=result._asdict() for field in result_dict.keys(): print(" {}={}".format(field,result_dict[field])) # if this is a trading request structure, display it element by element as well if field=="request": traderequest_dict=result_dict[field]._asdict() for tradereq_filed in traderequest_dict: print(" traderequest: {}={}".format(tradereq_filed,traderequest_dict[tradereq_filed])) print("shutdown() and quit") mt5.shutdown() quit() print("2. order_send done, ", result) print(" opened position with POSITION_TICKET={}".format(result.order)) print(" sleep 2 seconds before closing position #{}".format(result.order)) time.sleep(2) # 创建一个关闭请求 position_id=result.order price=mt5.symbol_info_tick(symbol).bid deviation=20 request={ "action": mt5.TRADE_ACTION_DEAL, "symbol": symbol, "volume": lot, "type": mt5.ORDER_TYPE_SELL, "position": position_id, "price": price, "deviation": deviation, "magic": 234000, "comment": "python script close", "type_time": mt5.ORDER_TIME_GTC, "type_filling": mt5.ORDER_FILLING_RETURN, } # 发送交易请求 result=mt5.order_send(request) # 检查执行结果 print("3. close position #{}: sell {} {} lots at {} with deviation={} points".format(position_id,symbol,lot,price,deviation)); if result.retcode != mt5.TRADE_RETCODE_DONE: print("4. order_send failed, retcode={}".format(result.retcode)) print(" result",result) 其他： print("4. position #{} closed, {}".format(position_id,result)) # 请求词典结果并逐个元素显示 result_dict=result._asdict() for field in result_dict.keys(): print(" {}={}".format(field,result_dict[field])) # if this is a trading request structure, display it element by element as well if field=="request": traderequest_dict=result_dict[field]._asdict() for tradereq_filed in traderequest_dict: print(" traderequest: {}={}".format(tradereq_filed,traderequest_dict[tradereq_filed])) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果 MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 1. order_send(): by USDJPY 0.1 lots at 108.023 with deviation=20 points 2. order_send done, OrderSendResult(retcode=10009, deal=535084512, order=557416535, volume=0.1, price=108.023, ... opened position with POSITION_TICKET=557416535 sleep 2 seconds before closing position #557416535 3. close position #557416535: sell USDJPY 0.1 lots at 108.018 with deviation=20 points 4. position #557416535 closed, OrderSendResult(retcode=10009, deal=535084631, order=557416654, volume=0.1, price=... retcode=10009 deal=535084631 order=557416654 volume=0.1 price=108.015 bid=108.015 ask=108.02 comment=Request executed request_id=55 retcode_external=0 request=TradeRequest(action=1, magic=234000, order=0, symbol='USDJPY', volume=0.1, price=108.018, stoplimit=0.0, ... traderequest: action=1 traderequest: magic=234000 traderequest: order=0 traderequest: symbol=USDJPY traderequest: volume=0.1 traderequest: price=108.018 traderequest: stoplimit=0.0 traderequest: sl=0.0 traderequest: tp=0.0 traderequest: deviation=20 traderequest: type=1 traderequest: type_filling=2 traderequest: type_time=0 traderequest: expiration=0 traderequest: comment=python script close traderequest: position=557416535 traderequest: position_by=0

order_check、OrderSend、交易操作类型、交易请求结构、交易请求结构的检查结果、交易请求结构的结果

---

## positions_get

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5positionsget_py

**Contents:**
- positions_get

获取可通过交易品种或单号筛选的未结持仓。共有三个调用选项。

positions_get( symbol="SYMBOL" // 交易品种名称 )

positions_get( group="GROUP" // 根据交易品种选择持仓的过滤器 )

positions_get( ticket=TICKET // 单号 )

[in] 交易品种名称。可选的命名参数。如果指定交易品种，则忽略ticket参数。

[in] 用于跑了一组必要交易品种的过滤器。可选的命名参数。如果指定了组，则函数只返回满足交易品种名称指定条件的持仓。

[in] 持仓单号(POSITION_TICKET)。可选的命名参数。

以指定元组结构(namedtuple)的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数允许在一个类似于PositionsTotal和PositionSelect的串联调用中接收所有未结持仓。

group参数可以包含多个逗号分隔条件。条件可以使用'*'设置为掩码。逻辑否定字符'!'可以用来表示排除条件。所有条件依序应用，这意味着应该先指定包含到组中的条件，然后再指定排除条件。例如，group="*, !EUR"意味着应该先选择所有交易品种的持仓，然后排除交易品种名称中包含"EUR"的持仓。

import MetaTrader5 as mt5 import pandas as pd pd.set_option('display.max_columns', 500) # number of columns to be displayed pd.set_option('display.width', 1500) # max table width to display # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) print() # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 获取USDCHF的未结持仓 positions=mt5.positions_get(symbol="USDCHF") if positions==None: print("No positions on USDCHF, error code={}".format(mt5.last_error())) elif len(positions)>0: print("Total positions on USDCHF =",len(positions)) # display all open positions for position in positions: print(position) # 获取名称包含"*USD*"的交易品种的持仓列表 usd_positions=mt5.positions_get(group="*USD*") if usd_positions==None: print("No positions with group=\"*USD*\", error code={}".format(mt5.last_error())) elif len(usd_positions)>0: print("positions_get(group=\"*USD*\")={}".format(len(usd_positions))) # display these positions as a table using pandas.DataFrame df=pd.DataFrame(list(usd_positions),columns=usd_positions[0]._asdict().keys()) df['time'] = pd.to_datetime(df['time'], unit='s') df.drop(['time_update', 'time_msc', 'time_update_msc', 'external_id'], axis=1, inplace=True) print(df) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 positions_get(group="*USD*")=5 ticket time type magic identifier reason volume price_open sl tp price_current swap profit symbol comment 0 548297723 2020-03-18 15:00:55 1 0 548297723 3 0.01 1.09301 1.11490 1.06236 1.10104 -0.10 -8.03 EURUSD 1 548655158 2020-03-18 20:31:26 0 0 548655158 3 0.01 1.08676 1.06107 1.12446 1.10099 -0.08 14.23 EURUSD 2 548663803 2020-03-18 20:40:04 0 0 548663803 3 0.01 1.08640 1.06351 1.11833 1.10099 -0.08 14.59 EURUSD 3 548847168 2020-03-19 01:10:05 0 0 548847168 3 0.01 1.09545 1.05524 1.15122 1.10099 -0.06 5.54 EURUSD 4 548847194 2020-03-19 01:10:07 0 0 548847194 3 0.02 1.09536 1.04478 1.16587 1.10099 -0.08 11.26 EURUSD

positions_total、orders_get

---

## positions_total

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5positionstotal_py

**Contents:**
- positions_total

该函数类似于PositionsTotal。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 检查是否存在未结持仓 positions_total=mt5.positions_total() if positions_total>0: print("Total positions=",positions_total) 其他： print("Positions not found") # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

positions_get、orders_total

---

## shutdown

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5shutdown_py

**Contents:**
- shutdown

关于之前建立的与MetaTrader 5程序端的连接。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed") quit() # 显示有关连接状态、服务器名称和交易账户的数据 print(mt5.terminal_info()) # 显示有关MetaTrader 5版本的数据 print(mt5.version()) # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

initialize、login_py、 terminal_info、version

---

## symbols_get

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5symbolsget_py

**Contents:**
- symbols_get

获取MetaTrader 5程序端中的所有交易品种。

symbols_get( group="GROUP" // 交易品种选择过滤器 )

[in] 用于跑了一组必要交易品种的过滤器。可选参数。如果指定了组，则函数只返回满足指定条件的交易品种。

以元组的形式返回交易品种。错误情况下返回None。可使用last_error()获取错误信息。

group参数可以根据名称对交易品种进行分类。'*'可以用在字符串的开头和结尾。

group参数可被用作命名的参数或未命名的参数。两个选项工作方式相同。命名的选项(group="GROUP")使代码更易于阅读。

group参数可以包含多个逗号分隔条件。条件可以使用'*'设置为掩码。逻辑否定字符'!'可以用来表示排除条件。所有条件依序应用，这意味着应该先指定包含到组中的条件，然后再指定排除条件。例如，group="*, !EUR"意味着应该先选择所有交易品种，然后排除名称中包含"EUR"的交易品种。

不同于symbol_info()，symbols_get()函数在单个调用中返回所有请求交易品种的数据。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 获取所有交易品种 symbols=mt5.symbols_get() print('Symbols: ', len(symbols)) count=0 # 显示前五个交易品种 for s in symbols: count+=1 print("{}. {}".format(count,s.name)) if count==5: break print() # 获取名称中包含RU的交易品种 ru_symbols=mt5.symbols_get("*RU*") print('len(*RU*): ', len(ru_symbols)) for s in ru_symbols: print(s.name) print() # 获取名称中不包含USD、EUR、JPY和GBP的交易品种 group_symbols=mt5.symbols_get(group="*,!*USD*,!*EUR*,!*JPY*,!*GBP*") print('len(*,!*USD*,!*EUR*,!*JPY*,!*GBP*):', len(group_symbols)) for s in group_symbols: print(s.name,":",s) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 交易品种：84 1. EURUSD 2. GBPUSD 3. USDCHF 4. USDJPY 5. USDCNH len(*RU*): 8 EURUSD USDRUB USDRUR EURRUR EURRUB FORTS.RUB.M5 EURUSD_T20 EURUSD4 len(*,!*USD*,!*EUR*,!*JPY*,!*GBP*): 13 AUDCAD : SymbolInfo(custom=False, chart_mode=0, select=True, visible=True, session_deals=0, session_buy_orders=0, session... AUDCHF : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... AUDNZD : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... CADCHF : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... NZDCAD : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... NZDCHF : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... NZDSGD : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... CADMXN : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... CHFMXN : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... NZDMXN : SymbolInfo(custom=False, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, sessi... FORTS.RTS.M5 : SymbolInfo(custom=True, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, ... FORTS.RUB.M5 : SymbolInfo(custom=True, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, ... FOREX.CHF.M5 : SymbolInfo(custom=True, chart_mode=0, select=False, visible=False, session_deals=0, session_buy_orders=0, ...

symbols_total、symbol_select、 symbol_info

---

## symbols_total

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5symbolstotal_py

**Contents:**
- symbols_total

获取MetaTrader 5程序端中所有交易品种的数量。

该函数类似于SymbolsTotal()。然而，它返回所有交易品种的数量，包括自定义交易品种，也包括市场报价中禁用的交易品种。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 获取交易品种的数量 symbols=mt5.symbols_total() if symbols>0: print("Total symbols =",symbols) 其他： print("symbols not found") # 断开与MetaTrader 5程序端的连接 mt5.shutdown()

symbols_get、symbol_select、 symbol_info

---

## symbol_info

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5symbolinfo_py

**Contents:**
- symbol_info

symbol_info( symbol // 交易品种名称 )

[in] 交易品种名称。所需的未命名参数。

以指定元组结构(namedtuple)的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数返回在一次调用中使用SymbolInfoInteger、 SymbolInfoDouble和SymbolInfoString可获得的所有数据。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 尝试在市场报价中启用显示EURJPY交易品种 selected=mt5.symbol_select("EURJPY",True) if not selected: print("Failed to select EURJPY") mt5.shutdown() quit() # 显示EURJPY交易品种属性 symbol_info=mt5.symbol_info("EURJPY") if symbol_info!=None: # display the terminal data 'as is' print(symbol_info) print("EURJPY: spread =",symbol_info.spread," digits =",symbol_info.digits) # display symbol properties as a list print("Show symbol_info(\"EURJPY\")._asdict():") symbol_info_dict = mt5.symbol_info("EURJPY")._asdict() for prop in symbol_info_dict: print(" {}={}".format(prop, symbol_info_dict[prop])) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 SymbolInfo(custom=False, chart_mode=0, select=True, visible=True, session_deals=0, session_buy_orders=0, session_sell_orders=0, ... EURJPY: spread = 17 digits = 3 Show symbol_info()._asdict(): custom=False chart_mode=0 select=True visible=True session_deals=0 session_buy_orders=0 session_sell_orders=0 volume=0 volumehigh=0 volumelow=0 time=1585069682 digits=3 spread=17 spread_float=True ticks_bookdepth=10 trade_calc_mode=0 trade_mode=4 start_time=0 expiration_time=0 trade_stops_level=0 trade_freeze_level=0 trade_exemode=1 swap_mode=1 swap_rollover3days=3 margin_hedged_use_leg=False expiration_mode=7 filling_mode=1 order_mode=127 order_gtc_mode=0 option_mode=0 option_right=0 bid=120.024 bidhigh=120.506 bidlow=118.798 ask=120.041 askhigh=120.526 asklow=118.828 last=0.0 lasthigh=0.0 lastlow=0.0 volume_real=0.0 volumehigh_real=0.0 volumelow_real=0.0 option_strike=0.0 point=0.001 trade_tick_value=0.8977708350166538 trade_tick_value_profit=0.8977708350166538 trade_tick_value_loss=0.8978272580355541 trade_tick_size=0.001 trade_contract_size=100000.0 trade_accrued_interest=0.0 trade_face_value=0.0 trade_liquidity_rate=0.0 volume_min=0.01 volume_max=500.0 volume_step=0.01 volume_limit=0.0 swap_long=-0.2 swap_short=-1.2 margin_initial=0.0 margin_maintenance=0.0 session_volume=0.0 session_turnover=0.0 session_interest=0.0 session_buy_orders_volume=0.0 session_sell_orders_volume=0.0 session_open=0.0 session_close=0.0 session_aw=0.0 session_price_settlement=0.0 session_price_limit_min=0.0 session_price_limit_max=0.0 margin_hedged=100000.0 price_change=0.0 price_volatility=0.0 price_theoretical=0.0 price_greeks_delta=0.0 price_greeks_theta=0.0 price_greeks_gamma=0.0 price_greeks_vega=0.0 price_greeks_rho=0.0 price_greeks_omega=0.0 price_sensitivity=0.0 basis= category= currency_base=EUR currency_profit=JPY currency_margin=EUR bank= description=Euro vs Japanese Yen exchange= formula= isin= name=EURJPY page=http://www.google.com/finance?q=EURJPY path=Forex\EURJPY

account_info、terminal_info

---

## symbol_info_tick

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5symbolinfotick_py

**Contents:**
- symbol_info_tick

symbol_info_tick( symbol // 交易品种名称 )

[in] 交易品种名称。所需的未命名参数。

以元组的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数类似于SymbolInfoTick。

import MetaTrader5 as mt5 # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 尝试在市场报价中启用显示GBPUSD selected=mt5.symbol_select("GBPUSD",True) if not selected: print("Failed to select GBPUSD") mt5.shutdown() quit() # 显示GBPUSD最后报价 lasttick=mt5.symbol_info_tick("GBPUSD") print(lasttick) # 以列表的形式显示报价字段值 print("Show symbol_info_tick(\"GBPUSD\")._asdict():") symbol_info_tick_dict = mt5.symbol_info_tick("GBPUSD")._asdict() for prop in symbol_info_tick_dict: print(" {}={}".format(prop, symbol_info_tick_dict[prop])) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 Tick(time=1585070338, bid=1.17264, ask=1.17279, last=0.0, volume=0, time_msc=1585070338728, flags=2, volume_real=0.0) Show symbol_info_tick._asdict(): time=1585070338 bid=1.17264 ask=1.17279 last=0.0 volume=0 time_msc=1585070338728 flags=2 volume_real=0.0

symbol_info、symbol_info

---

## symbol_select

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5symbolselect_py

**Contents:**
- symbol_select

在MarketWatch（市场报价）窗口中选择一个交易品种或从该窗口移除一个交易品种。

symbol_select( symbol, // 交易品种名称 enable=None // 启用或禁用 )

[in] 交易品种名称。所需的未命名参数。

[in] 切换。可选的未命名参数。如果是'false'，则交易品种应从MarketWatch（市场报价）窗口移除。否则，应该在市场报价窗口中将其选定。如果已存在一个交易品种的打开图表或有其未结持仓，则不可以删除此交易品种。

import MetaTrader5 as mt5 import pandas as pd # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) print() # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(login=25115284, server="MetaQuotes-Demo",password="4zatlbqx"): print("initialize() failed, error code =",mt5.last_error()) quit() # 尝试在市场报价中启用显示EURCAD selected=mt5.symbol_select("EURCAD",True) if not selected: print("Failed to select EURCAD, error code =",mt5.last_error()) 其他： symbol_info=mt5.symbol_info("EURCAD") print(symbol_info) print("EURCAD: currency_base =",symbol_info.currency_base," currency_profit =",symbol_info.currency_profit," currency_margin =",symbol_info.currency_margin) print() # get symbol properties in the form of a dictionary print("Show symbol_info()._asdict():") symbol_info_dict = symbol_info._asdict() for prop in symbol_info_dict: print(" {}={}".format(prop, symbol_info_dict[prop])) print() # 将词典转换为DataFrame和print df=pd.DataFrame(list(symbol_info_dict.items()),columns=['property','value']) print("symbol_info_dict() as dataframe:") print(df) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 SymbolInfo(custom=False, chart_mode=0, select=True, visible=True, session_deals=0, session_buy_orders=0, session_sell_orders=0, volume=0, volumehigh=0, .... EURCAD: currency_base = EUR currency_profit = CAD currency_margin = EUR Show symbol_info()._asdict(): custom=False chart_mode=0 select=True visible=True session_deals=0 session_buy_orders=0 session_sell_orders=0 volume=0 volumehigh=0 volumelow=0 time=1585217595 digits=5 spread=39 spread_float=True ticks_bookdepth=10 trade_calc_mode=0 trade_mode=4 start_time=0 expiration_time=0 trade_stops_level=0 trade_freeze_level=0 trade_exemode=1 swap_mode=1 swap_rollover3days=3 margin_hedged_use_leg=False expiration_mode=7 filling_mode=1 order_mode=127 order_gtc_mode=0 option_mode=0 option_right=0 bid=1.55192 bidhigh=1.55842 bidlow=1.5419800000000001 ask=1.5523099999999999 askhigh=1.55915 asklow=1.5436299999999998 last=0.0 lasthigh=0.0 lastlow=0.0 volume_real=0.0 volumehigh_real=0.0 volumelow_real=0.0 option_strike=0.0 point=1e-05 trade_tick_value=0.7043642408362214 trade_tick_value_profit=0.7043642408362214 trade_tick_value_loss=0.7044535553770941 trade_tick_size=1e-05 trade_contract_size=100000.0 trade_accrued_interest=0.0 trade_face_value=0.0 trade_liquidity_rate=0.0 volume_min=0.01 volume_max=500.0 volume_step=0.01 volume_limit=0.0 swap_long=-1.1 swap_short=-0.9 margin_initial=0.0 margin_maintenance=0.0 session_volume=0.0 session_turnover=0.0 session_interest=0.0 session_buy_orders_volume=0.0 session_sell_orders_volume=0.0 session_open=0.0 session_close=0.0 session_aw=0.0 session_price_settlement=0.0 session_price_limit_min=0.0 session_price_limit_max=0.0 margin_hedged=100000.0 price_change=0.0 price_volatility=0.0 price_theoretical=0.0 price_greeks_delta=0.0 price_greeks_theta=0.0 price_greeks_gamma=0.0 price_greeks_vega=0.0 price_greeks_rho=0.0 price_greeks_omega=0.0 price_sensitivity=0.0 basis= category= currency_base=EUR currency_profit=CAD currency_margin=EUR bank= description=Euro vs Canadian Dollar exchange= formula= isin= name=EURCAD page=http://www.google.com/finance?q=EURCAD path=Forex\EURCAD symbol_info_dict() as dataframe: property value 0 custom False 1 chart_mode 0 2 select True 3 visible True 4 session_deals 0 .. ... ... 91 formula 92 isin 93 name EURCAD 94 page http://www.google.com/finance?q=EURCAD 95 path Forex\EURCAD [96 rows x 2 columns]

---

## terminal_info

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5terminalinfo_py

**Contents:**
- terminal_info

获取已连接的MetaTrader 5客户端的状态和设置。

以指定元组结构(namedtuple)的形式返回信息。错误情况下返回None。可使用last_error()获取错误信息。

该函数返回在一次调用中使用TerminalInfoInteger、 TerminalInfoDouble和TerminalInfoDouble可获得的所有数据。

import MetaTrader5 as mt5 import pandas as pd # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 显示有关MetaTrader 5版本的数据 print(mt5.version()) # 显示有关程序端设置和状态的信息 terminal_info=mt5.terminal_info() if terminal_info!=None: # display the terminal data 'as is' print(terminal_info) # display data in the form of a list print("Show terminal_info()._asdict():") terminal_info_dict = mt5.terminal_info()._asdict() for prop in terminal_info_dict: print(" {}={}".format(prop, terminal_info_dict[prop])) print() # 将词典转换为DataFrame和print df=pd.DataFrame(list(terminal_info_dict.items()),columns=['property','value']) print("terminal_info() as dataframe:") print(df) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() 结果： MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 [500, 2366, '20 Mar 2020'] TerminalInfo(community_account=True, community_connection=True, connected=True,.... Show terminal_info()._asdict(): community_account=True community_connection=True connected=True dlls_allowed=False trade_allowed=False tradeapi_disabled=False email_enabled=False ftp_enabled=False notifications_enabled=False mqid=False build=2366 maxbars=5000 codepage=1251 ping_last=77850 community_balance=707.10668201585 retransmission=0.0 company=MetaQuotes Software Corp. name=MetaTrader 5 language=Russian path=E:\ProgramFiles\MetaTrader 5 data_path=E:\ProgramFiles\MetaTrader 5 commondata_path=C:\Users\Rosh\AppData\Roaming\MetaQuotes\Terminal\Common terminal_info() as dataframe: property value 0 community_account True 1 community_connection True 2 connected True 3 dlls_allowed False 4 trade_allowed False 5 tradeapi_disabled False 6 email_enabled False 7 ftp_enabled False 8 notifications_enabled False 9 mqid False 10 build 2367 11 maxbars 5000 12 codepage 1251 13 ping_last 80953 14 community_balance 707.107 15 retransmission 0.063593 16 company MetaQuotes Software Corp. 17 name MetaTrader 5 18 language Russian

initialize、shutdown、version

---

## version

**URL:** https://www.mql5.com/zh/docs/python_metatrader5/mt5version_py

**Contents:**
- version

返回MetaTrader 5程序端版本、build和发布日期。错误情况下返回None。可使用last_error()获取错误信息。

version()函数以三个值元组的形式返回程序端版本、build和发布日期：

import MetaTrader5 as mt5 import pandas as pd # 显示有关MetaTrader 5程序包的数据 print("MetaTrader5 package author: ",mt5.__author__) print("MetaTrader5 package version: ",mt5.__version__) # 建立与MetaTrader 5程序端的连接 if not mt5.initialize(): print("initialize() failed, error code =",mt5.last_error()) quit() # 显示有关MetaTrader 5版本的数据 print(mt5.version()) # display data on connection status, server name and trading account 'as is' print(mt5.terminal_info()) print() # 获取词典形式的属性 terminal_info_dict=mt5.terminal_info()._asdict() # 将词典转换为DataFrame和print df=pd.DataFrame(list(terminal_info_dict.items()),columns=['property','value']) print("terminal_info() as dataframe:") print(df[:-1]) # 断开与MetaTrader 5程序端的连接 mt5.shutdown() Result: MetaTrader5程序包作者：MetaQuotes Software Corp. MetaTrader5程序包版本：5.0.29 [500, 2367, '23 Mar 2020'] TerminalInfo(community_account=True, community_connection=True, connected=True, dlls_allowed=False, trade_allowed=False, ... terminal_info() as dataframe: property value 0 community_account True 1 community_connection True 2 connected True 3 dlls_allowed False 4 trade_allowed False 5 tradeapi_disabled False 6 email_enabled False 7 ftp_enabled False 8 notifications_enabled False 9 mqid False 10 build 2367 11 maxbars 5000 12 codepage 1251 13 ping_last 77881 14 community_balance 707.107 15 retransmission 0 16 company MetaQuotes Software Corp. 17 name MetaTrader 5 18 language Russian 19 path E:\ProgramFiles\MetaTrader 5 20 data_path E:\ProgramFiles\MetaTrader 5

initialize、shutdown、 terminal_info

---
