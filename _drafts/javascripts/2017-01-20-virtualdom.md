---
layout: post
title:  "VirtualDOM"
subtitle: "มารู้จัก VirtualDOM ใน HTML5 กัน"
date:   2017-01-16 02:15:41 +0700
categories: javascripts framework
image: /assets/images/thumbnail/js101.png
---

หลายๆคนที่เขียนเว็บในยุคปัจจุบันน่าจะเคยได้ยินเรื่อง VirtualDOM ที่เป็นฟีเจอร์ใหม่บน HTML5 และมีหลายต่อหลาย Framework ที่เลือกใช้ เช่น React / Vue เพื่อช่วยในเรื่องของประสิทธิภาพของการเรนเดอร์ให้เร็วขึ้นกว่าเดิมมาก แต่มันคืออะไรละ เดี๋ยวมาดูกัน

## DOM

ก่อนจะไปรู้จักการ VirtualDOM หรือดอมเสมือนเนี่ย เราก็ต้องมารู้จัก DOM ตัวจริงกันก่อน

```
<html>
	<body>
		<div class="container">
			<h3>Hello World</h3>
			<div class="content">
				Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt
			</div>
		</div>
	</body>
</html>
```

