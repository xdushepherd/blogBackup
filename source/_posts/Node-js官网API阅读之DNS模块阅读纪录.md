title: Node.js官网API阅读之DNS模块鱼肚纪录
date: 2015-11-14 01:05:43
tags: [DNS，域名系统]
categories: [node.js]
---

###概述
使用require('dns')来获取这个模块。这个模块包含了分别属于两个类别的函数。

第一种，使用操作系统提供的一些服务来完成域名解析的工作，不需要任何网络交流工作。这个目录包含一个函数，那就是dns.lookup()。
这是一个解析www.google.com网址的例子
```javascript
	var dns = require('dns');

	dns.lookup('www.google.com', function onLookup(err, addresses, family) {
	  console.log('addresses:', addresses);
	});
```
<!-- more -->

第二种，连接到真实DNS服务器来完成域名解析工作的函数，并且这些函数总是使用网络来完成DNS查询。这个类别包含了出国dns.lookup()以外的所有其他函数。这些函数使用的配置项目与dns.lookup()不同。举个例子，这些函数不使用来这/etc/hosts。这些函数会被那些不想使用系统提供的域名解析方法的开发者使用，他们倾向于使用DNS查询来完成域名解析。

下面是另一个解析'www.google.com'域名的例子。

```javascript
	var dns = require('dns');

	dns.resolve4('www.google.com', function (err, addresses) {
	  if (err) throw err;

	  console.log('addresses: ' + JSON.stringify(addresses));

	  addresses.forEach(function (a) {
	    dns.reverse(a, function (err, hostnames) {
	      if (err) {
	        throw err;
	      }

	      console.log('reverse for ' + a + ': ' + JSON.stringify(hostnames));
	    });
	  });
	});
```

###dns.lookup(hostname[.options],callback)

解析hostname，得到其IPV4或者IPV6地址纪录。options可以是一个对象也可以是一个整数。如果options没有传入，那么IPV4和IPV6都是合法的地址。如果options是一个整数，那就必须是4或者6.

可选地，options剋一事一个包含三个属性的对象：
	familyP{Number}-family纪录。如果存在，那它的值就应该是4或者6，如果不存在，那么IPV4和IPV6都被接受。
	hints:{Number}－如果存在，那他必须是一个或者多个被支持的getaddrinfo标志。如果没有出现，那么就不会有标志被传递给getaddrinfo。
	all:{ 布尔值 }-如果为true，那么回调函数会把所有的地址返回到一个数组中，否则返回单一一个地址。默认为false。
所有的属性都是可选的。下面是一个小例子:
```javascript
	{
	  family: 4,
	  hints: dns.ADDRCONFIG | dns.V4MAPPED,
	  all: false
	}
```

回调函数有三个参数callback(err,address,family)。address时一个代表IP地址的字符串。family时整数4或者6和地址家族的标记。

如果options的all属性被设置了，那么参数就变成。callback(err,addresses),addresses时一个带有address和family的对象的数组。

如果出现错误，erro就是一个Error实例对象。请记住，当域名不存在或者当查询失败的时候，err.code都会被设置成'ENOENT'。

###dns.lookupService(address,port,callback)

解析给定的IP地址及端口号，返回起域名和服务。

回调函数有三个参数，分别是err,hostname,service。域名和服务的参数是字符串(比如说，localhost，和,http)。

当有错误的时候，err是一个Error对象，err.code是错误代码号。

###dns.resolve(hostname[,rrtype],callback)
解析一个域名为一个被rrtype指定的record类型数组。
有效的rrtypes如下:
'A': (IPV4地址，默认)
'AAAA'(IPV6地址)
'MX'(邮件交换纪录)
'TXT'(文本纪录)
'SRV'(SRV纪录)
'PTR'(used for reverse IP lookups)
'NS'(name server records)
'CNAME'(canonical name records)
'SOA'(start of authority record)

###dns.resolve4(hostname,callback)
和dns.resolve(),但是只有IPV4查询(A纪录)。地址存在一个IPV4地址数组。比如[74.125.79.104,'74.125.79.105','74.125.79.106']。

###dns.resolve6(hostname,callback)
和dna.resolve4()一样，只不过是做IPV6查询的。（AAAA查询）

类似的还有dns.resolveMx(),dns.resolveTxt(hostname,callback),dns.resolveSrv(hostname,callback),dns.resoveSoa(hostname,callback),dns.resolveNs(hostname,callback),dns.resoveCname(hostname,callback)

###dns.reverse(ip,callback)

根据ip地址解析出IP的域名数组。回调函数有一组参数(err,hostnames)。当有错误出现的时候，err是一个Error对象。err.code会给出错误代码。

###dns.getServers()

返回当前使用的IP地址的字符串的数组。

###dns.setServers(servers)

给定一个字符串形式的IP地址数组，将这些地址设置成解析域名的服务地址。
如果你给IP地址指定了一个端口号，那么可能会出错，因为模块并不支持这些。

这里会抛出异常，如果你传递不合法的输入值。

###Error codes
dns.NODATA: DNS server returned answer with no data.
dns.FORMERR: DNS server claims query was misformatted.
dns.SERVFAIL: DNS server returned general failure.
dns.NOTFOUND: Domain name not found.
dns.NOTIMP: DNS server does not implement requested operation.
dns.REFUSED: DNS server refused query.
dns.BADQUERY: Misformatted DNS query.
dns.BADNAME: Misformatted hostname.
dns.BADFAMILY: Unsupported address family.
dns.BADRESP: Misformatted DNS reply.
dns.CONNREFUSED: Could not contact DNS servers.
dns.TIMEOUT: Timeout while contacting DNS servers.
dns.EOF: End of file.
dns.FILE: Error reading file.
dns.NOMEM: Out of memory.
dns.DESTRUCTION: Channel is being destroyed.
dns.BADSTR: Misformatted string.
dns.BADFLAGS: Illegal flags specified.
dns.NONAME: Given hostname is not numeric.
dns.BADHINTS: Illegal hints flags specified.
dns.NOTINITIALIZED: c-ares library initialization not yet performed.
dns.LOADIPHLPAPI: Error loading iphlpapi.dll.
dns.ADDRGETNETWORKPARAMS: Could not find GetNetworkParams function.
dns.CANCELLED: DNS query cancelled.

###支持的getaddrinfo标志
下面的标志可以被传递给dns.lookup()函数。

dns.ADDRCONFIG: Returned address types are determined by the types of addresses supported by the current system. For example, IPv4 addresses are only returned if the current system has at least one IPv4 address configured. Loopback addresses are not considered.
dns.V4MAPPED: If the IPv6 family was specified, but no IPv6 addresses were found, then return IPv4 mapped IPv6 addresses. Note that it is not supported on some operating systems (e.g FreeBSD 10.1).


###总结

对于这个模块，只是对一些方法的使用有了一些体验，对于这个模块的用途可能需要在后续的开发工作中做出总结。














