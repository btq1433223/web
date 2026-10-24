# web
practise

SQL注入复现
进入 SQL Injection 模块。
输入 1，正常返回。

<img width="462" height="183" alt="aad2fcdab73678df52564b8df707ce6e" src="https://github.com/user-attachments/assets/93876d84-34f7-43a6-b582-9be77903651a" />

输入 1' OR '1'='1，返回所有用户信息。

<img width="468" height="514" alt="d4bbd7665bdc0d28af647171ccc5ae9f" src="https://github.com/user-attachments/assets/720b514f-4661-4ba9-bddc-31a01a505b97" />

输入 1' UNION SELECT user, password FROM users #，成功爆出账号密码哈希。

<img width="612" height="619" alt="a1de661cf8171751e5d0b6b288541d52" src="https://github.com/user-attachments/assets/e1354aa6-e65d-4838-a18a-3992b7b3734e" />

XSS（跨站脚本）复现：
进入 XSS (Reflected) 模块。
输入 <script>alert('XSS攻击')</script>，浏览器弹窗。

<img width="644" height="151" alt="d9dd8b416efe49f988b5bb612a071635" src="https://github.com/user-attachments/assets/ea01a4a2-20b0-44ba-a424-2845d13bca5c" />

文件上传复现：
进入 File Upload 模块，准备一个 1.php（内容为 <?php phpinfo(); ?>）。

<img width="532" height="207" alt="22c9f512f62782400ffd297ac69a184b" src="https://github.com/user-attachments/assets/0e29b90d-14bb-4376-be97-ea112d5a4a4c" />

将其伪装成图片或直接上传，上传成功后在浏览器访问该文件路径，执行 PHP 代码。

<img width="1433" height="857" alt="1d82e8f40de6d07dd03a1a1aafe4e655" src="https://github.com/user-attachments/assets/b3ba3151-04f2-4f4d-985f-5ff131eff8e6" />

漏洞修复
修复 SQL 注入：
因为 user_id 本来就应该是个数字，所以你只要把用户输入强制转换成整数即可。
修改位置：
找到 $id = $_REQUEST[ 'id' ]; 这一行，改成：$id = (int)$_REQUEST[ 'id' ];

<img width="274" height="43" alt="19b161939ae9236e0e9fe0f330492559" src="https://github.com/user-attachments/assets/aa6a86e1-989c-48ce-8587-e8ebea6e29b3" />

再次输入 1' OR '1'='1，此时页面应该只会返回 ID 为 1 的结果，不再列出所有用户。

<img width="425" height="157" alt="7090660147050d6fd41ffba4443feedf" src="https://github.com/user-attachments/assets/4d60c369-8600-4a9f-8e7f-facf70f1d9bc" />

修复XSS（跨站脚本）：
$html .= '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';
输入的 $_GET['name'] 直接拼接到了 HTML 中，没有任何过滤，导致浏览器会把它当成代码执行。
将原来那行代码，替换成下面

<img width="856" height="152" alt="625b978ffb5204bb57e0911a17f49f8c" src="https://github.com/user-attachments/assets/e92d23dd-1543-49b1-b7b3-ce9ea1518190" />

回到网页刷新，输入 <script>alert('xss')</script>。
你会发现弹窗没有了，页面上只会原样显示Hello <script>alert('xss')</script></pre> 这行字。

<img width="563" height="117" alt="5c8f2036532783d1807e61e3deb68bf3" src="https://github.com/user-attachments/assets/a2983e61-c92a-4297-9cd2-f7038fe7cd88" />
