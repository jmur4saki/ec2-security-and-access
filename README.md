# EC2: Concepts and Practice

This repository contains a complete walkthrough and report on launching and connecting to an Amazon EC2 instance, with a focus on secure access, key permissions, and AWS best practices. The PDF document includes screenshots of all the steps performed.

## 📘 Document

See the full lab report in [documentation](https://github.com/jmur4saki/ec2-security-and-access/blob/main/documentation.pdf), which includes:

- An overview of EC2 and its use cases
- Explanation of AWS Security Groups
- Importance of restricting SSH access
- The Principle of Least Privilege in practice
- Detailed terminal steps to connect to EC2
- Real-world challenge with cross-OS key handling
- **Screenshots** illustrating key steps and results

---

## 🧠 What Is EC2?

Amazon EC2 (Elastic Compute Cloud) is a cloud service that allows users to run virtual machines (instances) with customizable resources such as operating system, CPU, memory, and storage. It's commonly used to host servers, APIs, and applications. Access is typically established using SSH with a key pair, and security is controlled through Security Groups.

---

## 🔐 Security Groups

A Security Group in AWS is a virtual firewall that controls the network traffic allowed to and 
from your EC2 instances. It acts as a set of rules that define who can connect to your 
machine and where it can connect. 

### ✅ Inbound Rules

Define **which connections can reach the instance**.  
Examples:
- Allowing SSH (port 22) **only from your IP**
- Allowing HTTP (port 80) for web access
- Enabling internal communication between instances

### 🔁 Outbound Rules

Define **where the instance is allowed to connect**.  
By default, outbound traffic is allowed, but it can be restricted for higher security.

---

## ⚠️ Why Restricting SSH Matters

Port 22 (SSH) gives remote control over your server. Leaving it open to the internet (`0.0.0.0/0`) is dangerous, as it invites brute-force attacks and unauthorized access attempts.

Restricting SSH to your own IP address helps:
- Prevent unauthorized logins
- Block automated attack bots
- Enforce trusted-only access

---

## 🧱 Principle of Least Privilege

There is a fundamental security principle known as the **Principle of Least Privilege**. It states that any user, system, or service should be granted **only the minimum level of access required** to perform its function — no more, no less.

Examples:
- A developer who only needs to read data from a database **should not** have permission to delete or modify it.
- An EC2 instance that uploads files to S3 should be granted only the `PutObject` permission — **not full access** to the entire bucket.

Applying this principle helps reduce security risks, limit the impact of potential breaches or errors, and enforce stricter control over resources and actions within a cloud environment.

---

## 💻 Practical Steps

1. **Launched an EC2 instance** via AWS Console.
2. In a **Linux virtual machine**, created a `.ssh` directory with:
   ```bash
   mkdir -p ~/.ssh
   ```
3. **Copied the private key** (originally downloaded on Windows) to the Linux VM:
   ```bash
   cp /mnt/c/Users/YourUser/Downloads/key.pem ~/.ssh/
   ```
4. **Set proper permissions** for the key file:
   ```bash
   chmod 400 ~/.ssh/key.pem
   ```
5. **Connected to the EC2 instance**:
   ```bash
   ssh -i ~/.ssh/key.pem ubuntu@3.16.113.232
   ```

✔️ The SSH connection was successful!

---

## 🛡️ Security Enhancement

To prevent open access, a **custom security group** was created that allowed SSH access **only from my IP address**.

To test this:
- Switched to a **mobile data connection** (different IP)
- Attempted SSH connection — it **failed** as expected
- ✅ Confirmed the rule was working as intended

---

## 🧩 Challenge Faced

The `.pem` private key was downloaded on Windows, but the SSH connection was made from a Linux VM. Since the Linux terminal could not directly access Windows folders, the key had to be **manually copied** using:

```bash
cp /mnt/c/Users/YourUser/Downloads/key.pem ~/.ssh/
```

Then `chmod 400` was required to set the correct permissions. SSH refuses to use keys that are too permissive.

Only after these adjustments was the connection possible.

---

## 📎 References

- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
- [AWS Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
- [Principle of Least Privilege – NIST](https://csrc.nist.gov/glossary/term/least_privilege)

---

📝 This lab was prepared by **jmuras4ki** as part of an academic activity on AWS EC2 fundamentals.  
📸 **Note:** All screenshots are included in the PDF report.
