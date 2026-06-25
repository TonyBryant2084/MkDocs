# SSH

1. 生成 SSH 加密密钥对（私钥 + 公钥）

```bash
ssh-keygen -t ed25519 -C "tonybryant2084@gmail.com"

1. ssh-keygen: 系统自带工具, 专门生成、管理 SSH 加密密钥对（私钥 + 公钥）
2. -t: type, 指定加密算法类型（椭圆曲线加密算法: ed25519）
3. -C: comment, 给这组密钥添加一段备注文字（备注文字: "tonybryant2084@gmail.com"）
```

2. 保存密钥的文件路径（默认：C:\Users\32413/...）

```bash
Enter file in which to save the key (C:\Users\32413/.ssh/id_ed25519): 
```

3. 密钥解锁密码

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```

4. 完整验证流程

   1. 前置准备

      1. 本地生成一对密钥：私钥（电脑存）、公钥（一串文本）

      2. 你把完整公钥粘贴到自己 GitHub 账号后台保存

         GitHub 数据库建立映射：`这条公钥 → 你的账号TonyBryant2084`

   2. 第 1 步：本地发起 SSH 连接（ssh -T git@github.com）

      你的电脑主动做两件事：

      1. 读取本地 `id_ed25519.pub` 公钥，计算出一段短 SHA256 指纹；
      2. 把**公钥指纹**发给 GitHub 服务器。

   3. 第 2 步：GitHub 靠指纹快速定位你的账号（这一步只确定 “来人对应的是哪个账号”，还没验证是不是本人。）

      1. GitHub 收到指纹，拿它去哈希索引表一键查找；
      2. 查到：这个指纹对应的完整公钥，存在你的 TonyBryant2084 账号下；
      3. 从你的账号里取出当初你上传的完整公钥，备用。

   4. 第 3 步：GitHub 发随机挑战暗号，验证所有权

      GitHub 生成一串随机字符（挑战码），用你的公钥加密，发回给你电脑。

      只有配套私钥，才能解开这个加密暗号。

   5. 第 4 步：本地私钥解密、签名回传证明身份

      1. 电脑用本地私钥解开加密的随机暗号；
      2. 用私钥给解开后的暗号做数字签名；
      3. 把签好名的暗号发回 GitHub。

   6. 第 5 步：最终匹配校验

      GitHub 拿你账号里存的公钥，校验你发回来的签名：

      1. 校验成功：确认你手里持有对应私钥，判定是账号本人，输出 `Hi TonyBryant2084!`，连接成功；
      2. 校验失败：签名对不上，直接拒绝连接，提示权限错误。

