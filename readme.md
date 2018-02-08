### aliyun-sms

```
<?php

class Sms
{
    /**
     * 取得AcsClient
     *
     * @return DefaultAcsClient
     */
    public static function getAcsClient()
    {
        // 加载区域结点配置
        Config::load();

        //产品名称:云通信流量服务API产品,开发者无需替换
        $product = "Dysmsapi";

        //产品域名,开发者无需替换
        $domain = "dysmsapi.aliyuncs.com";

        // TODO 此处需要替换成开发者自己的AK (https://ak-console.aliyun.com/)
        $accessKeyId = "LTAIddLLdAcGPooj";
        $accessKeySecret = "6h6hyI38WDHfbZm37Hhovhjr6VY8eZ";

        // 暂时不支持多Region
        $region = "cn-hangzhou";

        // 服务结点
        $endPointName = "cn-hangzhou";

        // 初始化acsClient,暂不支持region化
        $profile = DefaultProfile::getProfile($region, $accessKeyId, $accessKeySecret);

        // 增加服务结点
        DefaultProfile::addEndpoint($endPointName, $region, $product, $domain);

        // 初始化AcsClient用于发起请求
        $acsClient = new DefaultAcsClient($profile);
        return $acsClient;
    }

    /**
     * 发送短信
     * @param $PhoneNumbers
     * @param $SignName
     * @param $TemplateCode
     * @param $TemplateParam
     * @return mixed|\SimpleXMLElement
     */
    public static function send($PhoneNumbers, $SignName, $TemplateCode, $TemplateParam)
    {
        // 初始化SendSmsRequest实例用于设置发送短信的参数
        $request = new SendSmsRequest();

        // 必填，设置短信接收号码
        $request->setPhoneNumbers($PhoneNumbers);

        // 必填，设置签名名称，应严格按"签名名称"填写
        // 请参考: https://dysms.console.aliyun.com/dysms.htm#/develop/sign
        $request->setSignName($SignName);

        // 必填，设置模板CODE，应严格按"模板CODE"填写
        // 请参考: https://dysms.console.aliyun.com/dysms.htm#/develop/template
        $request->setTemplateCode($TemplateCode);

        // 可选，设置模板参数, 假如模板中存在变量需要替换则为必填项
        // 短信模板中字段的值
        $request->setTemplateParam(json_encode($TemplateParam, JSON_UNESCAPED_UNICODE));

        // 可选，设置流水号
        // $request->setOutId("yourOutId");

        // 选填，上行短信扩展码（扩展码字段控制在7位或以下，无特殊需求用户请忽略此字段）
        // $request->setSmsUpExtendCode("1234567");

        // 发起访问请求
        $acsResponse = static::getAcsClient()->getAcsResponse($request);

        return $acsResponse;
    }
}
```