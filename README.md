特殊验证

手机号验证

 //手机号以13， 15，18开头，八个 \d 数字字符
    NSString *phoneRegex = @"^((13[0-9])|(15[^4,\\D])|(18[0,0-9]))\\d{8}$";
    NSPredicate *phoneTest = [NSPredicate predicateWithFormat:@"SELF MATCHES %@",phoneRegex];
    return [phoneTest evaluateWithObject:mobile];
手机号归属验证

密码格式验证

// 正则匹配用户密码6-18位数字和字母组合
+ (BOOL)checkPassword:(NSString*) password

{

NSString*pattern =@"^(?![0-9]+$)(?![a-zA-Z]+$)[a-zA-Z0-9]{6,18}";

NSPredicate*pred = [NSPredicatepredicateWithFormat:@"SELF MATCHES %@",pattern];

BOOLisMatch = [predevaluateWithObject:password];

returnisMatch;

}
用户名格式验证

正则匹配用户姓名,20位的中文或英文

+ (BOOL)checkUserName : (NSString*) userName

{

NSString*pattern =@"^[a-zA-Z\u4E00-\u9FA5]{1,20}";

NSPredicate*pred = [NSPredicatepredicateWithFormat:@"SELF MATCHES %@",pattern];

BOOLisMatch = [predevaluateWithObject:userName];

returnisMatch;

}
邮箱验证

+ (BOOL) validateEmail:(NSString *)email
{
    NSString *emailRegex = @"[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,4}";
    NSPredicate *emailTest = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", emailRegex];
    return [emailTest evaluateWithObject:email];
}
银行卡号验证

+ (BOOL)isBankCard:(NSString *)cardNo{

    if (cardNo.length < 16) {
        return NO;
    }

    NSInteger oddsum = 0;     //奇数求和
    NSInteger evensum = 0;    //偶数求和
    NSInteger allsum = 0;
    NSInteger cardNoLength = (NSInteger)[cardNo length];
    // 取了最后一位数
    NSInteger lastNum = [[cardNo substringFromIndex:cardNoLength-1] intValue];
    //测试的是除了最后一位数外的其他数字
    cardNo = [cardNo substringToIndex:cardNoLength - 1];
    for (NSInteger i = cardNoLength -1 ; i>=1;i--) {
        NSString *tmpString = [cardNo substringWithRange:NSMakeRange(i-1, 1)];
        NSInteger tmpVal = [tmpString integerValue];
        if (cardNoLength % 2 ==1 ) {//卡号位数为奇数
            if((i % 2) == 0){//偶数位置

                tmpVal *= 2;
                if(tmpVal>=10)
                    tmpVal -= 9;
                evensum += tmpVal;
            }else{//奇数位置
                oddsum += tmpVal;
            }
        }else{
            if((i % 2) == 1){
                tmpVal *= 2;
                if(tmpVal>=10)
                    tmpVal -= 9;
                evensum += tmpVal;
            }else{
                oddsum += tmpVal;
            }
        }
    }

    allsum = oddsum + evensum;
    allsum += lastNum;
    if((allsum % 10) == 0)
        return YES;
    else
        return NO;
}
银行卡号所属银行验证

+(NSString *)bankOfCard:(NSString *)cardNum{
    //所需plist附件见文末
    NSString *plistPath = [[NSBundle mainBundle] pathForResource:@"BankAndBin" ofType:@"plist"];
    NSDictionary *dicBank = [[NSDictionary alloc] initWithContentsOfFile:plistPath];

    NSString *strPreTen = [self getPrefixStringToIndex:10 fromString:cardNum];//[cardNum substringToIndex:10];//前10位
    NSString *strPreNine = [self getPrefixStringToIndex:9 fromString:cardNum];//[cardNum substringToIndex:9];//前9位
    NSString *strPreEight = [self getPrefixStringToIndex:8 fromString:cardNum];//[cardNum substringToIndex:8];//8
    NSString *strPreSeven =[self getPrefixStringToIndex:7 fromString:cardNum];//[cardNum substringToIndex:7];//7
    NSString *strPreSix = [self getPrefixStringToIndex:6 fromString:cardNum];//[cardNum substringToIndex:6];//前6位
    NSString *strPreFive = [self getPrefixStringToIndex:5 fromString:cardNum];//[cardNum substringToIndex:5];//5
    NSString *strPreFour = [self getPrefixStringToIndex:4 fromString:cardNum];//[cardNum substringToIndex:4];//4
    NSString *strPreThree = [self getPrefixStringToIndex:3 fromString:cardNum];//[cardNum substringToIndex:3];//前三位

    NSString *strBankInfo = @"";
    if (dicBank[strPreThree]) {
        strBankInfo = dicBank[strPreThree];
    }else if (dicBank[strPreFour]){
        strBankInfo = dicBank[strPreFour];
    }else if (dicBank[strPreFive]){
        strBankInfo = dicBank[strPreFive];
    }else if (dicBank[strPreSix]){
        strBankInfo = dicBank[strPreSix];
    }else if (dicBank[strPreSeven]){
        strBankInfo = dicBank[strPreSeven];
    }else if (dicBank[strPreEight]){
        strBankInfo = dicBank[strPreEight];
    }else if (dicBank[strPreNine]){
        strBankInfo = dicBank[strPreNine];
    }else if (dicBank[strPreTen]){
        strBankInfo = dicBank[strPreTen];
    }else{
        strBankInfo = @"未找到匹配的发卡银行";
        return strBankInfo;
    }

    NSRange range =[strBankInfo rangeOfString:@" "];
    NSString *bankName = [strBankInfo substringToIndex:range.location];
    return bankName;
}
+(NSString *)getPrefixStringToIndex:(NSInteger)index fromString:(NSString *)strAll{
    if (strAll.length>=index) {
        return  [strAll substringToIndex:index];
    }else{
        return @" ";
    }
}
身份证号验证

+ (BOOL) validateIdentityCard: (NSString *)identityCard
{
    BOOL flag;
    if (identityCard.length <= 0) {
        flag = NO;
        return flag;
    }
    NSString *regex2 = @"^(\\d{14}|\\d{17})(\\d|[xX])$";
    NSPredicate *identityCardPredicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@",regex2];
    return [identityCardPredicate evaluateWithObject:identityCard];
}
车牌号验证

//车牌号验证
+ (BOOL) validateCarNo:(NSString *)carNo
{
    NSString *carRegex = @"^[\u4e00-\u9fa5]{1}[a-zA-Z]{1}[a-zA-Z_0-9]{4}[a-zA-Z_0-9_\u4e00-\u9fa5]$";
    NSPredicate *carTest = [NSPredicate predicateWithFormat:@"SELF MATCHES %@",carRegex];
    NSLog(@"carTest is %@",carTest);
    return [carTest evaluateWithObject:carNo];
}
