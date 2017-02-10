---
layout: post
title:  "มาลองสร้าง Serverless Application บน AWS Lambda กัน"
subtitle: "รู้จักการทำ Serverless App / AWS Lambda + Mini Workshop"
date:   2017-01-16 02:15:41 +0700
categories: aws nodejs
image: /assets/images/thumbnail/lambda.png
---

&nbsp;&nbsp;&nbsp;&nbsp;Serverless เป็นเทรนใหม่ในการทำ Backend Application ในยุคปัจจุบันที่สามารถสร้างและขยายปริมาณการรองรับการใช้งานได้อย่างง่ายดาย และบน AWS Lambda นั่น สามารถช่วยให้เราทำ Serverless Application ในการจัดการข้อมูล รวมไปถึง API สำหรับแอพลิเคชันหน้าบ้านได้โดยแทบไม่ต้องใช้ความรู้ในการ setup และ deploy แอพในรูปแบบของเซิฟเวอร์แบบเดิมเลยทีเดียว เพียงแค่เขียนโปรแกรมให้ได้ตามที่ต้องการก็เพียงพอแล้ว

---

&nbsp;&nbsp;&nbsp;&nbsp;ในยุคปัจจุบันนั้นการ การดีพลอยเว็บใช้วิธีการรันไว้บนเซิฟเวอร์ทั่วไป ซึ่งจำเป็นต้องรันไว้ตลอดเวลาเพื่อให้สามารถเข้าถึงได้ตลอดเวลานั่นเอง เมื่อต้องรันไว้ตลอดเวลา ก็ต้องมีเซิฟเวอร์รันค้างไว้ ถึงแม้จะไม่มีการเข้าใช้งานก็ตาม แต่ก็ยังใช้ทรัพยากรนั่นไปเรื่อยๆอยู่ดี

แต่วันนี้เรามีแนวทางการทำเว็บใหม่มานำเสนอ นั่นก็คือ

## Serverless Application

&nbsp;&nbsp;&nbsp;&nbsp;ชื่อก็บอกอยู่แล้วนะครับ `Serverless` ก็คือ ไม่จำเป็นต้องมีเซิฟเวอร์รันอยู่ตลอดเวลา โดยแอพที่เรารันนั้นจะรันเฉพาะเมื่อมี event ในการเรียกใช้งานเข้ามาเท่านั้น เช่น มีการเรียก HTTP Request เข้ามา หรือ มีค่าอัพเดตทาง DB แล้ว trigger เข้ามารันใช้งานแอพ ซึ่งแน่นอนว่าวิธีการเขียน การออกแบบ และการดีพลอยแอพ ก็จะแตกต่างออกไปจากเดิมค่อนข้างมากเลยทีเดียว ซึ่งทาง AWS ก็มีเครื่องมือ ตัวนึงที่จะมาใช้ให้เราสามารถสร้าง serverless application ได้ง่ายๆ บน aws เอง โดยมีชื่อว่า

## AWS Lambda

&nbsp;&nbsp;&nbsp;&nbsp;AWS Lambda เป็นเครื่องมือที่ไว้ใช้สำหรับสร้างและรัน `Serverless Application` บน AWS ครับ โดยเจ้า Lambda เองเป็นเครื่องกลางในการผนวก service อื่นๆของอเมซอนเข้ามาใช้ในการรันแอพลิเคชัน มาดูกันก่อนว่าจริงๆแล้ว ตัว AWS Lambda เอง มันประกอบไปด้วยอะไรบ้าง และสามารถเชื่อมต่อกับ service อื่นๆตัวไหนของ aws ได้อีกบ้าง

### Architecture

&nbsp;&nbsp;&nbsp;&nbsp;การรันโปรแกรมบน AWS Lambda นั้น ตัว Lambda จะแปลงโค้ดที่เราจะต้องการรัน หรือเรียกสั่งๆว่าเป็น `Lambda Function` เป็น Container เมื่อมีการรันโปรแกรมผ่านช่องทางที่เราผูก `event` ไว้ ตัว lambda เองก็จะรันตัวโปรแกรมนั้นให้เราจบๆไปเป็น event และเมื่อไม่มีการเรียกใช้งานก็จะอยู่ในสถานะ freeze ตัวเอง หรือไม่ได้รันเซิฟเวอร์อยู่นั่นเอง

