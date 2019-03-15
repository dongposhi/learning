

https://instagram-engineering.com/what-powers-instagram-hundreds-of-instances-dozens-of-technologies-adf2e22da2ad

- 这是一篇写于2011 年的文章，回答的是Instagram 技术栈选择的问题
- 首先，这篇文章回答的技术选型的**原则**
	- 简单
	- 不重新发明轮子
	- 使用可靠的技术
- 随后，回答了在各个环节的技术选择。可以说也是一个AWS 使用的案例。具体的技术选择不重要，重要的是对一个Web App 系统的模块分割方式和选型实践的思考角度
	- OS/Hosting
		- AWS + Ubuntu
	- Load Balancing
		- AWS R53
	- App Server
		- Django
	- Data Storage
		- Database
			- Postgre
		- Photos
			- AWS S3
		- CDN
			- AWS CloudFront
		- feed, session data
			- redis
		- cache
			- memcache
	- Task queue and push notificatoin
	- Monitoring
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg3ODU0ODM2OF19
-->