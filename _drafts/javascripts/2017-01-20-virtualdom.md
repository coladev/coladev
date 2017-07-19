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

ก่อนจะไปรู้จักการ VirtualDOM หรือดอมเสมือนเนี่ย เราก็ต้องมารู้จัก DOM ตัวจริงกันก่อน DOM ย่อมาจาก Document Object Model ซึ่งเป็ฯรูปแบบของเอกสารอย่างหนึ่ง
เมื่อนำไปใช้กับ HTML ก็จะเรียกว่า HTML DOM ซึ่งก็จะมีลักษณะเป็น Tree ที่เราสามารถใช้ค้นหา เดินทางไปตาม path ต่างๆได้ โดยตัว Browser ก็มีส่วนของ API ที่ใช้สำหรับการค้นหาและจัดการ HTML DOM อยู่ด้วย โดยเป็น Javascript Interface

### DOM Problem

DOM API เป็นตัวช่วยให้เราเข้าถึง DOM Node ให้ง่ายขึ้น แต่ความง่ายมักก่อให้เกิดความช้า แม้การค้นหา DOM Node จะใช้เวลาไม่นาน แต่การกระทำอะไรสักอย่างกับ DOM Node นั้น ค่อนข้างใช้เวลา เนื่องจากบราวเซอร์ต้องทำการอัพเดตผลลัพธ์ใหม่ที่ได้ในทุกๆครั้งที่เกิดการการทำ ซึ่งส่งผลให้ช้าได้ ซึ่งเราไม่สามารถทำการแก้ไขอะไรได้ นอกจากรอบราวเซอร์ปรับปรุงวิธีการ render ใหม่ให้ได้ประสิทธิภาพดีขึ้นนั่นเอง แต่ใน HTML5 ได้มีแนวคิดและมาตรฐานใหม่การในจัดการ DOM ซึ่งให้ประสิทธิภาพดีกว่า นั่นคือ Virtual DOM

## Virtual DOM

Virtual DOM เป็นวิธีการจัดการ DOM โดยใช้วิธีตามชื่อมันคือทำตัวเป็น DOM เสมือนเมื่อเราจัดการงานอะไรเสร็จค่อยจัดการเข้าไปเปลี่ยนแปลง DOM โดยเราสามารถใช้ Virtual DOM ของเราเข้าทำการ replace / render DOM ในส่วนที่เรากระทำนั่นได้เลย



## Performance

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