![AWS Lambda Architecture for Web Application](/assets/images/posts/lambda/lambda-architect.png)

ยกตัวอย่างการทำเว็บแอพลิเคชันเชื่อมต่อกับ AWS Lambda ซึ่งทำตัวเป็น API ตัวนึง ก็จะเกิดการรันก็ต่อเมื่อมี Request เข้ามาที่ตัวระบบ

### Lambda Function

การเขียนโปรแกรมบน AWS Lambda นั้นจะเป็น Functional Programming โดยเขียนเฉพาะฟังก์ชันเที่เราต้องการจะรันในการเกิด Event แต่ละครั้ง โดยเราสามารถเขียนบนภาษาที่ Lambda รองรับได้ คือ `Node.js` `C#` `Java` `Python`

### Event Driven

การรัน Lambda Function ที่เราเขียนไว้นั้น จะเกิดขึ้นก็ต่อเมื่อมี Event มาทำการสั่งรันตัวฟังก์ชัน ซึ่งมีด้วยกันหลาย trigger โดยแบ่งเป็น 2 แบบหลักๆคือ

- Streaming Event

เป็นอีเวนท์ที่เข้ามาตลอดเวลาในช่วงระยะเวลาหนึ่งๆ เช่น ดึงข้อมูลจาก social network เป็นต้น โดยอีเวนท์นี้สามารถที่จะเชื่อมต่อกับเซอร์วิสของ AWS 2 ตัว คือ

	- DynamoDB Streaming
	- AWS Kinesis

- Non-Streaming Event

เป็นอีเวนท์ปกติที่สามารถ trigger ขึ้นมาด้วย action บางอย่าง เช่น การเรียก HTTP Request / IoT Event เป็นต้น โดยสามารถเชื่อมต่อกับ AWS Services ได้ดังนี้

	- API Gateway 	สำหรับสร้าง API ให้สามารถเรียกได้ผ่าน HTTP Request
	- AWS IoT
	- Cloudfront
	- Cloudwatch
	- S3
	- SNS

![AWS Lambda S3](/assets/images/posts/lambda/lambda-photo.png)

ยกตัวอย่างการใช้งานรวมกับ S3 เมื่อมีรูปถูกอัพโหลดขึ้นมาที่ S3 ก็จะมี Event ตัวนึงไป trigger เพื่อรัน lambda function ไปทำการ resize รูป

## Hello World API with Node.js

สำหรับท่านที่ยังไม่มีสมัคร account aws กันก่อนนะครับ จากนั้นไปที่ Console > AWS Lambda แล้วเราก็จะมาเริ่มทำการสร้าง Lambda Function กันก่อนนะครับ

![AWS Lambda S3](/assets/images/posts/lambda/lambda-blueprint.png)

ในส่วนของการสร้างจะมี Blueprint ให้เราเลือกค่อนข้างเยอะ แต่ในบทความนี้เราจะมาดูตัวที่ง่ายที่สุดก่อนละกัน เลือกไปที่ `hello-world` เลยครับ


![AWS Lambda Trigger](/assets/images/posts/lambda/lambda-trigger.png)

จากนั้นเราก็จะมาตั้งค่าตัว trigger สำหรับ invoke lambda function ตัวนี้นะครับ โดยในตอนนี้จะมาสร้าง API สำหรับให้เรียกใช้ได้ผ่าน Http Request นะครับ ซึ่ง AWS ก็จะมีเครื่องมือที่เรียกว่า `API Gateway` 

![AWS Lambda + API Gateway](/assets/images/posts/lambda/lambda-api-gateway.png)

เมื่อเราเลือก API Gateway จากนั้นเราก็จะมาคอนฟิคตัว API กันนะครับ โดยมี 3 Option ให้เลือก

	- API name 			// Group Service
	- Deployment Stage		// Stage
	- Security

โดย Security จะมีให้เลือกอยู่ 3 option ตั้งเชื่อ API name และ deployment stage ได้ตามสะดวก

	- AWS IAM
	- Open
	- Open with access key

