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

## เริ่มต้น Terreform

#### 1. Clone template Terraform
1. ไปที่ [Terraform Template Repository](https://veteran.socialenable.co/customix/bbki/bbki-tf)
2. Clone Repository

#### 2. Setup Template
1. สร้าง Folder ที่ /environments `suggestion: bb-sit, bb-prod` โดยใน Folder จะประกอบด้วยไฟล์ ดังนี้
2. สร้างไฟล์ `variables.tf` และเพิ่มข้อมูลดังนี้
```sh
#variables.tf

########################################
## PROJ INFO
########################################
variable "project_name" {
  description = "Project Name"
  default     = "ASA"
}
variable "project_chn" {
  description = "Project CHN Number"
  default     = "25628"
}



variable "environment_stage" {
  description = "ENV Stage"
}


########################################
## AWS
########################################
variable "aws_region" {
  description = "The AWS region"

  
}




variable "aws_account_id" {
  description = "AWS account ID"
  type        = string
}


########################################
## SNS
########################################
variable "sns_platform_applications" {
  type = list(object({
    platform_applications_name = string
    platform_credential        = string
    success_feedback_sample_rate = number
  }))
}

variable "sns_topics" {
  type = list(object({
    name = string
  }))
}

#######################################
# SQS
#######################################
variable "sqs_queues" {
  type = list(object({
    queue_name                 = string
    delay_seconds              = number
    max_message_size           = number
    message_retention_seconds  = number
    receive_wait_time_seconds  = number
    visibility_timeout_seconds = number

    fifo_queue = bool

    redrive_policy_enable = bool
    redrive_policy_dead_letter_target_arn = optional(string)
    redrive_policy_max_receive_count = optional(number)

  }))
}

variable "sqs_dlqs" {
  type = list(object({
    queue_name                 = string
    delay_seconds              = number
    max_message_size           = number
    message_retention_seconds  = number
    receive_wait_time_seconds  = number
    visibility_timeout_seconds = number

    fifo_queue = bool

  }))
}
```
3. สร้างไฟล์ `providers.tf` และเพิ่มข้อมูลดังนี้
```sh
#providers.tf

provider "aws" {
  region = "ap-southeast-1"
}
```
4. สร้างไฟล์ `version.tf`
```sh
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = ">= 5.34"
    }
  }
}
```
5. สร้างไฟล์ `main.tf` และเพิ่มข้อมูลดังนี้
```sh
#main.tf

module "iam" {
  source = "../../modules/iam"
}

module "sns_platform_applications" {
  source = "../../modules/sns-platform-applications"
  for_each = { for platform_application in var.sns_platform_applications : platform_application.platform_applications_name => platform_application }

  platform_applications_name = each.value.platform_applications_name
  platform_credential = each.value.platform_credential
  failure_feedback_role_arn    = module.iam.sns_failure_feedback_role_arn
  success_feedback_role_arn    = module.iam.sns_success_feedback_role_arn
  success_feedback_sample_rate = each.value.success_feedback_sample_rate
  environment_stage = var.environment_stage
  project_name = var.project_name
  project_chn = var.project_chn

  depends_on = [module.iam]
}

module "sns_topics" {
  source = "../../modules/sns-topics"
  for_each = { for topic in var.sns_topics : topic.name => topic }

  topic_name = each.value.name
  environment_stage = var.environment_stage
  project_name = var.project_name
  project_chn = var.project_chn
}

module "sqs_normal" {
  source = "../../modules/sqs/sqs-normal"
  for_each = { for queue in var.sqs_queues : queue.queue_name => queue }

  queue_name = each.value.queue_name
  delay_seconds = each.value.delay_seconds
  max_message_size = each.value.max_message_size
  message_retention_seconds = each.value.message_retention_seconds
  receive_wait_time_seconds = each.value.receive_wait_time_seconds
  visibility_timeout_seconds = each.value.visibility_timeout_seconds
  
  fifo_queue = each.value.fifo_queue

  redrive_policy_enable = each.value.redrive_policy_enable
  redrive_policy_dead_letter_target_arn = try(
    each.value.redrive_policy_enable ? module.sqs_dlqs[each.value.redrive_policy_dead_letter_target_arn].aws_sqs_dlq_arn : null,
    null
  )
  redrive_policy_max_receive_count = try(
    each.value.redrive_policy_enable ? each.value.redrive_policy_max_receive_count : null,
    null
  )

  environment_stage = var.environment_stage
  project_name = var.project_name
  project_chn = var.project_chn

  depends_on = [module.sqs_dlqs]
}

module "sqs_dlqs" {
  source = "../../modules/sqs/sqs-dlqs"
  for_each = { for dlq in var.sqs_dlqs : dlq.queue_name => dlq }

  queue_name = each.value.queue_name
  delay_seconds = each.value.delay_seconds
  max_message_size = each.value.max_message_size
  message_retention_seconds = each.value.message_retention_seconds
  receive_wait_time_seconds = each.value.receive_wait_time_seconds
  visibility_timeout_seconds = each.value.visibility_timeout_seconds
  
  fifo_queue = each.value.fifo_queue

  environment_stage = var.environment_stage
  project_name = var.project_name
  project_chn = var.project_chn
}
```
6. สร้างไฟล์ `terraform.auto.tfvars` และเพิ่มข้อมูลดังนี้
```sh
#terraform.auto.tfvars

environment_stage = "sit"   # Define environment for provisioning example stg,sit,uat,preprod,prod
project_chn       = "25628" # CHN number for TLI service request
project_name      = "ASA"

aws_region = "ap-southeast-1"
aws_account_id               = "806386304711" # AWS account id which is target of provisioning , example 806386304719
#ecs_task_execution_role_name = "" # example ecsTaskExecutionRole

########################################
## SNS
########################################
sns_platform_applications = [
  {
    platform_applications_name = "ms-notification"
    platform_credential        = <<EOF
{
    "type": "",
    "project_id": "",
    "private_key_id": "",
    "private_key": "",
    "client_email": "",
    "client_id": "",
    "auth_uri": "",
    "token_uri": "",
    "auth_provider_x509_cert_url": "",
    "client_x509_cert_url": "",
    "authDomain": "",
    "projectId": "",
    "storageBucket": "",
    "messagingSenderId": "",
    "appId": "",
    "measurementId": "",
    "universe_domain": ""
}
EOF
    success_feedback_sample_rate = 100
  }
]

sns_topics = [
  {
    name = "company-news"
  },
  {
    name = "customer"
  },
  {
    name = "self-recruitment"
  },
  {
    name = "team-recruitment"
  }
]

########################################
## SQS
########################################

sqs_queues = [
  {
    queue_name                 = "ms-notification.fifo"
    delay_seconds              = 0
    max_message_size           = 262144
    message_retention_seconds  = 1209600
    receive_wait_time_seconds  = 0
    visibility_timeout_seconds = 30
    fifo_queue = true

    redrive_policy_enable = false
  },
  {
    queue_name                 = "ms-push-notification.fifo"
    delay_seconds              = 0
    max_message_size           = 262144
    message_retention_seconds  = 604800
    receive_wait_time_seconds  = 0
    visibility_timeout_seconds = 30
    fifo_queue = true

    redrive_policy_enable = true
    redrive_policy_dead_letter_target_arn = "ms-push-notification-dlq.fifo"
    redrive_policy_max_receive_count = 5
  }
]

sqs_dlqs = [
  {
    queue_name                 = "ms-push-notification-dlq.fifo"
    delay_seconds              = 0
    max_message_size           = 262144
    message_retention_seconds  = 1209600
    receive_wait_time_seconds  = 0
    visibility_timeout_seconds = 30
    fifo_queue = true
  }
]

```

---

#### การ Config ไฟล์ `terraform.auto.tfvars`
1. Environment และ Project Configurations
   | Parameter | Description |
   |------------------|--------------------------|
   | `environment_stage` | ใช้ระบุการติดตั้งโครงสร้างพื้นฐาน `example : stg,sit,uat,preprod,prod` |
   | `project_chn` | หมายเลขที่ใช้ระบุโครงการภายในองค์กร |
   | `project_name` | ชื่อโปรเจกต์ที่จะใช้สร้างทรัพยากร |
   | `aws_region` | โซนที่ตั้งของ AWS Service `example : ap-southeast-1` |
   | `aws_account_id` | กำหนด ID ของบัญชี AWS ที่ใช้ `example : 806386304719` |

   Example
   ```sh
   environment_stage = "sit"   # Define environment for provisioning example stg,sit,uat,preprod,prod
   project_chn       = "25628" # CHN number for TLI service request
   project_name      = "ASA"
   
   aws_region = "ap-southeast-1"
   aws_account_id               = "806386304711" # AWS account id which is target of provisioning , example 806386304719
   ```
3. SNS Config

   2.1 sns_platform_applications `List<Object>` : ใช้สำหรับกำหนดค่าการเชื่อมต่อ AWS SNS กับบริการ Push Notifications
      | Parameter | Description |
      |------------------|--------------------------|
      | `platform_applications_name` | กำหนดชื่อของ SNS Application ที่จะใช้ |
      | `platform_credential` | ใช้กำหนดค่าการรับรองความถูกต้องของ Firebase (JSON Key) |
      | `success_feedback_sample_rate` | ค่าเปอร์เซ็นต์ที่ใช้กำหนดอัตราส่วนของ Feedback ที่จะได้รับจาก SNS `example : 100` (หมายถึงได้รับทุกข้อความ) |

      Example
      ```sh
      sns_platform_applications = [
         {
          platform_applications_name = "ms-notification"
          platform_credential        = <<EOF
            {
             "type": "service_account",
             "project_id": "project-example-dev",
             "private_key_id": "867730b2d27a8e531d24d23b7048768adgg11ji0",
             "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCgqARyU/yTS/RZ\n66WXAYmzXBo4u+qljwwnfZKFZ4HXit/kOBx0RLQRL26s+jH7NCpnCtJPPGDpl7fV\nvmIOYQj6gcrFanhL0xY+IHkc7bhoDaxO5mLBAsw4uZ7ZZNQmhEuQMknF7BFoR+TF\nusJcprOS6WHVVmPUGuYqOHdSEQ22RaGLxlBM9lfqFmzUle4bpQk8Ir/Y2WcHEMaI\njm532SlmMYHgUlEC0WmlxfOFVlYxMvh4OZ5DOsZp20sDXUsUkqj3l0i3yyCsU81J\nmbq3vBsRV1f5HYrJwMn9v+EuBxjl9T8SGLn7N3SYsQj8yZsjepOudc7ktI/lEUNV\nbI4wW3FfAgMBAAECggEAJHqiL9eUNVn8zOIuAmDOBJT4O0OoHxmhF+Jyf6omGGwn\nNGntRBVJkdFkXxCcgr6wlh8l6+n403Gow3bsSvmLkV4hTbxEPmta1JaJCaGMysu4\nAV5ctBjPiF3VjOVRKsHFaqOQjslwlm4kqyUh9um/Qq+NkGSCXGoTMykJJvBKXcWL\nlll/pbQeqa+tSHVkK2ikJLkL7uYt+SEca25fKIuytd5sazm0i2sHeB0NCfwyhxQ\nvNUSr3tezX1Gb61ToNOV5yt+4pYLp5s3prtWEu1pteulLB6eLJJiNy3uYElnwh2p\n/ULs9SEZ0vlsFOFX4iF+Pk6Kjm59Q+EqNJNz5ByMCQKBgQDVX/KTqikt/Qm80jOP\n8UYz8FilfSpc/KsRnc3mWku89ljTzf30aL5GxFh7nMpGU1i1xoCsTomiGI5vJU5h\nIzdzn6u2hgY9Mnc5PzPmxQLeV1l2O/MU9DO+0FexA5LevDPNh+3XhGh5OR0gOfZC\n+H8J5MPGsSTQqYG7/gTLAInyJQKBgQDAwAfcyP8UvCtpsp5uL2E7bMO+ZByBRfHo\nXpaN9BCbTajw0ihLz6OnoLd9/OD13yzoNAFPUzAYnzHZ6DZSLIxyzBp3RSQalWxb\nlK1GoxCMpYfz4AXmB6MM1h2ewtfd9mWg5rjOirqQBkE63c9bet4Ue6pp1Gj+WDsc\ng6zF/5ckMwKBgFaVorftGtzAi40Ft0q324cEBtTQn7owBbMWWBEbnxRE2vLH6j+d\nqWjKECXJEAPNmnfY3yoMjKksUdAkUVP1mHDg4Csm4609p0ehsAv68hkLVRA3b4KX\nPdA2dv8rthOQR20ltYv4/PQECx+ipZ2zfMmk/RTJmWja//wrLgK6az/JAoGAR/Zp\n+RBBy+gE/U1mwRSHZLxSyH5v9gO7MVo/M3hvysvPyzlrOgKnwynJ2AvVoXZVOcxQ\nwOD6D/c/kW+U+FERjrqNFwKFXDd1YSu9FaZDw375px94lE09nslB/cgY+BHKF6yM\nSvCYfrUXHXc6t5jD2Tnigeh/2pXuW9L9al7meukCgYEAhRm8F2XxBJQBs9bDziHe\nW5+MIK5U9jmIUqVwcLybSSX2ABriBnExwbCKDRyLi4o5DcmDXVD5Y3sDG9nLi9u8\nAcEL6djrmjSvDuh0z1waX11+QOkyjKk4VFnB0rTiOVKj/IzZZoQatxWiO1ESpHsk\nMUS88RHwDRA0cq3rv6Ssr3o=\n-----END PRIVATE KEY-----\n",
             "client_email": "firebase-adminsdk-x4ocf@project-example-dev.iam.gserviceaccount.com",
             "client_id": "108474086456716402281",
             "auth_uri": "https://accounts.google.com/o/oauth2/auth",
             "token_uri": "https://oauth2.googleapis.com/token",
             "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
             "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/firebase-adminsdk-x4ocf%project-example-dev.iam.gserviceaccount.com",
             "authDomain": "project-example-dev.firebaseapp.com",
             "projectId": "project-example-dev",
             "storageBucket": "project-example-dev.appspot.com",
             "messagingSenderId": "18563310883",
             "appId": "1:18563310883:web:faa3854d0dd8bb1uyt5556",
             "measurementId": "G-JX9M4EHLH0",
             "universe_domain": "googleapis.com"
            }
      EOF
          success_feedback_sample_rate = 100
         }
      ]
      ```
      
   2.2 sns_topics `List<Object>` : ใช้สำหรับการส่งข้อความแจ้งเตือนผ่าน AWS SNS
      | Parameter | Description |
      |------------------|--------------------------|
      | `name` | เป็นจุดรับข้อความที่ผู้ใช้สามารถสมัครรับข้อมูลได้ |
   
      Example
      ```sh
      sns_topics = [
        {
          name = "company-news"
        },
        {
          name = "customer"
        },
        {
          name = "self-recruitment"
        },
        {
          name = "team-recruitment"
        }
      ]
      ```
  
4. SQS Config

   3.1 sqs_queues `List<Object>` : ใช้สำหรับกำหนดค่า คิวข้อความ เพื่อรับและประมวลผลข้อความ
   | Parameter | Description |
   |------------------|--------------------------|
   | `queue_name` | ชื่อของ SQS Queue |
   | `delay_seconds` | เวลาในการหน่วงข้อความก่อนจะสามารถดึงไปใช้งานได้ (0 = ไม่มีหน่วง) |
   | `max_message_size` | ขนาดสูงสุดของข้อความที่สามารถส่งได้ (หน่วยเป็น Bytes, ค่าเริ่มต้น 256 KB) |
   | `message_retention_seconds` | เวลาที่ข้อความสามารถเก็บอยู่ในคิวได้ (สูงสุด 14 วัน) |
   | `receive_wait_time_seconds` | ใช้กำหนดค่าระยะเวลารอรับข้อความ (0 = ไม่ต้องรอ) |
   | `visibility_timeout_seconds` | เวลาที่ข้อความจะถูกซ่อนหลังจากมีคนดึงไปใช้งาน (ช่วยป้องกันการอ่านซ้ำ) |
   | `fifo_queue` | กำหนดว่าคิวเป็น FIFO (First-In-First-Out) หรือไม่ |
   | `redrive_policy_enable` | เปิดใช้งาน Dead-letter Queue หรือไม่ |
   | `redrive_policy_dead_letter_target_arn` | ARN ของ Dead-letter Queue ที่ใช้เมื่อข้อความเกิดข้อผิดพลาด |
   | `redrive_policy_max_receive_count` | จำนวนครั้งที่อนุญาตให้รับข้อความก่อนที่จะย้ายไปยัง DLQ |
   
   Example
   ```sh
      sqs_queues = [
        {
          queue_name                 = "ms-notification.fifo"
          delay_seconds              = 0
          max_message_size           = 262144
          message_retention_seconds  = 1209600
          receive_wait_time_seconds  = 0
          visibility_timeout_seconds = 30
          fifo_queue = true
      
          redrive_policy_enable = false
        },
        {
          queue_name                 = "ms-push-notification.fifo"
          delay_seconds              = 0
          max_message_size           = 262144
          message_retention_seconds  = 604800
          receive_wait_time_seconds  = 0
          visibility_timeout_seconds = 30
          fifo_queue = true
      
          redrive_policy_enable = true
          redrive_policy_dead_letter_target_arn = "ms-push-notification-dlq.fifo"
          redrive_policy_max_receive_count = 5
        }
      ]
   ```
   
   3.2 sqs_dlqs `List<Object>` : ใช้สำหรับกำหนด Dead Letter Queue เพื่อเก็บข้อความที่ล้มเหลว
   
   Example
   ```sh
   sqs_dlqs = [
     {
       queue_name                 = "ms-push-notification-dlq.fifo"
       delay_seconds              = 0
       max_message_size           = 262144
       message_retention_seconds  = 1209600
       receive_wait_time_seconds  = 0
       visibility_timeout_seconds = 30
       fifo_queue = true
     }
   ]
   ```
