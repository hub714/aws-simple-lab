## Create your first template

The goal of this section is to get you familiar with EC2 so that you know what you will be deploying with CloudFormation. Then you'll create a template to deploy EC2 while following best practices.

### Lab 1 - Launch and connect to an EC2 instance
The first thing we'll do is try to launch an EC2 instance into one of the public subnets of the VPC

1. Create an EC2 Key Pair

In order to log into your instance later, you'll have to create a key pair. You can do this in the console by navigating to the Key Pairs page of the EC2 Console or you can run a CLI command:

<pre>
$ aws ec2 create-key-pair --key-name aws-simple-lab --query 'KeyMaterial' --output text > aws-simple-lab.pem
$ chmod 600 aws-simple-lab.pem
</pre>

This command creates a key pair and outputs it as a text file named `aws-simple-lab.pem`. The chmod command locks it down. Keep in mind that you will never be able to download this key pair again, so don't lose it.

3. Launch an EC2 instance into a PUBLIC subnet

Navigate to the [AWS EC2 Console](https://console.aws.amazon.com/ec2) and click **Launch Instance**. Launch an instance with the following properties:

Launch an EC2 instance with these parameters:
- AMI: ami-0b69ea66ff7391e80 (**"Amazon Linux 2 AMI (HVM), SSD Volume Type"**)

![Choose AMI](images/chooseami.png)

- Instance type: t2.micro

![Choose Instance Type](images/choosetype.png)

- In the VPC that was created for you in a PUBLIC subnet (It's tagged public)
- Auto assigns a public IP

![Choose VPC](images/chooseVPC.png)

- In the aws-simple-lab security group
- With your aws-simple-lab keypair

4. Try to connect to your EC2 instance.

From Cloud9, try to connect. Get the public IP of your instance and use this command:

<pre>
$ nc -v publicIPofInstance 22
</pre>

5. Troubleshoot your connectivity

For some reason it doesn't seem like you can access your EC2 instance. Try and figure out why. 
<details>
<summary>HINT 1</summary>
There are a number of prerequisites for EC2 instances to be reachable via public IP. First they must have a public IP. Make sure you set the instance up properly with a publicly routable IP. 

</details>

<details>
<summary>HINT 2</summary>
The next thing to look at is the security group of your instance. Is it allowing access to port 22 from anything?
</details>

<details>
<summary>HINT 3</summary>
Finally, let's consider the VPC design. It's possible that the VPC was designed incorrectly. 
</details>

<details>
<summary>HINT 4</summary>

Check the subnet your instance is in and look at the route tables. What's required for internet connectivity here? Since we're focusing on the public subnet, the answer is here: [VPC Scenario 1](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario1.html).

</details>

<details>
<summary>FINAL HINT</summary>
Does the route table show a route to an IGW for 0.0.0.0/0? It doesn't. Choose a different route table to associate with the subnet. One of them will have the 0.0.0.0/0 route.
</details>

6. Test connectivity again

From Cloud9, do this again:

<pre>
$ nc -v publicIPofInstance 22
Connection to ec2-52-212-4-85.eu-west-1.compute.amazonaws.com 3389 port [tcp/ms-wbt-server] succeeded!
</pre>

At this point, you should be able to hit your instance and log in. But now we have to make sure the VPC is actually deployed correctly. 

### Lab 2 - Update VPC Configuration within CloudFormation


### Checkpoint