ในบทความนี้เราจะทำ API ที่เป็น Public ก่อน ดังนั้นเลือก Open เลยครับ 


![AWS Lambda Function Detail](/assets/images/posts/lambda/lambda-function-detail.png)

&nbsp;&nbsp;&nbsp;&nbsp;จากนั้นเราก็จะมา config ตัว lambda function และเขียนโค้ด lambda function กันนะครับ ก็ตั้งชื่อและเลือก Runtime เลย โดยในที่นี้ผมจะใช้ Node.js ในการรัน ซึ่งเวอร์ชันที่ Lambda ใช้จะเป็น Node 4.3 และในการเขียนโค้ดก็มีวิธีการใช้ดึงมาโค้ดมาใช้ได้หลายแบบ เช่น เขียนบนเว็บ อัพโหลดไฟล์ หรือดึงมาจาก S3 แน่นอนว่าตัวอย่างนี้แนะนำวิธีที่ง่ายที่สุดคือ เขียนบน `AWS Console` เลย โดยผมเลือก option เป็น default ทั้งหมดเลย โดยมี option ดังนี้

	- Environment variables
	- Hanlder
	- Role
	- Memory
	- Timeout
	- DLQ
	- VPC
	- KMS Key

โดยตัวหลักๆที่เราจะมาตั้งแค่กันก่อนในคราวนี้คือ

- Hanlder

เป็นตั้งค่าไฟล์และฟังก์ชันที่จะรันเป็นฟังก์ชันหลักของตัว Lambda Function ตัวนี้ โดย default จะเป็น index.hanlder คือฟังก์ชัน `hanlder` ในไฟล์ index.js นั่นเอง

- Role

เป็นการทำหนดการเข้าถึงเครื่องมืออื่นๆของ aws เช่น cloudwatch สำหรับดู log เป็นต้น โดย role นั้นจะต้องมี permission ที่ให้มาสามารถเข้าใช้งานเครื่องมืออื่นๆกำหนดไว้

การตั้งชื่อ API name / deployment stage และ function name จะมีผลกับ url ของ `API Gateway` ที่เราจะเรียกใช้ service นี้เช่นกัน โดยเราจะได้ url ออกมาในรูปแบบ

> https://aws-gen-url/[DEPLOYMENT STAGE]/[FUNCTION NAME]

เช่นผมจะได้ url สำหรับเรียกใช้ Lambda Function เป็น

> https://aws-gen-url/development/helloworld

จากนั้นก็กดสร้างก็จะได้ Lambda Function ไว้ช้งานเรียบร้อย

![AWS Lambda Function](/assets/images/posts/lambda/lambda-edit-response-console.png)


มาดูในส่วนของโค้ดฟังก์ชันกันก่อนนะครับ

```
'use strict';

console.log('Loading function');

exports.handler = (event, context, callback) => {
    //console.log('Received event:', JSON.stringify(event, null, 2));
    console.log('value1 =', event.key1);
    console.log('value2 =', event.key2);
    console.log('value3 =', event.key3);
    callback(null, event.key1);  // Echo back the first key value
    //callback('Something went wrong');
};
```

&nbsp;&nbsp;&nbsp;&nbsp;Lambda Function บน Node.js นั้นเราจะมีฟังก์ชันหลักโดยจะมี parameter อยู่ 3 ตัว ส่ง response กลับไปด้วยฟังก์ชัน callback และหากเราจะส่ง response ผ่านทาง `API Gateway` เราก็ต้องมาดูกันก่อนว่า API Gateway นั้นรับพารามิเตอร์อะไรบ้าง เพื่อให้เราสามารถเขียน response ของ lambda function ให้ถูกฟอร์แมตของตัว API Gateway กันนะครับ ไม่อย่างนั้นจะไม่สามารถส่ง response กลับไปที่บราวเซอร์ได้ถูกต้องนะครับ

ฟอร์แมต Response ไปที่ตัว `API Gateway` ต้องเป็นประมานนี้

```
{
  "statusCode": 200,		// HTTP Status
  "headers": {},		// Header
  "body": ""			//String
}
```

จากนั้นเราก็เขียน callback ให้ส่ง response กลับในรูปแบบนี้นะครับ

