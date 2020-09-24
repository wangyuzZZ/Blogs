# git 公钥生成/查看

## 查看

1.打开你的git bash 窗口

2.进入.ssh目录：cd ~/.ssh

3.找到id_rsa.pub文件：ls

4.查看公钥：cat id_rsa.pub 或者vim id_rsa.pub

## 生成

1.打开你的git bash 窗口

2.执行生成公钥和私钥的命令：ssh-keygen -t rsa 并按回车3下（为什么按三下，是因为有提示你是否需要设置密码，如果设置了每次使用Git都会用到密码，一般都是直接不写为空，直接回车就好了）。会在一个文件夹里面生成一个私钥 id_rsa和一个公钥id_rsa.pub。（可执行start ~ 命令，生成的公私钥在 .ssh的文件夹里面）

3.cat ~/.ssh/id_rsa.pub 