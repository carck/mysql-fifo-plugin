mysql-fifo-plugin
=================

MySQL Pipes (FIFOs) Plugin

Build
-----
	gcc -O3  -g  -I/usr/include/mysql -I/usr/include  -fPIC -lm -lz -shared -o fifo.so fifo.c
	sudo mv fifo.so /usr/lib/mysql/plugin/
	
Plugin Install and Uninstall
--------------

	drop function fifo_create;
	drop function fifo_remove;
	drop function fifo_read;
	drop function fifo_write;

	create function fifo_create returns string soname 'fifo.so';
	create function fifo_remove returns string soname 'fifo.so';
	create function fifo_read returns string soname 'fifo.so';
	create function fifo_write returns string soname 'fifo.so';
			
Testing
-------
### �����ܵ�	
	mysql> create function fifo_create returns string soname 'fifo.so';
	Query OK, 0 rows affected (0.02 sec)

	mysql> select fifo_create('/tmp/myfifo');
	+----------------------------+
	| fifo_create('/tmp/myfifo') |
	+----------------------------+
	| ture                       |
	+----------------------------+
	1 row in set (0.00 sec)
	
	�鿴�ܵ��Ƿ񴴽�
	
	$ ls /tmp/myfifo 
	/tmp/myfifo
	
	���ǲ��ԣ���ȷӦ�÷���false
	
	mysql> select fifo_create('/tmp/myfifo');
	+----------------------------+
	| fifo_create('/tmp/myfifo') |
	+----------------------------+
	| false                      |
	+----------------------------+
	1 row in set (0.00 sec)

### ɾ���ܵ�	
	mysql> select fifo_remove('/tmp/myfifo');
	+----------------------------+
	| fifo_remove('/tmp/myfifo') |
	+----------------------------+
	| true                       |
	+----------------------------+
	1 row in set (0.00 sec)
	
	mysql> select fifo_remove('/tmp/my');
	+------------------------+
	| fifo_remove('/tmp/my') |
	+------------------------+
	| false                  |
	+------------------------+
	1 row in set (0.00 sec)

	ɾ�������ڵĹܵ�����ʾ false

### ���ܵ�

	��һ���ն˴���������
	mysql> select fifo_read('/tmp/myfifo');
	+--------------------------+
	| fifo_read('/tmp/myfifo') |
	+--------------------------+
	| Hello world              |
	+--------------------------+
	1 row in set (7.85 sec)

	����һ���ն˴���������
	mysql> select fifo_write('/tmp/myfifo','Hello world !!!');
	+---------------------------------------------+
	| fifo_write('/tmp/myfifo','Hello world !!!') |
	+---------------------------------------------+
	| true                                        |
	+---------------------------------------------+
	1 row in set (0.00 sec)	
	
	����
	
	������������
	$ echo "Hello world" > /tmp/myfifo
	
	��SQL�ͻ���������
	mysql> select fifo_read('/tmp/myfifo');
	+--------------------------+
	| fifo_read('/tmp/myfifo') |
	+--------------------------+
	| Hello world
				 |
	+--------------------------+
	1 row in set (0.00 sec)
	ע������echo���Զ����ӻ��з���-n�������Ա���
	$ echo -n "Hello world" > /tmp/myfifo
	
	mysql> select fifo_read('/tmp/myfifo');
	+--------------------------+
	| fifo_read('/tmp/myfifo') |
	+--------------------------+
	| Hello world              |
	+--------------------------+
	1 row in set (0.01 sec)
	
### д�ܵ�
	mysql> select fifo_write('/tmp/myfifo','Hello world !!!');
	+---------------------------------------------+
	| fifo_write('/tmp/myfifo','Hello world !!!') |
	+---------------------------------------------+
	| true                                        |
	+---------------------------------------------+
	1 row in set (0.00 sec)
	
	$ cat /tmp/myfifo
	Hello world !!!
	