---
layout: post
title:  "JS Basic: EP3. Object & Class"
subtitle: "object และ class แบบเจาะลึก"
date:   2017-01-16 02:15:41 +0700
categories: javascripts framework
image: /assets/images/thumbnail/js101.png
---

## Object

### New Operator

จากบทที่แล้วเราจะเห็น operator ตัวหนึ่งที่ใช้สำหรับสร้าง object ใหม่นั่นคือ `new` operator โดยตัว object ที่เราสร้างขึ้นมาใหม่นั้นจะมี scope เป็นของตัวเอง โดย

```
var Person = function(name) {
	this.name = name
}

var john = new Person("john");		// new object
var sara = new Person("sara");		// new object
```

โดยใน object ที่เราสร้างขึ้นมานั้นจะมีตัวแปรๆนึงที่สามารถใช้ได้ภายใน object นั้น คือ `this` โดยจากตัวอย่างข้างต้นเราจะเห็นได้ว่า มีความเหมือนกัน class บนภาษาอื่นๆเลยทีเดียว แล้วมี private variable อ้างอิงอยู่กับ this นั่นเอง

นอกจากการใช้คำสั่ง new แล้ว การประกาศ object ใหม่ก็เป็นการสร้าง scope ขึ้นมาใหม่เช่นกัน และสามารถเรียกใช้ this ใน scope นั้นได้เช่นกัน

```
var Person = {
	name: "john",
	getName: function() {
		return this.name;
	}
}

Person.getName() 	// john
```

## Class

จริงๆแล้ว javascript นั้น ไม่มีสิ่งที่เรียกว่า class จริงๆเหมือนพวกภาษาจาวา มีแต่สิ่งที่เราเรียกว่า Class-like หรือสิ่งที่คล้ายๆกันเท่านั้น เราจะเห็นได้ว่าเราจะใช้ function แทนการใช้ class เช่น

```
var Building = function(name) {
	this.name = name;
}

var House = new Building("House");
var Tower = new Building("Tower");

House.name 	// House
Tower.name 	// Tower
```

### Constructor

### Prototype

### ES6 Class

