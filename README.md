# Widen Tag Guide

## 개요

> 웹 페이지에서 광고 컨버전을 기록하기 위한 Widen Tag 가이드 문서 입니다.

## 상세

와이든 태그는 랜딩페이지 도달, 페이지에서의 전환 체크를 위한 아래의 기능을 제공합니다.
이를 위해 랜딩, 전환 완료 페이지 (또는 둘이 같을 경우 랜딩페이지)에 지정된 이벤트를 호출하는 스크립트를 작성하여 주세요

| Name | Required | Description | Example |
| :--- | :--- | :--- | :--- |
| `tx_id` | O | Track ID | _XXXXXXXXXXXXXXXXXXXX_ |
| `e_n` | O | Event Name | _"OPEN"_, _"CONVERSION"_,  _"PURCHASE"_, _"PRE_REGISTRATION"_   |
| `e_v` |  | Event Value | string : `p_1234567`, JSON : `{"product_id": "p_1234567"}` |


##### 스크립트 삽입

클릭을 통해 도달한 랜딩 페이지에 아래의 스크립트를 작성하여 주세요.

```html
<script type="text/javascript">
    window._paq =window._paq || [];
    _paq.push(['e_n', "OPEN"]);

    !function(){var e=document.getElementsByTagName("script")[0],n=document.createElement("script");n.src="//cdn.widenlink.com/js/widentag.js",n.async=!0,n.defer=!0,e.parentNode.insertBefore(n,e)}();
</script>
```

### 전환 완료

회원가입, 구매전환 등 각각 전환 집계 시 구분을 원하시는 경우 Event Name을 달리하여 입력이 가능합니다.

#### 회원가입

> USER_ID와 같은 개인 식별 정보는 SHA1 Hasing된 값으로 보내는 것을 권장하며 
> 
> 동일 User가 2회 이상 전환 완료가 일어난 경우 최초 1회만 기록됩니다.

##### 스크립트 
```html
<script type="text/javascript" src="//cdn.widenlink.com/js/util.js" ></script>
<script type="text/javascript">
    var strUser = "USER_ID";

    window._paq =window._paq || [];
    _paq.push(['e_n', "CONVERSION"]);
    _paq.push(['e_v', SHA1(strUser)]);

    !function(){var e=document.getElementsByTagName("script")[0],n=document.createElement("script");n.src="//cdn.widenlink.com/js/widentag.js",n.async=!0,n.defer=!0,e.parentNode.insertBefore(n,e)}();
</script>
```

#### CPS

> CPS 전환 완료시,  추가적으로 필요한 상품 정보(가격, 수량 등)도 추가 가능합니다.

##### 스크립트
```html
<script type="text/javascript">
    window._paq =window._paq || [];
    _paq.push(['e_n', "PURCHASE"]);
    _paq.push(['e_v', "PRODUCT_ID");

    !function(){var e=document.getElementsByTagName("script")[0],n=document.createElement("script");n.src="//cdn.widenlink.com/js/widentag.js",n.async=!0,n.defer=!0,e.parentNode.insertBefore(n,e)}();
</script>
```
