# Initial Terraform

## การเตรียมสภาพแวดล้อม (Prepare Environment)

### 1. การสร้าง IAM User
1. ไปที่ `AWS Management Console`
2. เลือก `IAM` ภายใต้หมวด `Security, Identity & Compliance`
3. สร้าง `User` ใหม่ พร้อมกำหนดสิทธิ์การเข้าถึงตามความเหมาะสม

### 2. การสร้าง Access Key
1. เลือก `IAM User` ที่ต้องการสร้าง `Access Key`
2. ไปที่แท็บ `Security Credentials`
3. คลิก `Create access key`
4. ในส่วน `Use case` ให้เลือก `Command Line Interface (CLI)` จากนั้นกด Next
5. ยืนยันการสร้าง และคลิก `Create access key`
6. **สำคัญ!** คัดลอก `Access Key ID` และ `Secret Access Key` เก็บไว้ เนื่องจากระบบจะแสดงข้อมูลนี้เพียงครั้งเดียว

### 3. การกำหนดสิทธิ์ (`Permission`) สำหรับการเข้าถึง SQS และ SNS
1. ไปที่ `AWS Management Console`
2. เลือก `IAM` ภายใต้หมวด `Security, Identity & Compliance`
3. เลือก `Roles` และคลิก `Create Role`
4. ตั้งชื่อ `Role` และเลือก `Entity Type` ตามการใช้งาน
5. เพิ่มสิทธิ์ (`Permissions`) โดยเลือก `AmazonSQSFullAccess` และ `AmazonSNSFullAccess`
6. คลิก `Create Role`

### 4. การเพิ่ม User ให้กับ Role ที่ใช้สำหรับ SQS และ SNS
1. ไปที่ `Roles` ที่สร้างไว้
2. เลือกแท็บ `Users`
3. คลิก `Add User` และเลือก User ที่ต้องการเพิ่มเข้าไปใน Role

---

## การติดตั้ง Terraform และ AWS CLI

### สำหรับ Windows [#Referance](https://docs.oracle.com/en/solutions/infrastructure-components-to-deploy-peoplesoft/ebs-configuring-terraform-windows-systems.html#GUID-6DD1EC34-3052-45C1-8196-7F07C47ACD74)

#### 1. ดาวน์โหลดและติดตั้ง Terraform
1. ดาวน์โหลด `Terraform` จากเว็บไซต์ทางการ: [Download Terraform](https://developer.hashicorp.com/terraform/install#windows)  
   - เลือก `386` สำหรับ `Windows 32-bit` หรือ `amd64` สำหรับ `Windows 64-bit`
2. สร้างโฟลเดอร์สำหรับเก็บไฟล์ Terraform
3. แตกไฟล์ (`.zip`) ที่ดาวน์โหลดมา และย้ายไปยังโฟลเดอร์ที่สร้างไว้

#### 2. ตั้งค่า Environment Variables
1. กดปุ่ม `Windows` แล้วค้นหา `Environment Variables`
2. เลือก `Edit the system environment variables`
3. ในหน้าต่าง `System Properties` คลิก `Environment Variables`
4. เลื่อนลงไปที่ `System Variables` และเลือก `Path`
5. คลิก `Edit` และเพิ่ม `ที่อยู่โฟลเดอร์ของ Terraform` ลงไป

#### 3. ดาวน์โหลดและติดตั้ง AWS CLI
1. ดาวน์โหลด `AWS CLI`: [Download AWS CLI](https://awscli.amazonaws.com/AWSCLIV2.msi)
2. ติดตั้ง `AWS CLI` โดยทำตามขั้นตอนที่ระบบแนะนำ
3. ตรวจสอบการติดตั้งโดยรันคำสั่ง

   ```sh
   aws --version
   ```

---

### สำหรับ macOS [#Referance](https://docs.oracle.com/en/solutions/infrastructure-components-to-deploy-peoplesoft/ebs-configuring-terraform-unix-systems.html#GUID-79356933-676C-427A-ACEE-8F49E634011A)
#### 1. ดาวน์โหลดและติดตั้ง Terraform
ใช้คำสั่งด้านล่างเพื่อติดตั้ง `Terraform` ผ่าน `Homebrew`:  
```sh
brew tap hashicorp/tap  
brew install hashicorp/tap/terraform  
```

#### 2. ดาวน์โหลดและติดตั้ง AWS CLI
1. ดาวน์โหลด `AWS CLI`: [Download AWS CLI](https://awscli.amazonaws.com/AWSCLIV2.pkg)
2. ติดตั้ง `AWS CLI` ตามขั้นตอนที่ระบบแนะนำ
3. ตรวจสอบการติดตั้งโดยรันคำสั่ง

   ```sh
   aws --version
   ```

#### 3. ตั้งค่า AWS CLI
1. เปิด `Terminal` หรือ `iTerm`
2. รันคำสั่ง

   ```sh
   aws configure
   ```  
4. กรอก `Access Key ID` และ `Secret Access Key` ที่ได้จาก IAM
5. กำหนด `Default region name` (เช่น `us-east-1`, `ap-southeast-1`)
6. ตั้งค่า `Output format` (ค่าเริ่มต้นเป็น `json` หรือเลือก `table`, `text`)

---
