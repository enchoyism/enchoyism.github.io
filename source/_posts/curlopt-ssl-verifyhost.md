---
title: CURLOPT_SSL_VERIFYHOST explained
date: 2018-01-25 23:25:48
tags: [cURL, PHP]
---
# cURL options

| Options                | Description                                                                                                                                                                                                                                                                                |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CURLOPT_SSL_VERIFYPEER | FALSE to stop cURL from verifying the peer’s certificate. Alternate certificates to verify against can be specified with the CURLOPT_CAINFO option or a certificate directory can be specified with the CURLOPT_CAPATH option.                                                             |
| CURLOPT_SSL_VERIFYHOST | 1 to check the existence of a common name in the SSL peer certificate. 2 to check the existence of a common name and also verify that it matches the hostname provided. 0 to not check the names. In production environments the value of this option should be kept at 2 (default value). |

# Before: cURL Version < 7.28.0

`libcurl/7.19.7 NSS/3.14.0.0 zlib/1.2.3 libidn/1.18 libssh2/1.4.2`
위와 같이 NSS 라이브러리를 통해 빌드 하였다면 예컨데 다음과 같은 PHP 쓰임새를 보면

``` php
<?php
 // create curl resource
 $ch = curl_init();

 // set url
 curl_setopt($ch, CURLOPT_URL, “example.com”);

 //return the transfer as a string
 curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

 // stop cURL from verifying the peer’s certificate
 curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);

 // $output contains the output string
 $output = curl_exec($ch);

 // close curl resource to free up system resources
 curl_close($ch);
?>
```

peer의 인증서를 확인 하지 않는다는 옵션 0 (CURLOPT_SSL_VERIFYPEER) 일때
`CURLOPT_SSL_VERIFYHOST - verify the certificate's name against host`의 옵션은 DEFAULT=0이다.

# After: cURL Version >= 7.28.0

`libcurl/7.46.0 OpenSSL/1.0.2k zlib/1.2.3 nghttp2/1.6.0`

7.28.0 이후부터는 빌드시 NSS 라이브러리를 사용하지 않는데, 이때 CURLOPT_SSL_VERIFYHOST 의 옵션은 DEFAULT=2이다.
따라서 기존처럼 사용시 http code가 000으로 리턴되고 **SSL: no alternative certificate subject name matches target host name** 와 같은
에러 메시지로 정상동작하지 않는다면 반드시 CURLOPT_SSL_VERIFYPEER 옵션 명시할 것을 검토하여야 한다.
인증서 검증시 정상이거나 OS가 내장하고 있는 번들인증서가 최신이라면 오류날 가능성이 적으므로, 오류가 나지 않을 수도 있어, 반드시 테스트를 해봐야 정확히 파악이 가능하다.

CURLOPT_SSL_VERIFYPEER, CURLOPT_SSL_VERIFYHOST 두 값을 false로 명시하는것은 사실 보안상 좋진 않지만
장애가 날 경우를 대비하는 관점에서 명시하여 사용하는게 좋을 수 있다.

# Reference

*   [https://curl.haxx.se/libcurl/c/CURLOPT_SSL_VERIFYHOST.html](https://curl.haxx.se/libcurl/c/CURLOPT_SSL_VERIFYHOST.html)
