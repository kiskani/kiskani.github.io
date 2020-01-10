---
layout: post
title:  "Risk Measurement in Finance"
date:   2020-01-03 15:52:14 -0800
categories: finance
---
In this post, I will explain the methods of measuring risk in finance. One of the widely used concepts to measue the risk of an asset is the [Sharpe Ratio](https://en.wikipedia.org/wiki/Sharpe_ratio). I have implemented a code in Python to compute the Sharpe Ratio for some assets. 

{% highlight python %}
from pandas_datareader import data  # You will need to run "pip install pandas_datareader"
import holidays   # You will need to run "pip install holidays"
import datetime 
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
{% endhighlight %}
{% highlight python %}
ONE_DAY = datetime.timedelta(days=1)
HOLIDAYS_US = holidays.US()

def previous_business_day(specific_date):
    previous_day = specific_date - ONE_DAY
    while previous_day.weekday() in holidays.WEEKEND or previous_day in HOLIDAYS_US:
        previous_day -= ONE_DAY
    return previous_day

def next_business_day(specific_date):
    next_day = specific_date + ONE_DAY
    while next_day.weekday() in holidays.WEEKEND or next_day in HOLIDAYS_US:
        next_day += ONE_DAY
    return next_day
{% endhighlight %}
{% highlight python %}
def get_historical_data(share, start_date='2019-01-01', end_date='2020-01-01'):
    try:
        panel_data = data.DataReader(share, 'yahoo', start_date, end_date)
    except: 
        panel_data = pd.DataFrame()
    return panel_data
{% endhighlight %}
{% highlight python %}
def compute_average_annual_return(share, start_date='2019-01-02', end_date='2019-12-31'):
    panel_data = get_historical_data(share, start_date, end_date)
    if panel_data.empty:
        return None, None, None, None, None
    
    adj_close = panel_data['Adj Close']
    
    start_date = datetime.datetime.strptime(start_date, '%Y-%m-%d')
    end_date = datetime.datetime.strptime(end_date, '%Y-%m-%d')
    
    while start_date.strftime('%Y-%m-%d') not in adj_close.index: 
        start_date = next_business_day(start_date)
    
    while end_date.strftime('%Y-%m-%d') not in adj_close.index: 
        end_date = previous_business_day(end_date)
    
    delta = end_date - start_date
    years = delta.days / 365
    
    start_price = adj_close.loc[start_date.strftime('%Y-%m-%d')]
    end_price = adj_close.loc[end_date.strftime('%Y-%m-%d')]
    
    return_value = (end_price/start_price)**(1/years) - 1
    return round(return_value, 4), start_date, end_date, delta, years
{% endhighlight %}
{% highlight python %}
def compute_daily_return_std_per_year(share, start_date='2019-01-02', end_date='2020-01-02'):
    panel_data = get_historical_data(share, start_date, end_date)
    if panel_data.empty:
        return None
    last_day_price = None
    daily_returns = [] 
    for index, row in panel_data.iterrows():
        if last_day_price:
            daily_returns.append(row['Adj Close']/last_day_price - 1)
        last_day_price = row['Adj Close']
    return round(np.std(daily_returns)*np.sqrt(252), 4)
{% endhighlight %}
{% highlight python %}
def compute_sharpe(share, start_date='2019-01-02', end_date='2020-01-02'):
    avg, _, _, _, _ = compute_average_annual_return(share, start_date, end_date)
    std = compute_daily_return_std_per_year(share, start_date, end_date)
    if not avg or not std:
        return
    return round(avg/std, 4)
{% endhighlight %}
{% highlight python %}
SP_500 = pd.read_excel('SP_500.xlsx', index_col=0)
SP_500_symbol_list = SP_500.index.to_list()[:-2]
iTickers = {}
iTickers["SPY"] = "SPDR S&P 500 ETF Trust"
iTickers["VOO"] = "Vanguard S&P 500 ETF"
iTickers["VTI"] = "Vanguard Total Stock Market ETF"
iTickers["BRK-B"] = "Berkshire Hathaway Inc."
iTickers["AAPL"] = "Apple Inc."
iTickers["BAC"] = "Bank of America"
iTickers["MSFT"] = "Microsoft"
iTickers["IVV"] = "iShares Core S&P 500 ETF"
{% endhighlight %}
{% highlight python %}
for symb in SP_500_symbol_list:  
    print("---------------\n", symb)
    sharpe_ratio = compute_sharpe(symb)
    print(SP_500.loc[symb]['Name'], sharpe_ratio)
{% endhighlight %}
---------------
 A
Agilent Technologies, Inc. 1.2822
---------------
 AAL
American Airlines Group, Inc. -0.3002
---------------
 AAP
Advance Auto Parts, Inc. 0.0596
---------------
 AAPL
Apple, Inc. 3.42
---------------
 ABBV
AbbVie, Inc. 0.1799
---------------
 ABC
AmerisourceBergen Corp. 0.6024
---------------
 ABMD
ABIOMED, Inc. -0.8983
---------------
 ABT
Abbott Laboratories 1.3995
---------------
 ACN
Accenture Plc 3.0866
---------------
 ADBE
Adobe, Inc. 1.9811
---------------
 ADI
Analog Devices, Inc. 1.4346
---------------
 ADM
Archer-Daniels-Midland Co. 0.857
---------------
 ADP
Automatic Data Processing, Inc. 1.8709
---------------
 ADS
Alliance Data Systems Corp. -0.7731
---------------
 ADSK
Autodesk, Inc. 1.4509
---------------
 AEE
Ameren Corp. 1.7286
---------------
 AEP
American Electric Power Co., Inc. 2.5355
---------------
 AES
The AES Corp. 2.2819
---------------
 AFL
Aflac, Inc. 1.2564
---------------
 AGN
Allergan Plc 1.2691
---------------
 AIG
American International Group, Inc. 1.4234
---------------
 AIV
Apartment Investment & Management Co. 1.7466
---------------
 AIZ
Assurant, Inc. 2.7122
---------------
 AJG
Arthur J. Gallagher & Co. 2.2164
---------------
 AKAM
Akamai Technologies, Inc. 2.1556
---------------
 ALB
Albemarle Corp. -0.1267
---------------
 ALGN
Align Technology, Inc. 0.8288
---------------
 ALK
Alaska Air Group, Inc. 0.5393
---------------
 ALL
The Allstate Corp. 2.526
---------------
 ALLE
Allegion Plc 2.8326
---------------
 ALXN
Alexion Pharmaceuticals, Inc. 0.2807
---------------
 AMAT
Applied Materials, Inc. 2.409
---------------
 AMCR
Amcor Plc 1.0252
---------------
 AMD
Advanced Micro Devices, Inc. 2.6893
---------------
 AME
AMETEK, Inc. 2.6476
---------------
 AMG
Affiliated Managers Group, Inc. -0.4056
---------------
 AMGN
Amgen, Inc. 1.4197
---------------
 AMP
Ameriprise Financial, Inc. 2.258
---------------
 AMT
American Tower Corp. 2.7276
---------------
 AMZN
Amazon.com, Inc. 0.8859
---------------
 ANET
Arista Networks, Inc. -0.0797
---------------
 ANSS
ANSYS, Inc. 3.4838
---------------
 ANTM
Anthem, Inc. 0.6925
---------------
 AON
Aon Plc 2.3342
---------------
 AOS
A. O. Smith Corp. 0.4623
---------------
 APA
Apache Corp. -0.0422
---------------
 APD
Air Products & Chemicals, Inc. 2.6578
---------------
 APH
Amphenol Corp. 1.7336
---------------
 APTV
Aptiv Plc 1.8452
---------------
 ARE
Alexandria Real Estate Equities, Inc. 3.1934
---------------
 ARNC
Arconic, Inc. 2.3872
---------------
 ATO
Atmos Energy Corp. 1.9956
---------------
 ATVI
Activision Blizzard, Inc. 0.7777
---------------
 AVB
AvalonBay Communities, Inc. 2.0036
---------------
 AVGO
Broadcom, Inc. 1.0017
---------------
 AVY
Avery Dennison Corp. 2.2881
---------------
 AWK
American Water Works Co., Inc. 2.8192
---------------
 AXP
American Express Co. 1.8875
---------------
 AZO
AutoZone, Inc. 2.04
---------------
 BA
The Boeing Co. 0.1015
---------------
 BAC
Bank of America Corp. 1.9287
---------------
 BAX
Baxter International, Inc. 1.4815
---------------
 BBT
BB&T Corp. 1.5622
---------------
 BBY
Best Buy Co., Inc. 2.0393
---------------
 BDX
Becton, Dickinson & Co. 1.1618
---------------
 BEN
Franklin Resources, Inc. -0.396
---------------
 BF.B
Brown-Forman Corp. None
---------------
 BHGE
Baker Hughes, a GE Co. None
---------------
 BIIB
Biogen, Inc. -0.0565
---------------
 BK
The Bank of New York Mellon Corp. 0.3823
---------------
 BKNG
Booking Holdings, Inc. 0.7732
---------------
 BLK
BlackRock, Inc. 1.6234
---------------
 BLL
Ball Corp. 2.0856
---------------
 BMY
Bristol-Myers Squibb Co. 1.0469
---------------
 BR
Broadridge Financial Solutions, Inc. 1.6564
---------------
 BRK.B
Berkshire Hathaway, Inc. None
---------------
 BSX
Boston Scientific Corp. 1.3376
---------------
 BWA
BorgWarner, Inc. 0.8292
---------------
 BXP
Boston Properties, Inc. 1.9126
---------------
 C
Citigroup, Inc. 2.2179
---------------
 CAG
Conagra Brands, Inc. 1.8254
---------------
 CAH
Cardinal Health, Inc. 0.6039
---------------
 CAT
Caterpillar, Inc. 0.7543
---------------
 CB
Chubb Ltd. 1.5421
---------------
 CBOE
Cboe Global Markets, Inc. 1.4099
---------------
 CBRE
CBRE Group, Inc. 2.3664
---------------
 CBS
ViacomCBS, Inc. -0.4197
---------------
 CCI
Crown Castle International Corp. 2.1631
---------------
 CCL
Carnival Corp. 0.2243
---------------
 CDNS
Cadence Design Systems, Inc. 2.3408
---------------
 CDW
CDW Corp. 3.4027
---------------
 CE
Celanese Corp. 1.5688
---------------
 CELG
Celgene Corp. 2.4394
---------------
 CERN
Cerner Corp. 2.1124
---------------
 CF
CF Industries Holdings, Inc. 0.5479
---------------
 CFG
Citizens Financial Group, Inc. (Rhode Island) 1.5065
---------------
 CHD
Church & Dwight Co., Inc. 0.5325
---------------
 CHRW
C.H. Robinson Worldwide, Inc. -0.1314
---------------
 CHTR
Charter Communications, Inc. 3.1157
---------------
 CI
Cigna Corp. 0.3097
---------------
 CINF
Cincinnati Financial Corp. 2.5029
---------------
 CL
Colgate-Palmolive Co. 1.2016
---------------
 CLX
The Clorox Co. 0.2278
---------------
 CMA
Comerica, Inc. 0.2566
---------------
 CMCSA
Comcast Corp. 1.7305
---------------
 CME
CME Group, Inc. 0.5989
---------------
 CMG
Chipotle Mexican Grill, Inc. 3.3372
---------------
 CMI
Cummins, Inc. 1.6624
---------------
 CMS
CMS Energy Corp. 2.3963
---------------
 CNC
Centene Corp. 0.3271
---------------
 CNP
CenterPoint Energy, Inc. 0.0777
---------------
 COF
Capital One Financial Corp. 1.4931
---------------
 COG
Cabot Oil & Gas Corp. -0.8582
---------------
 COO
The Cooper Cos., Inc. 1.4672
---------------
 COP
ConocoPhillips 0.1762
---------------
 COST
Costco Wholesale Corp. 2.8176
---------------
 COTY
Coty, Inc. 1.3052
---------------
 CPB
Campbell Soup Co. 2.4507
---------------
 CPRI
Capri Holdings Ltd. -0.0527
---------------
 CPRT
Copart, Inc. 4.4482
---------------
 CRM
salesforce.com, inc. 0.7849
---------------
 CSCO
Cisco Systems, Inc. 0.6108
---------------
 CSX
CSX Corp. 0.8323
---------------
 CTAS
Cintas Corp. 2.9706
---------------
 CTL
CenturyLink, Inc. -0.1797
---------------
 CTSH
Cognizant Technology Solutions Corp. -0.0196
---------------
 CTVA
Corteva, Inc. 0.1227
---------------
 CTXS
Citrix Systems, Inc. 0.6578
---------------
 CVS
CVS Health Corp. 0.6726
---------------
 CVX
Chevron Corp. 0.7294
---------------
 CXO
Concho Resources, Inc. -0.3966
---------------
 D
Dominion Energy, Inc. 1.5451
---------------
 DAL
Delta Air Lines, Inc. 0.8687
---------------
 DD
DuPont de Nemours, Inc. -0.4644
---------------
 DE
Deere & Co. 0.7414
---------------
 DFS
Discover Financial Services 2.083
---------------
 DG
Dollar General Corp. 2.1939
---------------
 DGX
Quest Diagnostics, Inc. 1.7378
---------------
 DHI
D.R. Horton, Inc. 1.9881
---------------
 DHR
Danaher Corp. 2.6473
---------------
 DIS
The Walt Disney Co. 1.533
---------------
 DISCA
Discovery, Inc. 0.8958
---------------
 DISCK
Discovery, Inc. 0.9954
---------------
 DISH
DISH Network Corp. 1.1273
---------------
 DLR
Digital Realty Trust, Inc. 1.0482
---------------
 DLTR
Dollar Tree, Inc. 0.1119
---------------
 DOV
Dover Corp. 2.9386
---------------
 DOW
Dow, Inc. 0.5831
---------------
 DRE
Duke Realty Corp. 2.8369
---------------
 DRI
Darden Restaurants, Inc. 0.5941
---------------
 DTE
DTE Energy Co. 1.8575
---------------
 DUK
Duke Energy Corp. 0.9944
---------------
 DVA
DaVita, Inc. 1.4173
---------------
 DVN
Devon Energy Corp. 0.3238
---------------
 DXC
DXC Technology Co. -0.5895
---------------
 EA
Electronic Arts, Inc. 0.9161
---------------
 EBAY
eBay, Inc. 1.1981
---------------
 ECL
Ecolab, Inc. 2.0533
---------------
 ED
Consolidated Edison, Inc. 1.9071
---------------
 EFX
Equifax, Inc. 2.742
---------------
 EIX
Edison International 1.427
---------------
 EL
The Estée Lauder Companies, Inc. 2.1808
---------------
 EMN
Eastman Chemical Co. 0.4323
---------------
 EMR
Emerson Electric Co. 1.4522
---------------
 EOG
EOG Resources, Inc. -0.1894
---------------
 EQIX
Equinix, Inc. 3.3266
---------------
 EQR
Equity Residential 2.2755
---------------
 ES
Eversource Energy 2.9657
---------------
 ESS
Essex Property Trust, Inc. 2.087
---------------
 ETFC
E*TRADE Financial Corp. 0.1015
---------------
 ETN
Eaton Corp. Plc 1.9676
---------------
 ETR
Entergy Corp. 3.4837
---------------
 EVRG
Evergy, Inc. 1.4818
---------------
 EW
Edwards Lifesciences Corp. 2.1031
---------------
 EXC
Exelon Corp. 0.4363
---------------
 EXPD
Expeditors International of Washington, Inc. 0.8608
---------------
 EXPE
Expedia Group, Inc. -0.0683
---------------
 EXR
Extra Space Storage, Inc. 1.5761
---------------
 F
Ford Motor Co. 0.9452
---------------
 FANG
Diamondback Energy, Inc. -0.0463
---------------
 FAST
Fastenal Co. 1.6376
---------------
 FB
Facebook, Inc. 1.8649
---------------
 FBHS
Fortune Brands Home & Security, Inc. 2.8977
---------------
 FCX
Freeport-McMoRan, Inc. 0.6918
---------------
 FDX
FedEx Corp. -0.1846
---------------
 FE
FirstEnergy Corp. 2.6099
---------------
 FFIV
F5 Networks, Inc. -0.5157
---------------
 FIS
Fidelity National Information Services, Inc. 2.0265
---------------
 FISV
Fiserv, Inc. 2.6269
---------------
 FITB
Fifth Third Bancorp 1.3258
---------------
 FLIR
FLIR Systems, Inc. 1.0036
---------------
 FLS
Flowserve Corp. 1.106
---------------
 FLT
FleetCor Technologies, Inc. 2.4021
---------------
 FMC
FMC Corp. 2.257
---------------
 FOX
Fox Corp. -0.2986
---------------
 FOXA
Fox Corp. -0.0561
---------------
 FRC
First Republic Bank (San Francisco, California) 1.5156
---------------
 FRT
Federal Realty Investment Trust 0.9899
---------------
 FTI
TechnipFMC Plc 0.3143
---------------
 FTNT
Fortinet, Inc. 1.701
---------------
 FTV
Fortive Corp. 0.6222
---------------
 GD
General Dynamics Corp. 0.7738
---------------
 GE
General Electric Co. 1.1187
---------------
 GILD
Gilead Sciences, Inc. 0.3022
---------------
 GIS
General Mills, Inc. 2.4564
---------------
 GL
Globe Life, Inc. 2.4475
---------------
 GLW
Corning, Inc. 0.0025
---------------
 GM
General Motors Co. 0.5506
---------------
 GOOG
Alphabet, Inc. 1.1608
---------------
 GOOGL
Alphabet, Inc. 1.1465
---------------
 GPC
Genuine Parts Co. 0.8425
---------------
 GPN
Global Payments, Inc. 3.6679
---------------
 GPS
Gap, Inc. -0.6969
---------------
 GRMN
Garmin Ltd. 2.3688
---------------
 GS
The Goldman Sachs Group, Inc. 1.5609
---------------
 GWW
W.W. Grainger, Inc. 0.9465
---------------
 HAL
Halliburton Co. -0.1969
---------------
 HAS
Hasbro, Inc. 1.0726
---------------
 HBAN
Huntington Bancshares, Inc. 1.2716
---------------
 HBI
Hanesbrands, Inc. 0.5615
---------------
 HCA
HCA Healthcare, Inc. 0.8642
---------------
 HCP
Healthpeak Properties, Inc. None
---------------
 HD
The Home Depot, Inc. 1.6676
---------------
 HES
Hess Corp. 1.7129
---------------
 HFC
HollyFrontier Corp. 0.0514
---------------
 HIG
The Hartford Financial Services Group, Inc. 2.8051
---------------
 HII
Huntington Ingalls Industries, Inc. 1.4897
---------------
 HLT
Hilton Worldwide Holdings, Inc. 2.6255
---------------
 HOG
Harley-Davidson, Inc. 0.4476
---------------
 HOLX
Hologic, Inc. 1.8187
---------------
 HON
Honeywell International, Inc. 2.1545
---------------
 HP
Helmerich & Payne, Inc. 0.0522
---------------
 HPE
Hewlett-Packard Enterprise Co. 0.7847
---------------
 HPQ
HP, Inc. 0.0966
---------------
 HRB
H&R Block, Inc. -0.1919
---------------
 HRL
Hormel Foods Corp. 0.6313
---------------
 HSIC
Henry Schein, Inc. 0.5011
---------------
 HST
Host Hotels & Resorts, Inc. 0.9615
---------------
 HSY
The Hershey Co. 2.63
---------------
 HUM
Humana, Inc. 1.0942
---------------
 IBM
International Business Machines Corp. 1.0766
---------------
 ICE
Intercontinental Exchange, Inc. 1.5188
---------------
 IDXX
IDEXX Laboratories, Inc. 1.7312
---------------
 IEX
IDEX Corp. 1.9647
---------------
 IFF
International Flavors & Fragrances, Inc. -0.0618
---------------
 ILMN
Illumina, Inc. 0.382
---------------
 INCY
Incyte Corp. 1.2015
---------------
 INFO
IHS Markit Ltd. 3.588
---------------
 INTC
Intel Corp. 1.1276
---------------
 INTU
Intuit, Inc. 1.488
---------------
 IP
International Paper Co. 0.7066
---------------
 IPG
Interpublic Group of Cos., Inc. 0.8802
---------------
 IPGP
IPG Photonics Corp. 0.6032
---------------
 IQV
IQVIA Holdings, Inc. 1.5215
---------------
 IR
Ingersoll-Rand Plc 2.4807
---------------
 IRM
Iron Mountain, Inc. 0.3202
---------------
 ISRG
Intuitive Surgical, Inc. 0.9726
---------------
 IT
Gartner, Inc. 0.8473
---------------
 ITW
Illinois Tool Works, Inc. 2.0171
---------------
 IVZ
Invesco Ltd. 0.5048
---------------
 JBHT
J.B. Hunt Transport Services, Inc. 1.0365
---------------
 JCI
Johnson Controls International Plc 2.0586
---------------
 JEC
Jacobs Engineering Group, Inc. 2.7853
---------------
 JKHY
Jack Henry & Associates, Inc. 1.1148
---------------
 JNJ
Johnson & Johnson 1.0164
---------------
 JNPR
Juniper Networks, Inc. -0.2425
---------------
 JPM
JPMorgan Chase & Co. 2.4143
---------------
 JWN
Nordstrom, Inc. -0.2401
---------------
 K
Kellogg Co. 1.2395
---------------
 KEY
KeyCorp 1.5849
---------------
 KEYS
Keysight Technologies, Inc. 1.9039
---------------
 KHC
The Kraft Heinz Co. -0.5553
---------------
 KIM
Kimco Realty Corp. 2.7108
---------------
 KLAC
KLA Corp. 3.245
---------------
 KMB
Kimberly-Clark Corp. 1.3925
---------------
 KMI
Kinder Morgan, Inc. 2.433
---------------
 KMX
CarMax, Inc. 1.4972
---------------
 KO
The Coca-Cola Co. 1.2852
---------------
 KR
The Kroger Co. 0.3082
---------------
 KSS
Kohl's Corp. -0.4963
---------------
 KSU
Kansas City Southern 2.549
---------------
 L
Loews Corp. 0.9722
---------------
 LB
L Brands, Inc. -0.5817
---------------
 LDOS
Leidos Holdings, Inc. 4.1772
---------------
 LEG
Leggett & Platt, Inc. 1.7215
---------------
 LEN
Lennar Corp. 1.5532
---------------
 LH
Laboratory Corp. of America Holdings 1.7061
---------------
 LHX
L3Harris Technologies, Inc. 2.202
---------------
 LIN
Linde Plc 1.9824
---------------
 LKQ
LKQ Corp. 1.8705
---------------
 LLY
Eli Lilly & Co. 0.8272
---------------
 LMT
Lockheed Martin Corp. 3.1099
---------------
 LNC
Lincoln National Corp. 0.584
---------------
 LNT
Alliant Energy Corp. 2.7578
---------------
 LOW
Lowe's Cos., Inc. 1.2479
---------------
 LRCX
Lam Research Corp. 3.1358
---------------
 LUV
Southwest Airlines Co. 0.7024
---------------
 LVS
Las Vegas Sands Corp. 1.2305
---------------
 LW
Lamb Weston Holdings, Inc. 0.7869
---------------
 LYB
LyondellBasell Industries NV 0.6392
---------------
 M
Macy's, Inc. -0.9758
---------------
 MA
Mastercard, Inc. 2.7492
---------------
 MAA
Mid-America Apartment Communities, Inc. 3.266
---------------
 MAC
Macerich Co. -1.1549
---------------
 MAR
Marriott International, Inc. 1.9141
---------------
 MAS
Masco Corp. 2.4401
---------------
 MCD
McDonald's Corp. 1.004
---------------
 MCHP
Microchip Technology, Inc. 1.4904
---------------
 MCK
McKesson Corp. 0.843
---------------
 MCO
Moody's Corp. 3.3418
---------------
 MDLZ
Mondelez International, Inc. 2.681
---------------
 MDT
Medtronic Plc 1.9571
---------------
 MET
MetLife, Inc. 1.3635
---------------
 MGM
MGM Resorts International 1.3329
---------------
 MHK
Mohawk Industries, Inc. 0.4546
---------------
 MKC
McCormick & Co., Inc. 1.3197
---------------
 MKTX
MarketAxess Holdings, Inc. 2.7141
---------------
 MLM
Martin Marietta Materials, Inc. 2.9652
---------------
 MMC
Marsh & McLennan Cos., Inc. 2.6999
---------------
 MMM
3M Co. -0.1807
---------------
 MNST
Monster Beverage Corp. 1.1435
---------------
 MO
Altria Group, Inc. 0.329
---------------
 MOS
The Mosaic Co. -0.6736
---------------
 MPC
Marathon Petroleum Corp. 0.1541
---------------
 MRK
Merck & Co., Inc. 1.2966
---------------
 MRO
Marathon Oil Corp. -0.1645
---------------
 MS
Morgan Stanley 1.3064
---------------
 MSCI
MSCI, Inc. 3.0146
---------------
 MSFT
Microsoft Corp. 2.9596
---------------
 MSI
Motorola Solutions, Inc. 1.7345
---------------
 MTB
M&T Bank Corp. 0.8994
---------------
 MTD
Mettler-Toledo International, Inc. 1.8726
---------------
 MU
Micron Technology, Inc. 1.4712
---------------
 MXIM
Maxim Integrated Products, Inc. 0.9329
---------------
 MYL
Mylan NV -0.5923
---------------
 NBL
Noble Energy, Inc. 0.7853
---------------
 NCLH
Norwegian Cruise Line Holdings Ltd. 1.5622
---------------
 NDAQ
Nasdaq, Inc. 1.8688
---------------
 NEE
NextEra Energy, Inc. 3.2634
---------------
 NEM
Newmont Goldcorp Corp. 1.3121
---------------
 NFLX
Netflix, Inc. 0.6069
---------------
 NI
NiSource, Inc. 0.8736
---------------
 NKE
NIKE, Inc. 1.8594
---------------
 NLSN
Nielsen Holdings Plc -0.3465
---------------
 NOC
Northrop Grumman Corp. 1.929
---------------
 NOV
National Oilwell Varco, Inc. -0.055
---------------
 NRG
NRG Energy, Inc. 0.2194
---------------
 NSC
Norfolk Southern Corp. 1.3745
---------------
 NTAP
NetApp, Inc. 0.1774
---------------
 NTRS
Northern Trust Corp. 1.3019
---------------
 NUE
Nucor Corp. 0.4401
---------------
 NVDA
NVIDIA Corp. 1.823
---------------
 NVR
NVR, Inc. 2.2974
---------------
 NWL
Newell Brands, Inc. 0.2123
---------------
 NWS
News Corp. 1.2408
---------------
 NWSA
News Corp. 1.1628
---------------
 O
Realty Income Corp. 1.4109
---------------
 OKE
ONEOK, Inc. 2.4097
---------------
 OMC
Omnicom Group, Inc. 0.8108
---------------
 ORCL
Oracle Corp. 0.9718
---------------
 ORLY
O'Reilly Automotive, Inc. 1.3389
---------------
 OXY
Occidental Petroleum Corp. -0.9898
---------------
 PAYX
Paychex, Inc. 2.1643
---------------
 PBCT
People's United Financial, Inc. 0.9132
---------------
 PCAR
PACCAR, Inc. 1.9661
---------------
 PEG
Public Service Enterprise Group, Inc. 1.5259
---------------
 PEP
PepsiCo, Inc. 2.0887
---------------
 PFE
Pfizer Inc. -0.3202
---------------
 PFG
Principal Financial Group, Inc. 1.1966
---------------
 PG
Procter & Gamble Co. 2.4878
---------------
 PGR
Progressive Corp. 1.2703
---------------
 PH
Parker-Hannifin Corp. 1.4712
---------------
 PHM
PulteGroup, Inc. 2.0064
---------------
 PKG
Packaging Corporation of America 1.6133
---------------
 PKI
PerkinElmer, Inc. (United States) 1.1199
---------------
 PLD
Prologis, Inc. 3.431
---------------
 PM
Philip Morris International, Inc. 1.4074
---------------
 PNC
The PNC Financial Services Group, Inc. 1.999
---------------
 PNR
Pentair Plc 0.9058
---------------
 PNW
Pinnacle West Capital Corp. 0.8155
---------------
 PPG
PPG Industries, Inc. 1.6953
---------------
 PPL
PPL Corp. 2.1263
---------------
 PRGO
Perrigo Co. Plc 0.9715
---------------
 PRU
Prudential Financial, Inc. 0.7685
---------------
 PSA
Public Storage 0.8224
---------------
 PSX
Phillips 66 1.4836
---------------
 PVH
PVH Corp. 0.3168
---------------
 PWR
Quanta Services, Inc. 1.5352
---------------
 PXD
Pioneer Natural Resources Co. 0.3838
---------------
 PYPL
PayPal Holdings, Inc. 1.0379
---------------
 QCOM
QUALCOMM, Inc. 1.5021
---------------
 QRVO
Qorvo, Inc. 2.4007
---------------
 RCL
Royal Caribbean Cruises Ltd. 1.5227
---------------
 RE
Everest Re Group Ltd. 1.8945
---------------
 REG
Regency Centers Corp. 0.9572
---------------
 REGN
Regeneron Pharmaceuticals, Inc. 0.0283
---------------
 RF
Regions Financial Corp. 1.1949
---------------
 RHI
Robert Half International, Inc. 0.5331
---------------
 RJF
Raymond James Financial, Inc. 1.0022
---------------
 RL
Ralph Lauren Corp. 0.4191
---------------
 RMD
ResMed, Inc. 1.3193
---------------
 ROK
Rockwell Automation, Inc. 1.3514
---------------
 ROL
Rollins, Inc. -0.2537
---------------
 ROP
Roper Technologies, Inc. 1.8198
---------------
 ROST
Ross Stores, Inc. 1.9604
---------------
 RSG
Republic Services, Inc. 2.1419
---------------
 RTN
Raytheon Co. 2.1518
---------------
 SBAC
SBA Communications Corp. 2.4648
---------------
 SBUX
Starbucks Corp. 2.0503
---------------
 SCHW
The Charles Schwab Corp. 0.556
---------------
 SEE
Sealed Air Corp. 0.5658
---------------
 SHW
The Sherwin-Williams Co. 2.3754
---------------
 SIVB
SVB Financial Group 0.8171
---------------
 SJM
The J. M. Smucker Co. 0.6944
---------------
 SLB
Schlumberger NV 0.4255
---------------
 SLG
SL Green Realty Corp. 1.2497
---------------
 SNA
Snap-On, Inc. 0.7522
---------------
 SNPS
Synopsys, Inc. 2.6688
---------------
 SO
The Southern Co. 3.7616
---------------
 SPG
Simon Property Group, Inc. -0.2836
---------------
 SPGI
S&P Global, Inc. 3.2019
---------------
 SRE
Sempra Energy 3.1758
---------------
 STI
SunTrust Banks, Inc. 1.8611
---------------
 STT
State Street Corp. 0.9961
---------------
 STX
Seagate Technology Plc 1.9466
---------------
 STZ
Constellation Brands, Inc. 0.6361
---------------
 SWK
Stanley Black & Decker, Inc. 1.2609
---------------
 SWKS
Skyworks Solutions, Inc. 2.3024
---------------
 SYF
Synchrony Financial 2.3245
---------------
 SYK
Stryker Corp. 1.7388
---------------
 SYMC
Symantec Corp. None
---------------
 SYY
Sysco Corp. 2.6276
---------------
 T
AT&T, Inc. 2.3005
---------------
 TAP
Molson Coors Beverage Co. -0.1075
---------------
 TDG
TransDigm Group, Inc. 3.2855
---------------
 TEL
TE Connectivity Ltd. 1.3231
---------------
 TFX
Teleflex, Inc. 2.0698
---------------
 TGT
Target Corp. 3.0048
---------------
 TIF
Tiffany & Co. 1.63
---------------
 TJX
The TJX Cos., Inc. 1.9768
---------------
 TMO
Thermo Fisher Scientific, Inc. 2.1704
---------------
 TMUS
T-Mobile US, Inc. 0.9927
---------------
 TPR
Tapestry, Inc. -0.4018
---------------
 TRIP
TripAdvisor, Inc. -0.915
---------------
 TROW
T. Rowe Price Group, Inc. 1.6066
---------------
 TRV
The Travelers Cos., Inc. 1.1819
---------------
 TSCO
Tractor Supply Co. 0.6648
---------------
 TSN
Tyson Foods, Inc. 3.0919
---------------
 TTWO
Take-Two Interactive Software, Inc. 0.5347
---------------
 TWTR
Twitter, Inc. 0.2714
---------------
 TXN
Texas Instruments Incorporated 1.4901
---------------
 TXT
Textron, Inc. -0.1102
---------------
 UA
Under Armour, Inc. 0.4168
---------------
 UAA
Under Armour, Inc. 0.5302
---------------
 UAL
United Airlines Holdings, Inc. 0.2033
---------------
 UDR
UDR, Inc. 1.7367
---------------
 UHS
Universal Health Services, Inc. 1.0165
---------------
 ULTA
Ulta Beauty, Inc. 0.0491
---------------
 UNH
UnitedHealth Group, Inc. 0.911
---------------
 UNM
Unum Group 0.0118
---------------
 UNP
Union Pacific Corp. 1.398
---------------
 UPS
United Parcel Service, Inc. 1.0239
---------------
 URI
United Rentals, Inc. 1.5565
---------------
 USB
U.S. Bancorp 1.9722
---------------
 UTX
United Technologies Corp. 2.0101
---------------
 V
Visa, Inc. 2.3626
---------------
 VAR
Varian Medical Systems, Inc. 1.0535
---------------
 VFC
VF Corp. 2.0443
---------------
 VIAB
Viacom, Inc. -0.2859
---------------
 VLO
Valero Energy Corp. 1.0473
---------------
 VMC
Vulcan Materials Co. 2.3697
---------------
 VNO
Vornado Realty Trust 1.1283
---------------
 VRSK
Verisk Analytics, Inc. 2.4437
---------------
 VRSN
VeriSign, Inc. 1.4714
---------------
 VRTX
Vertex Pharmaceuticals, Inc. 1.2537
---------------
 VTR
Ventas, Inc. 0.3155
---------------
 VZ
Verizon Communications, Inc. 0.904
---------------
 WAB
Westinghouse Air Brake Technologies Corp. 0.3099
---------------
 WAT
Waters Corp. 1.0044
---------------
 WBA
Walgreens Boots Alliance, Inc. -0.3924
---------------
 WCG
WellCare Health Plans, Inc. 1.5651
---------------
 WDC
Western Digital Corp. 1.4881
---------------
 WEC
WEC Energy Group, Inc. 2.9024
---------------
 WELL
Welltower, Inc. 1.5196
---------------
 WFC
Wells Fargo & Co. 0.9797
---------------
 WHR
Whirlpool Corp. 1.3925
---------------
 WLTW
Willis Towers Watson Plc 1.9077
---------------
 WM
Waste Management, Inc. 2.1952
---------------
 WMB
The Williams Cos., Inc. 0.6034
---------------
 WMT
Walmart, Inc. 2.0939
---------------
 WRK
WestRock Co. 0.5056
---------------
 WU
The Western Union Co. 3.4052
---------------
 WY
Weyerhaeuser Co. 2.1843
---------------
 WYNN
Wynn Resorts Ltd. 0.9696
---------------
 XEC
Cimarex Energy Co. -0.3449
---------------
 XEL
Xcel Energy, Inc. 2.5163
---------------
 XLNX
Xilinx, Inc. 0.3475
---------------
 XOM
Exxon Mobil Corp. 0.2717
---------------
 XRAY
Dentsply Sirona, Inc. 1.9363
---------------
 XRX
Xerox Holdings Corp. 2.8264
---------------
 XYL
Xylem, Inc. 0.887
---------------
 YUM
Yum! Brands, Inc. 0.7777
---------------
 ZBH
Zimmer Biomet Holdings, Inc. 2.1879
---------------
 ZION
Zions Bancorporation NA 1.138
---------------
 ZTS
Zoetis, Inc. 2.8062
{% highlight python %}
plt.figure(figsize=(14,4))
start_date='2000-01-31' 
for symb in iTickers:
    print(iTickers[symb])
    daa = get_historical_data(symb, start_date=start_date)
    sharpe_ratio = compute_sharpe(symb)
    #print(SP_500.loc[symb]['Name'], sharpe_ratio)
    print(iTickers[symb], sharpe_ratio)
    plt.plot(daa['Adj Close'])
    plt.grid(color='g', linestyle='-.', linewidth=0.5)
plt.legend([iTickers[sym] for sym in iTickers])
plt.savefig('well-known-tickers.png')
{% endhighlight %}
{% highlight text %}
SPDR S&P 500 ETF Trust
SPDR S&P 500 ETF Trust 2.5004
Vanguard S&P 500 ETF
Vanguard S&P 500 ETF 2.5269
Vanguard Total Stock Market ETF
Vanguard Total Stock Market ETF 2.4393
Berkshire Hathaway Inc.
Berkshire Hathaway Inc. 0.7833
Apple Inc.
Apple Inc. 3.42
Bank of America
Bank of America 1.9287
Microsoft
Microsoft 2.9596
iShares Core S&P 500 ETF
iShares Core S&P 500 ETF 2.5012
{% endhighlight %}
![Some SP 500 tickers](well-known-tickers.png)
{% highlight python %}
plt.figure(figsize=(14,4))
start_date='2010-01-02' 
end_date='2020-01-07' 
for symb in ['BRK-B','SPY']:
    daa = get_historical_data(symb, start_date=start_date, end_date=end_date)
    plt.plot(daa['Adj Close'])
    plt.grid(color='g', linestyle='-.', linewidth=0.5)
plt.legend(['BRK-B','SPY'])
plt.savefig('BRK_vs_SPY.png')
{% endhighlight %}
![Berkshire Hathaway vs SP 500](BRK_vs_SPY.png)
{% highlight python %}
def plot_stock_price(stock, start_date='2019-01-02', end_date='2019-12-31'):
    plt.figure(figsize=(14,4))
    daa = get_historical_data(stock, start_date=start_date, end_date=end_date)
    sharpe_ratio = compute_sharpe(stock)
    plt.plot(daa['Adj Close'])
    plt.grid(color='g', linestyle='-.', linewidth=0.5)
    plt.legend([stock])
    plt.savefig('{}_{}_{}.png'.format(stock, start_date, end_date))
{% endhighlight %}
{% highlight python %}
plot_stock_price('GOOG', start_date='2007-01-02', end_date='2010-01-02')
{% endhighlight %}
![Google](GOOG_2007-01-02_2010-01-02.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1900-01-01', end_date='1960-01-01')
{% endhighlight %}
![SP500_1900_1960](^GSPC_1900-01-01_1960-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1960-01-01', end_date='1970-01-01')
{% endhighlight %}
![SP500_1960_1970](^GSPC_1960-01-01_1970-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1970-01-01', end_date='1980-01-01')
{% endhighlight %}
![SP500_1970_1980](^GSPC_1970-01-01_1980-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1980-01-01', end_date='1990-01-01')
{% endhighlight %}
![SP500_1980_1990](^GSPC_1980-01-01_1990-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='1990-01-01', end_date='2000-01-01')
{% endhighlight %}
![SP500_1990_2000](^GSPC_1990-01-01_2000-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='2000-01-01', end_date='2010-01-01')
{% endhighlight %}
![SP500_2000_2010](^GSPC_2000-01-01_2010-01-01.png)
{% highlight python %}
plot_stock_price('^GSPC', start_date='2010-01-01', end_date='2020-01-01')
{% endhighlight %}
![SP500_2010_2020](^GSPC_2010-01-01_2020-01-01.png)
{% highlight python %}
nasdaq = data.get_nasdaq_symbols()
print(nasdaq.shape)
nasdaq.head()
{% endhighlight %}
{% highlight python %}
def compute_stats(symb):
    ticker = data.get_quote_yahoo(symb) 
    ticker['c1_marketCap_bn_calc'] = ticker['price'] * ticker['sharesOutstanding'] / 1e9
    ticker['c2_marketCap_bn_orig'] = ticker['marketCap'] / 1e9
    ticker['c3_price_calc'] = ticker['epsForward'] * ticker['trailingPE']
    ticker['c4_earnings_bn_calc'] = ticker['epsForward'] * ticker['sharesOutstanding'] / 1e9
    return ticker
{% endhighlight %}
{% highlight python %}
compute_stats(['EXPE', 'GOOG', 'MSFT', 'AAPL', 'BB', 'JNPR', 'BRK-B', 'BRK-A'])
{% endhighlight %}
A summary of this code is available in [this notebook](https://github.com/kiskani/kiskani.github.io/blob/master/finance/2020/01/03/Finance-Notebook-1.ipynb).
