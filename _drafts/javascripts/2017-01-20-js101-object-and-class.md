---
layout: post
title:  "JS Basic: EP3. Object & Class"
subtitle: "object และ class แบบเจาะลึก"
date:   2017-01-16 02:15:41 +0700
categories: javascripts framework
image: /assets/images/thumbnail/js101.png
---

Javascript นั้นเป็นการประกอบกันไปด้วย Object และ Function

## Object

### this

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

