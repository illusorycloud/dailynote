

## 1.��¡Զ����Ŀ

```java


git clone git@github.com:lillusory/ssm-crm.git

//����ֿ��ļ���

cd D:\lillusory\MyProject\lillusory.github.io

�������ؿ��GitHubԶ�̿�

���Ƴ�ԭ�еĹ���

git remote rm origin 

������µ�

git remote add origin git@github.com:lillusory/lillusory.github.io.git

git remote add origin git@github.com:lillusory/ssm-crm.git

�������ļ���ӵ����ػ������ύ�����ط�֧

git add .

git commit -m "edit post"

//�ύ

git push -u origin master

//��Զ�ֿ̲����������

git pull origin master
```

## 2.������Ϣ

```java

�޸�ͼƬλ��

�ֿ��е�λ�ã�https://github.com/lillusory/lillusory.github.io/blob/master/images/posts/java/java-base-learning-one.jpg

��blob��Ϊraw������markdown����ʾ

https://github.com/lillusory/lillusory.github.io/raw/master/images/posts/java/java-base-learning-one.jpg

�����û���������

git config --global user.name "lillusory"

git config --global user.email "xueduanli@163.com"

�鿴���õ���Ϣ

git config -l

������һЩGithub���õ����

�л���֧��git checkout name

�����޸ģ�git checkout -- file

ɾ���ļ���git rm file

�鿴״̬��git status

��Ӽ�¼��git add file �� git add .

���������git commit -m "miao shu nei rong"

ͬ�����ݣ�git pull

�ύ���ݣ�git push origin name

��֧����

�鿴��֧��git branch

������֧��git branch name

�л���֧��git checkout name

����+�л���֧��git checkout -b name

�ϲ�ĳ��֧����ǰ��֧��git merge name

ɾ����֧��git branch -d name

ɾ��Զ�̷�֧��git push origin :name

```

## 3.�л���֧����

```java
1. ������Ŀ��Ĭ����master��֧ ������֧ Ӧ��֤�÷�֧������Զ����ȷ�ģ������е�
2.����ʱһ�����ݹ��ܴ��������֧ 
//git branch XXX ������֧XXX git checkout XXX �л�����֧XXX
//����git checkout -b XXX �������л�����֧XXX
3.���������кϲ� merge �л���master��֧ git merge XXX ��XXX��֧�ϲ�����ǰ��֧����master��֧��
4.�ϲ���ɺ󼴿�ɾ������ʱ�����ķ�֧ git branch -d XXX ɾ����֧XXX
```



## 4.��������

#### 1���½������

```
# �ڵ�ǰĿ¼�½�һ��Git�����
 git init
# �½�һ��Ŀ¼�������ʼ��ΪGit�����
git init [project-name]
# ����һ����Ŀ����������������ʷ
git clone [url]
```

####    2���鿴�ļ�״̬

```
#�鿴ָ���ļ�״̬
git status [filename]
#�鿴�����ļ�״̬
git status
```

####     3��������<-->�ݴ���

```
# ���ָ���ļ����ݴ���
git add [file1] [file2] ...
# ���ָ��Ŀ¼���ݴ�����������Ŀ¼
git add [dir]
# ��ӵ�ǰĿ¼�������ļ����ݴ���
git add .
#��������Ҫɾ���ݴ������֧�ϵ��ļ�, ͬʱ������Ҳ����Ҫ����ļ���, ����ʹ�ã�??��
git rm file_path
#��������Ҫɾ���ݴ������֧�ϵ��ļ�, ����������Ҫʹ��, ���ʱ��ֱ��push�Ǳ�����ļ���û�У����push֮ǰ����add��ô���ǻ��С�
git rm --cached file_path
#ֱ�Ӽ��ļ���   ���ݴ������ļ��ָ���������������������Ѿ��и��ļ������ѡ�񸲸�
#���ˡ���֧���� +�ļ���  ���ʾ�ӷ�֧��Ϊ��д�ķ�֧������ȡ�ļ� �����ǹ���������ļ�
git checkout
```

####     4��������<-->��Դ�⣨�汾�⣩

```
#���ݴ���-->��Դ�⣨�汾�⣩
git commit -m '�ô��ύ˵��'
#�������:������Ҫ���ļ�commit ���� �ϴ��ύ�����Ǵ��  ���� ����ı��ݴ������ݣ�ֻ��������ύ����Ϣ
#�Ƴ�����Ҫ����ӵ��ݴ������ļ�
git reset HEAD �ļ���
#ȥ����һ�ε��ύ����ֱ�ӱ��add֮ǰ״̬��   
git reset HEAD^ 
#ȥ����һ�ε��ύ�����add֮��commit֮ǰ״̬�� 
git reset --soft  HEAD^ 
```

####     5��Զ�̲���

```
# ȡ��Զ�ֿ̲�ı仯�����뱾�ط�֧�ϲ�
git pull
# �ϴ�����ָ����֧��Զ�ֿ̲�
git push
```

####    6��������������

```
# ��ʾ��ǰ��Git����
git config --list
# �༭Git�����ļ�
git config -e [--global]
#����commit֮ǰ����Ҫ�����û����估�û�����ʹ���������
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
#����Git�İ����ĵ�
git --help
#�鿴ĳ����������İ����ĵ�
git +���� --help
#�鿴git�İ汾
git --version
```

git reset �Csoft ����ı�stage����������commit���˵���ָ�����ύ 
git reset �Cmixed ���ظı乤���������ǻ���ָ����commit����stage ����֮ǰ�����ݴ�����ݶ���ΪΪ�ݴ��״̬ 

git reset �Chard ʹ��ָ����commit�����ݸ���stage���͹�������