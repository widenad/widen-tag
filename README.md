# Widen Tag Guide

## 개요

> 웹 페이지에서 광고 컨버전을 기록하기 위한 Widen Tag 가이드 문서 입니다.

## 상세

> 와이든 태그 스크립트가 삽입되는 위치는 아래 2군데입니다.

- 랜딩 페이지
- 전환 완료 페이지

| Name | Required | Description | Example |
| :--- | :--- | :--- | :--- |
| `tx_id` | O | Track ID | _978454f85837562693186863104891c_ |
| `bid_id` | O | Bid ID | _bnr_4b58837562693363023872bfa4_ |
| `e_n` | O | Event Name | _"OPEN"_, _"CONVERSION"_,  _"PURCHASE"_  |
| `e_v` |  | Event Value | `PRODUCT_ID`, `USER_ID` |


### 랜딩
    
와이든애드가 광고를 식별하기 위한 파라미터는 **tx_id,** **bid_id** 두개 입니다.

브라우저를 통해 광고를 전달받을 경우엔 쿠키에 해당 데이터가 포함되어 있으며 

네이티브 앱을 통해 접근하는 경우 랜딩, 전환 완료 페이지에 도달하기까지 아래와 같은 URL로 해당 파라미터들이 전달되어야 합니다.

```
http://landing.page.com?tx_id=xxxxx&bid_id=yyyyy
```

##### 스크립트

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
