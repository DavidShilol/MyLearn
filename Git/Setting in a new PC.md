# Setting in a new PC

+ git config [--global] user.name "your name"

  git config [--global] user.email "your email"

+ git remote add ***local name*** ***github project url***

  Example: git remote add origin http://xxx/yyy/zzz/qqq.git.

  Note: 使用ssh连接，可以避免每次push都要输入用户名和密码；HTTP连接则需要。

+ ssh-keygen -t rsa -C "your email" (same with step 1 email)

  生成的pub公钥放在github账号的ssh设置里

+ ssh -T git@github.com

  测试连接

+ 可以开始 pull 和 push了

