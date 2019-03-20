
## Test case on Beta

### Test Environment Preparation
1. WEBLAB need turn on
3. necessary ASIN (GS, FTZ (PS, BC, BBC), 3P)
4. Mock of UUB service down
5. how to validation 
6. what's kind of permission need to be confirmed
   1. access apollo env/pipelin/host, timber, aws account
   2. weblab operation permission

cases:
    only mock "UUBee unverified", "UUBee Failed" cases should be tested on Beta
   
## Test case on Gamma
### Test Environment 
1. mobile devices
   1. Apple's build
   2. Andriod build
1. Test endpoints, override check
2. ASIN preparation for (GS, FTZ (PS, BC, BBC), 3P)
3. payment credit card preparation
4. test account preparation
5. internal test checkpoints
   1. DynamoDB
   2. Log check
   3. YO
   4. Greener
   5. Herd
## weblab dialup impact

1. weblab 全吗？是否有别的组的weblab 影响我们？有哪些service 的weblab 需要调查？
2. 讨论一下weblab 是session based 还是 customer based。 dialup的顺序。 出现问题需要回滚会出现的可能场景
3. 如何在测试中override weblab，有哪些注意事项
4. weblab diabup 的MCM 由谁来负责


 - AGS_CN_KYC_EXPIRATION_DATE_160652:T1 
 - KYC_CASE_FRONTEND_ENABLE_PURCHASER_165940:T1 
 - KYC_TOKENATOR_INTEGRATION_SUBDATATYPE_EMPTY_ALLOWED_184929:T1 
 - CN_AGS_KYC_USE_REAL_PURCHASER_IN_CONSISTENCY_CHECK_156287
 - CN_AGS_KYC_VALIDATE_PAYER_PURCHASER_156286
 - KYC_MANAGEMENT_PURCHASER_COLLECTION_187107
 - UCP_DAS_PAYER_KYC_SUPPORT_185454: C
 - UCP_CDTS_PAYER_KYC_SUPPORT_184923: T1