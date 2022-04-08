---
typora-copy-images-to: upload

---

###### 环境:101

###### 浏览器: Edge

###### 广告项目功能检查,以及相关问题解决



1. ##### Campaign添加/修改遇到的问题

   1. ##### ==添加==

   2. 创建Campaign(检查能否创建等)

   3. 添加Sku是成功的

   4. ![img](http://qiniu.binxixi.com//clipboard.png)

   5. 提示报错

   6. ![img](http://qiniu.binxixi.com//clipboard-16493843279271.png)

   7. 本地调试解决具体情况

   8. ![img](http://qiniu.binxixi.com//clipboard-16493843279272.png)

   9. 数据库不存在,在此数据库同步一下最新添加的

   10. ```c#
       var profileIdSync = new long?[] { apiModel.ProfileId };
       var campaignsIds = new long?[] { compaignId };
       await SpcampaignMongoDBSync(profileIdSync, null, campaignsIds);
       ```

   11. 最后还是会报错

   12. ![img](http://qiniu.binxixi.com//clipboard-16493843279283.png)

       1. 原因:发现在最后全部同步的时候出现的,portfolioIds为空数据转换出现的异常

   13. ![img](http://qiniu.binxixi.com//clipboard-16493843279284.png)





1. 第二个api请求,=NegativeKeywordMongoDBSyncAsync同步数据时
   1. 数据库得不到,空异常
   2. ![img](http://qiniu.binxixi.com//clipboard-16493843279285.png)
   3. 第二个请求同步NegativeKeyword的时候接口会超时,后台也同步相对比较慢
   4. 多线程的问题,如果传入了具体的Id的话,同步暂时不走多线程同步的方法
2. 添加完结
   1. ![image-20220408105617006](http://qiniu.binxixi.com//image-20220408105617006.png)



1. ##### ==修改==

   1. 发现查询时搜索含带 / 转义查询报错
   2. ![image-20220408111142896](http://qiniu.binxixi.com//image-20220408111142896.png)
   3. 修改是成功的
   4. ![image-20220408111633223](http://qiniu.binxixi.com//image-20220408111633223.png)
   5. ![image-20220408111648232](http://qiniu.binxixi.com//image-20220408111648232.png)





##### 2.单条操作

1. Profile列表
   1. Modify Budget修改	 ==OK==
2. Portfolio列表
   1. 编辑操作 	==OK==
      1. Add Campaigns操作 ==OK==
3. Campaign列表
   1. Status状态修改	==OK==
   2. Edit Campaign 	==OK==
   3. Modify Binding To Portfolio ==OK==
      1. 出现问题已经修复
      2. ![image-20220408134053416](http://qiniu.binxixi.com//image-20220408134053416.png)
   4. Add AdGroup ==OK==
   5. Add SKU ==OK==
      1. 同步是多线程出现异常,当传入确定的Id时,暂不需要多线程同步数据
   6. Add Keywords ==OK==
   7. Add Negative Keywords ==OK==
   8. Add Negative Products ==OK==
4. AdGroup列表
   1. Status状态修改
   2. Default Bid修改
   3. Add Sku
   4. Add Keywords
   5. Add Negative Keywords
   6. Add Negative Products
5. Targeting列表(加载不出来,其他功能未知)
   1. Status状态修改
6. NegativeTargeting列表
   1. State状态修改
7. ProductAds列表(加载不出来,其他功能未知)
   1. State状态修改
8. SearchTerm列表(加载不出来,其他功能未知)

##### 3.批量操作

##### 4.检查分页条数问题,筛选等特殊情况在查看分页是否丢失
