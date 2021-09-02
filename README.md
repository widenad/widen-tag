# Widen Tag Guide

## Change Log
| 버전 | 날짜 | 변경사항 |
| :--- | :--- | :--- |
| 1.0 | 2021-08-01 | 최초 버전 배포 |

## 개요

> WidenAd의 광고 클릭을 통한 HTML로 작성된 웹 사이트에서의 컨버전 체크 스크립트인 Widen Tag의 가이드 문서 입니다.

## 상세

> Widen Tag 는 사이트 도달, 전환을 체크 기능을 제공합니다.
이를 위해 랜딩, 전환 완료 페이지 (또는 둘이 같을 경우 랜딩페이지)에 지정된 이벤트를 호출하는 스크립트를 작성하여 주세요.

| Name | Required | Description | Example |
| :--- | :--- | :--- | :--- |
| `tx_id` | O | Track ID | _XXXXXXXXXXXXXXXXXXXX_ |
| `e_n` | O | Event Name | _"OPEN"_, _"CONVERSION"_,  _"PURCHASE"_, _"PRE_REGISTRATION"_   |
| `e_v` |  | Event Value | string : `p_1234567`, JSON : `{"product_id": "p_1234567"}` |

### 랜딩 페이지
클릭을 통해 도달한 랜딩 페이지에 아래의 스크립트를 작성하여 주세요.

##### Code

```html
<script type="text/javascript">
    window._wztg = window._wztg || [];
    _wztg.push(['e_n', "OPEN"]);

    !function(){var e=document.getElementsByTagName("script")[0],n=document.createElement("script");n.src="//cdn.widenlink.com/js/widentag.js",n.async=!0,n.defer=!0,e.parentNode.insertBefore(n,e)}();
</script>
```

### 전환

사전예약, 회원가입, 상품구매 등 전환 체크를 위한 페이지에 아래의 스크립트 작성하여 주세요.
전환 이벤트별 샘플은 아래 캠페인 섹션을 참조해주세요.
 
> 전환 완료시 포함되는 개인 정보는 SHA1 Hash 또는 암호화 된 값을 보내는 것을 권장합니다.


##### Code 

| Template | Nullable | Description | 
| --- | --- | --- |
| {$userInfo} | NOT NULL | 전화번호 또는 이메일 등 사용자를 식별할 수 있는 개인 정보 |

```html
<script type="text/javascript" src="//cdn.widenlink.com/js/util.js" ></script>
<script type="text/javascript">
    var strValue = '${userInfo}';

    window._wztg = window._wztg || [];
    _wztg.push(['e_n', "CONVERSION"]);
    _wztg.push(['e_v', sha1(strUser)]);

    !function(){var e=document.getElementsByTagName("script")[0],n=document.createElement("script");n.src="//cdn.widenlink.com/js/widentag.js",n.async=!0,n.defer=!0,e.parentNode.insertBefore(n,e)}();
</script>
```

---
## 캠페인

### 1. 사전예약 켐페인

> 사전예약 전환 완료시, 사용자를 식별할 수 있는 식별자가 포함되어 있어야 합니다.

##### 스크립트

```html
<script type="text/javascript" src="//cdn.widenlink.com/js/util.js" ></script>
<script type="text/javascript">
    var strUser = "${userInfo}";
    
    window._wztg = window._wztg || [];
    _wztg.push(['e_n', "PRE_REGISTRATION"]);
    _wztg.push(['e_v', encrypt(strUser)]);

    !function(){var e=document.getElementsByTagName("script")[0],n=document.createElement("script");n.src="//cdn.widenlink.com/js/widentag.js",n.async=!0,n.defer=!0,e.parentNode.insertBefore(n,e)}();
</script>
```

### 2. CPS 켐페인

> CPS 전환 완료시, 추가적으로 필요한 상품 정보(가격, 수량 등)도 추가 가능합니다.

##### 스크립트

| Template | Nullable | Description | 
| --- | --- | --- |
| {$itemId} | NOT NULL | 결제된 상품 ID (상품코드). <br/>이때의 상품ID는 상품상세 페이지에서 전달한 상품ID와 동일한 것이어야 한다. |
| {$productPrice} | NOT NULL | 결제된 상품의 가격 |
| {$productQuantity} | NOT NULL | 결제된 상품의 갯수 |
| {$orderId} | NOT NULL | 결제완료된 주문 ID |
| {$orderPrice} | NOT NULL | 결제완료된 주문의 가격 |
```html
<script type="text/javascript" src="//cdn.widenlink.com/js/util.js" ></script>
<script type="text/javascript">
    var orderInfo =
            {
                "order_id":"${orderId}",
                "order_price":"${orderPrice}",
                "products":[{
                    "id":"${itemId}",
                    "price":"${productPrice}",
                    "quantity":"${productQuantity}"
                },{
                    "id":"${itemId}",
                    "price":"${productPrice}",
                    "quantity":"${productQuantity}"
                }]
            };
    
    window._wztg = window._wztg || [];
    _wztg.push(['e_n', "PURCHASE"]);
    _wztg.push(['e_v', orderInfo);

    !function(){var e=document.getElementsByTagName("script")[0],n=document.createElement("script");n.src="//cdn.widenlink.com/js/widentag.js",n.async=!0,n.defer=!0,e.parentNode.insertBefore(n,e)}();
</script>
```