```
'use strict';

exports.handler = (event, context, callback) => {
	var str = "Hello World";

	callback(null, {
		"statusCode": 200,
		"headers": {},
		"body": str
	});
};
```

จากนั้นมาดูว่าเราสามารถ Trigger `Lambda Function` ได้ผ่านทาง url อะไรนะครับ โดยอยู่ที่แถบ Trigger ในที่นี้เราก็จะได้ url เอาไว้ลองทดสอบฟังก์ชันของเราดูได้แล้ว


![AWS Lambda Url Trigger](/assets/images/posts/lambda/lambda-url-trigger.png)

ตัว `API Gateway` นั่น สามารถ config ได้ว่าให้เข้าไปผ่านที่ Method ไหน แต่การสร้าง API Gateway เพื่อใช้งานกับ Lambda นั่น จะต้องผูกเป็น `ANY` Method หรือเข้าผ่านเมธอดอะไรก็ได้ ดังนั้นเราสามารถจึงยิงผ่านเมธอดอะไรก็ได้เข้ามานะครับ

![AWS Lambda Request Testing](/assets/images/posts/lambda/lambda-request-test.png)

ทีนี้มาลองรับค่าพารามิเตอร์โดยรับค่าผ่านตัวแปร `event` แล้วส่งค่าเป็น json response กลับไปผ่าน header `Content-Type` กันนะครับ ก่อนหน้านั้นมาดู Behavior ของตัว Lambda ฟังก์ชันกันก่อนว่ามีการรับค่า params และ payload เก็บไว้ที่ตัวแปรใด

โดยค่าที่มากับตัวแปร `event`, `context` นั้นจะขึ้นอยู่กับว่าเราผูกตัว Lambda Function เข้ากับ Trigger ตัวไหน โดยในที่นี้ใช้ API Gateway ค่าของตัวแปร parameter และ payload ที่ส่งมากับ API Gateway จะมากับตัวแปร event ในรูปแบบนี้

```
parameter 	>> event.queryStringParameters
payload 	>> event.body
```

โดยตัว payload จะมาในรูปแบบ raw string หากเราจะนำไปใช้ก็ต้องทำการ parse เป็น format ที่เราส่งมากันก่อน โดยในตัวอย่างส่งมาเป็น json ก็ทำการ parse json ซะ จากนั้นก็เอาค่า a และ b ผ่านทาง payload มาบวกกันแล้วส่ง response กลับไป

```
'use strict';

exports.handler = (event, context, callback) => {
	var body = JSON.parse(event.body);

	var sum = parseInt();
	var response = {
		equation: body.a + " + " + body.b,
		sum: body.a + body.b
	}

	callback(null, {
		"statusCode": 200,
		"headers": {
			"Content-Type": "application/json"
		},
		"body": JSON.stringify(response)
	});
};
```

เมื่อยิงผ่าน POST Method พร้อมส่ง Payload ไปด้วยก็ได้จะ Response ตามที่ต้องการ

![AWS Lambda Logs](/assets/images/posts/lambda/lambda-json-response.png)


## Debugging

หลายๆคนอาจจะสงสัยว่าเรารัน Lambda ที่เป็น Event Driven แบบนี้ เราจะสามารถดู log หรือทำการ debug ได้ที่ไหน ตัว AWS เองก็มีเครื่องมือช่วยในการทำเรื่องนี้ โดยใช้ `CloudWatch` เข้ามาช่วยเก็บ log แบบ streaming กันเลยทีเดียว เราสามารถใช้ console.log และเข้าไปดูผลที่ CloudWatch ได้เลย

![AWS Lambda Logs](/assets/images/posts/lambda/lambda-log.png)

## Monitoring

ตัว CloudWatch เองนอกจากจะช่วยให้เราดู Log ได้แล้ว ยังเก็บการทำงานและช่วยให้เรา Monitoring ตัว Lambda Function ได้ด้วย ว่ามีการใช้งานขนาดไหน

![AWS Lambda Monitoring](/assets/images/posts/lambda/lambda-monitoring.png)

## References

- [AWS Lambda](https://aws.amazon.com/lambda/)
- [AWS Lambda + API Gateway Proxy Intregration](http://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-as-simple-proxy.html)