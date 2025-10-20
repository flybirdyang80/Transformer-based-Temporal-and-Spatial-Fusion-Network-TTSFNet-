# Transformer based Temporal and Spatial Fusion Network TTSFNet-
Spatiotemporal Fusion Network Based on Improved Transformer for Inverting Subsurface Thermohaline Structure

本项目提出了TTSFNet模型，在Transformer框架中引入融合卷积与时空增强块，仅用海面遥感异常即可一次性反演北太平洋5–2000 m共33层温盐异常。项目模型训练代码已开源，欢迎复用与改进。
研究区域：北太平洋（120°E–90°W，0–60°N）。

数据：

	变量	 来源	     空间分辨率	  时间分辨率	
	
	SSH	     AVISO	      0.25°	      	日	
	
	SSS	    SMAP/SMOS  	  0.125°	   日/月	
	
	SST	    UKMO OSTIA	  0.05°	        日	
	
    SSW	    CCMPv3.0	  0.125°  	    月	
   3D T/S	GLORYS12V1 	  0.25°	        月	
   

裁剪北太平洋区域，双线性重采样到0.25°，统一时间分辨率为月均。无效网格点剔除，计算海表异常数据；进行标准化。生成衍生变量DSSTA/DSSSA（海面-次表层月平均差）。3D T/S选33层（5–2000 m）。2011-2020年的数据作为训练集和验证集（随机选取30%作为验证集，70%作为训练集），2021年的数据作为测试集。

模型：模型设计了双分支Transformer结构，输入海面遥感异常（SSHA、SSTA、SSSA、SSWA）+衍生垂直变量（DSSTA/DSSSA），多头注意力+卷积前馈+Transformer提取20×20窗口的空间特征，TFEB+Transformer提取前12月的时间特征，融合后一次输出当前月份的33层STA/SSA。

<img width="4252" height="1645" alt="image" src="https://github.com/user-attachments/assets/5c25dd26-d12e-4cc5-8895-06c20a4a00fe" />


创新：提出DSSTA/DSSSA衍生变量，无需额外观测即显著提升5–200 m精度；设计SFEB/TFEB双增强块，多尺度可分离卷积+膨胀因果卷积，兼顾空间细节与时间长依赖；一次前向同时反演33层，训练速度提高，且垂直一致提高。

结果：2021全年33层温盐异常平均R²可达0.705/0.510，RMSE仅0.441 ℃/0.062 psu；较 FCNN/CNN/BiLSTM/CBAM-CNN/Transformer/MTSAN，R²提高0.029–0.158，RMSE降低8–15 %；且时空-垂向误差稳定。

发表文章：Mu J, Yang J, Wang C, et al. Spatiotemporal Fusion Network Based on Improved Transformer for Inverting Subsurface Thermohaline Structure[J]. IEEE Transactions on Geoscience and Remote Sensing, 2024.
