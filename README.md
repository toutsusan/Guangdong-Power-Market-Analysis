# 广东电力市场结构演变与电价驱动因素量化分析（2022-2024）
> 本项目旨在利用官方发布的年度报告数据，通过量化分析手段，揭示广东省电力市场的运行规律，并识别影响月度成交均价的核心驱动因素。

该项目仅作简单分析，所有数据均来自官方公布资料，感兴趣可选择仓库下载查看

这是一个个人量化分析项目，聚焦于广东电力市场2022-2024年的结构演变与电价驱动因素。基于广东电力交易中心的官方年度报告和月度CSV数据，使用OLS回归模型（Python/SPSS实现）识别核心驱动变量，如用电侧HHI对电价的显著正向影响。项目包括数据处理、回归分析、模型诊断和政策建议，旨在为市场主体提供决策参考，并作为电力市场改革的入门指南。

关键发现：
用电侧HHI是电价主要驱动（系数6.928, p<0.001）。
2024年新能源装机增长100%，绿电交易突破72.6亿kWh，导致发电侧集中度负影响。
模型R²=0.955，诊断良好，但存在中度共线性。

项目适合入门行业的人员参考。

关于python做ols回归分析的参考代码如下

    import pandas as pd
    import statsmodels.api as sm
    
    df = pd.read_csv('文件位置',encoding='UTF-8-SIG') # 读取并准备数据
    
    df['日期'] = pd.to_datetime(df['日期'], format='%Y%m') # 将日期列设为索引
    df = df.set_index('日期')
    
    Y = df['价格_元_MWh'] # 因变量Y，可自行调整
    
    X = df[['成交量_GWh', '用电侧HHI', '发电侧HHI', '发电侧Top4份额']] # 自变量X，可自行调整
    
    X = sm.add_constant(X) # statsmodels需要手动添加一个常数项
    
    model = sm.OLS(Y, X).fit() # 构建并拟合模型
    
    with open('存储位置', 'w', encoding='utf-8') as f:
        f.write(model.summary().as_html())
    
    print("回归结果已成功导出为以下文件：")
    print("1. 文本格式: regression_results.txt")
    print("2. CSV系数表: regression_coefficients.csv")
    print("3. HTML格式: regression_results.html")
