# Initial-Terraform

## Prepain Environment
 #### สร้าง User บน IAM
 - ไปที่ AWS Management Console
 - เลือก IAM ภายใต้ Security, Identity & Compliacne
 - สร้าง User ใหม่บน IAM และตั้งค่าสิทธิ์ให้ User

#### สร้าง Access Key
 - เลือก User ที่ต้องการสร้าง Access Key
 - ไปที่แท็บ Security Credentials
 - กดปุ่ม Create accress key
 - เลือก Use case → เลือก Command Line Interface (CLI) แล้วกด Next
 - กดยืนยัน แล้วกด Create access key

 **สำคัญ! Copy ค่า Access Key ID และ Secret Access Key แล้วบันทึกไว้ เพราะจะดูได้แค่ครั้งเดียว**
 
#### การสร้าง Permission สำหรับเข้าถึง SQS & SNS
 - ไปที่ AWS Managerment Console
 - เลือก IAM ภายใต้ Security, Identity & Compliacne
 - เลือก Role
 - กดปุ่ม Create Role 
 - ใส่ชื่อ Role และเลือก Entity Type
 - เพิ่ม Permission AmazonSQSFullAccess และ AmazonSQNFullAccess ให้ User
 - กด Create Role

#### การเพิ่ม User ให้ Role ที่ใช้สำหรับเข้าถึง SQS & SNS
 - ไปที่ Role ที่สร้างไว้
 - เลือก User
 - กด Add User และเพิ่ม User ที่ต้องการ

## Install Terraform & CLI

### On Windows [Reference](https://docs.oracle.com/en/solutions/infrastructure-components-to-deploy-peoplesoft/ebs-configuring-terraform-windows-systems.html#GUID-6DD1EC34-3052-45C1-8196-7F07C47ACD74)
 
  #### Download Terraform
  - [Download Terraform](https://developer.hashicorp.com/terraform/install#windows) โดย *386 สำหรับ Windows 32 and amd64 สำหรับ Windows 64*
  - สร้าง folder แล้ว นำไฟล์ที่ download ไปวาง
  - แตกไฟล์ที่ dowload มา

  #### Set Environment
  - กดปุ่ม Windows แล้วค้นหา Environment แล้วเลือก Edit the system Environment Variables
  - เลือก Environment Variables
  - ในหัวข้อ System Variables : เลือกที่ Path แล้วนำ address path ของ folder ที่แตก Terraform ไฟล์ไว้ไปใส่
 
  #### Download AWS CLI
  - [Download AWS CLI](https://awscli.amazonaws.com/AWSCLIV2.msi)
  - ติดตั้ง AWS CLI
  - ตรวจสอบว่า Install สำเร็จ โดยการรัน ```aws --version```

### On macOS [Reference](https://docs.oracle.com/en/solutions/infrastructure-components-to-deploy-peoplesoft/ebs-configuring-terraform-unix-systems.html#GUID-79356933-676C-427A-ACEE-8F49E634011A)
 
  #### Download Terraform
```
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

  #### Download AWS CLI
  - [Download AWS CLI](https://awscli.amazonaws.com/AWSCLIV2.pkg)
  - ติดตั้ง AWS CLI
  - ตรวจสอบว่า Install สำเร็จ โดยการรัน ```aws --version```

  #### Setup AWS CLI
  - เปิด Terminal หรือ iTerm
  - run ```aws configure```
  - ใส่ Access Key ID (จาก IAM)
  - ใส่ Secret Access Key (จาก IAM)
  
