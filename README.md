# Initial-Terraform

Install Terraform

**On Windows**
 
  Download Terraform
  - download Terraform https://developer.hashicorp.com/terraform/install#windows โดย *386 สำหรับ Windows 32 and amd64 สำหรับ Windows 64*
  - สร้าง folder แล้ว นำไฟล์ที่ download ไปแตกไฟล์ลงไว้

  Set Environment
  - กดปุ่ม Windows แล้วค้นหา Environment แล้วเลือก Edit the system Environment Variables
  - เลือก Environment Variables
  - ในหัวข้อ System Variables : เลือกที่ Path แล้วนำ address path ของ folder ที่แตก Terraform ไฟล์ไว้ไปใส่

  Starter Terraform
  - เปิด Editor
  - สร้างไฟล์โครงสร้าง Terraform
    -  main.tf : ไฟล์หลักสำหรับการ run เพื่อสร้าง Service
    -  variables.tf : ไฟล์สำหรับกำหนดค่าของตัวแปรที่จะนำไปใช้ใน main
    -  output.tf : ไฟล์สำหรับกำหนดค่าที่ส่งออกไปใช้งาน

  Generate SSH Key
    - เปิด Command prompt แล้ว run ```ssh-keygen -t rsa -N "" -b 2048 -C "<key_name>"``` **เปลี่ยน <key_name> เป็นชื่อ key**
    - -t คือ Algorithm สำหรับ gen key
    - -N คือ passphrase ที่จะสร้าง
    - -b จำนวน Bit ของ Key แนะนำให้ 2048 ขึ้นไป
      
  Terraform Module Example: https://github.com/hashicorp-education/learn-terraform-modules-create.git

  Reference: ***About Configuring Terraform on Windows System*** https://docs.oracle.com/en/solutions/infrastructure-components-to-deploy-peoplesoft/ebs-configuring-terraform-windows-systems.html#GUID-FB72CD79-78BA-40F6-85C4-678CA4A44717
