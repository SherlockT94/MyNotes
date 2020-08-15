# XPath

    XPath - A Way to select elements in xml file
    
    
| Expression | Description                                                                                           |
|------------|-------------------------------------------------------------------------------------------------------|
| nodename   | Selects all nodes with the name "nodename"                                                            |
| /          | Selects from the root node                                                                            |
| //         | Selects nodes in the document from the current node that match the selection no matter where they are |
| .          | Selects the current node                                                                              |
| ..         | Selects the parent of the current node                                                                |
| @          | Selects attributes                                                                                    |

* 若验证数据存放在XML文件中，可能存在XPath注入，其原理是通过查找user表中的用户名（username）和密码（password）的结果来进行授权访问。
 
 
    Username: blah' or 1=1 or 'a'='a
    Password: blah

    FindUserXPath becomes //Employee[UserName/text()='blah' or 1=1 or
            'a'='a' And Password/text()='blah']

    Logically this is equivalent to:
            //Employee[(UserName/text()='blah' or 1=1) or
            ('a'='a' And Password/text()='blah')] 
 
* In this case, only the first part of the XPath needs to be true. The password part becomes irrelevant, and the UserName part will match ALL employees because of the “1=1” part. JUST LIKE THE SQL INJECTION!!! [OWASP - XPath Injection](https://owasp.org/www-community/attacks/XPATH_Injection)
