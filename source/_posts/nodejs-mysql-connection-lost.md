---
title: Node.js + MySQL Connection lost
date: 2018-02-02 23:59:10
tags: [Node.js, MySQL]
---
**해당 포스트는 Connection Pool을 사용시 release 또는 end등, idle timeout 등의 설정 및 처리가 완벽하다는 전제하에 작성되었다.**
**`Error: Connection lost: The server closed the connection.`** Node.js와 MySQL을 이용한 개발 및 운영시 해당 에러를 뱉어내고 죽는 경우가 있다. 다른 언어의 플랫폼 및 프레임워크에서는 해당 사항에 대하여 어떤식으로 해결하고 있는지는 정확히 모르겠으나 적어도 Node.js + MySQL의 경우 구글링을 하여도 근본적인 원인에 대한 자세한 설명보다 이해하지 못하면 미봉책에 가까운 코드만 나열되어 있는 것 같아 문제에 대한 원인과 해결 방법을 기술한다.

# Node.js

먼저 쉽게 찾아 볼 수 있는 해결책에 가까운 코드를 찾아보면 2가지 경우가 있다.

``` javascript
// Sol 1.
‘use strict’;

const mysql = require(‘mysql’);

let conn;

const createConn = () => {
	conn = mysql.createConnection({
		host: ‘localhost’, user: ‘root’,
		password: ‘p@ssw0rd’, database: ‘example’
	});

	conn.connect((error) => {
		if (error) {
			console.log(connecting error: <span class="subst">${error}</span>);
			setTimeout(createConn, 2000);
		}
	});

	conn.on(‘error’, (error) => {
		if (error.code === ‘PROTOCOL_CONNECTION_LOST’) {
			return createConn();
		}

		throw error;
	});
};

createConn();
```

``` javascript
// Sol 2.
setInterval(function () {
 	db.query(‘SELECT 1’);
}, 5000);
```

위의 두가지 경우는 모두 단일 커넥션의 경우이다. 첫번째는 연결이 끊어졌을때 커넥션을 다시 연결하는 경우이고, 두번째는 일정시간동안 쿼리를 날려 커넥션을 강제로 유지하는 방법인데 Connection Pool을 사용한다면 Pool의 커넥션 자원을 모두 처리하기에는 어려움이 있어, 차라리 새로운 Pool을 생성하여 사용하는것이 더 좋다고 생각한다. 하지만 이 마저도 자원 낭비가 되기 때문에 근본적인 해결 방법은 아니라고 생각한다.

# MySQL(my.cnf)

![nodejs-mysql-connection-lost-1](/images/nodejs-mysql-connection-lost-1.png)
먼저 MySQL 또는 MariaDB를 구동후 위와 같이 Variables를 조회 하였을때 해당 문제를 해결하기 위해 주목해야 할 부분은 다음과 같다. **`interactive_timeout(콘솔 또는 터미널)`**, **`wait_timeout()`** 해당 의미는 모두 활동하지 않는 커넥션을 끊을때까지 서버가 대기하는 시간인데 위 설정의 경우는 3600초로 1시간동안 활동이 없으면 서버가 커넥션을 닫아버린다. 즉, 트래픽이 얼마 없는 서비스이거나 새벽시간대에 주로 에러를 뱉는 경우가 많다. 따라서 my.cnf에서 서비스에 적정한 값으로 수정후 해당 DB 데몬을 재실행 하여야 한다.

``` bash
… 중략
[mysqld]
wait_timeout            = {적정한 값}
interactive_timeout     = {적정한 값}
… 중략
```

# Reference

*   [http://www.linuxchannel.net/docs/mysql-timeout.txt](http://www.linuxchannel.net/docs/mysql-timeout.txt)
*   [https://gist.github.com/hanjong/1205199](https://gist.github.com/hanjong/1205199)
*   [https://stackoverflow.com/questions/20210522/nodejs-mysql-error-connection-lost-the-server-closed-the-connection](https://stackoverflow.com/questions/20210522/nodejs-mysql-error-connection-lost-the-server-closed-the-connection)
