

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



