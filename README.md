# Hemogram using AWS services

Hemogram is a web application designed to collect, store and process digital Blood Cells(BC) images from Peripheral Blood Smears(PBS) collected from patients at resource-limited clinics. 

Amazon Web Services(AWS) offer various cloud computing services to build a secure and highly avialable applications quickly and cost effectively. Here, I am trying to explore idea of building and hosting Hemogram web application using AWS.

## Hemogram Architecture Diagram
The diagram below shows the basic architecture for the Hemogram application. Inside the AWS, the application is deployed on an Amazon  EC2 instance. Any web requests to the application will be directed through Application Load Balancer which resides in front of the EC2 instance. 
The application will handle functions such as user sign up/sign in, patient registration, patient sample processing and among others. Amazon RDS instance is used for storing users and patient data, whereas Amazon S3 is for storing BC images. Amazon Lambda function is used to asynchronously process images metadata from Amazon S3 and update the Amazon RDS instance. 
![basic_architecture](https://user-images.githubusercontent.com/7229266/65449154-37869580-ddef-11e9-8d37-0b6ddc4be72c.png)


## Data Security
Since, the Hemogram application is processing sensitive patient data, it is very crucial protect these data. To protect the data, end-to-end encryption is required. In this section, I am exploring the idea of encrypting the data while it is in transit and at rest. 

### In transit
The diagram below depicts the data flow through various component of the application and how the data can be encrypted as it moves from one compoment to another. 
![data_in_transit](https://user-images.githubusercontent.com/7229266/65449123-276eb600-ddef-11e9-8547-0cc14e984002.png)

### At rest
It is equally important to protect the data while it is stored in Amazon RDS and Amazon S3 as it is when the data is in transit. For Amazon RDS and Amazon S3, we can enable encryption option to protect the data while at rest. 

Finally, we took several measures to protect the patient data by implementing encryption at rest, but it is equally important to protect the application configuration data which is in Amazon EC2 instance storage volume provided by Amazon Elastic Block Storage(EBS). AWS provides the option to encrypt the EBS volume. To encypt the patient data as well as the server configuration data, we will use Amazon Key Management Service (KMS).
![data_at_rest](https://user-images.githubusercontent.com/7229266/65449005-e9719200-ddee-11e9-834a-0459abc12495.png)

## High Availability
