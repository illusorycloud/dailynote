```
1. ����git�û���������
   ����Git��user name��email��

git config --global user.name "lillusory"

git config --global user.email "xueduanli@163.com"

git config --global user.name "lixueduan"

git config --global user.email "lixueduan@cqkct.com"

1. ����ssh key
   ������Կ��
   ssh-keygen -t rsa -C "xueduanli@163.com"
   ��3���س�������Ϊ�ա�
2. �ϴ�key��github
   clip < ~/.ssh/id_rsa.pub

����key��������

��¼github

������Ϸ���Accounting settingsͼ��

ѡ�� SSH key

��� Add SSH key

1. �����Ƿ����óɹ�
   ssh -T git@github.com

������óɹ��������ʾ�� Hi username! You��ve successfully authenticated, but GitHub does not provide shell access.



```

