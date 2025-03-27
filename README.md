# CloudFoxable---It-s-another-secret
A lot of the challenges in the category Assumed Breach: Principal will have you assume into a role to simulate a new starting point.
# 🔐 CloudFoxable CTF - Challenge: *It's another secret*

**Category:** Assumed Breach: Principal  
**Author:** Seth Art ([@sethsec](https://twitter.com/sethsec))  
**Estimated Cost:** ~$0.05  
**Starting Role:** `arn:aws:iam::ACCOUNT_ID:role/Ertz`

---

## 📘 Overview

This challenge is part of the **CloudFoxable CTF**, simulating a real-world cloud penetration test. You start as a compromised user and are given access to assume the `Ertz` IAM role. Your objective is to identify accessible permissions and find the hidden flag: `its-another-secret`.

If you've completed the earlier challenge (*It's a secret*), this one will feel familiar—but with a new role and more practice in role assumption techniques.

---

## 🎯 Objective

✅ **Assume into the IAM role `Ertz`**  
🔍 **Enumerate what you can access**  
🎉 **Retrieve the flag**

> 🏑 **Flag:**  
> `FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}`

---

## 🚀 Steps to Solve

### 🔑 1. Set Up the AWS Profile to Assume Role

To avoid manually copying temporary credentials, you can configure an AWS CLI profile to handle role assumption automatically.

Open your `~/.aws/config` and add:

```ini
[profile ertz]
region = us-west-2
role_arn = arn:aws:iam::ACCOUNT_ID:role/Ertz
source_profile = cloudfoxable
```

Now run:

```bash
aws --profile ertz sts get-caller-identity
```

✅ If successful, you should see output like:

```json
{
  "UserId": "AROAQXHJKLZKFYSRACOES:botocore-session-ACCOUNT_ID",
  "Account": "ACCOUNT_ID",
  "Arn": "arn:aws:sts::ACCOUNT_ID:assumed-role/Ertz/botocore-session-ACCOUNT_ID"
}
```

---

### 🕵️ 2. Enumerate Permissions

Now that you're operating as the `Ertz` role, it's time to see what actions you can perform.

Use tools like:

- [`CloudFox`](https://github.com/BishopFox/cloudfox)
- [`enumerate-iam`](https://github.com/andresriancho/enumerate-iam)
- Or manual enumeration using the AWS CLI

Check for access to:

- Secrets Manager (`secretsmanager:GetSecretValue`)
- S3 buckets (`s3:ListBucket`, `s3:GetObject`)
- Systems Manager (`ssm:GetParameter`)
- IAM permissions (`iam:ListRoles`, `iam:GetPolicy`)

---

### 🎯 3. Find the Flag

In this case, the flag was directly accessible after role assumption and exploration:

```
FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}
```

No privilege escalation or advanced exploitation was required. Just correctly assuming the role and verifying access was enough!

---

## 🩼 Cleanup

No cleanup actions needed. No resources were modified or created.

---

## 📝 Notes

- This challenge reinforces role assumption using AWS CLI profiles.
- Enumerating IAM permissions is key to finding sensitive data.
- Expect more role-switching in future CloudFoxable CTF challenges!

---

## 🔗 References

- 📘 [CloudFoxable GitHub](https://github.com/BishopFox/cloudfoxable)
- 🛠️ [CloudFox Tool](https://github.com/BishopFox/cloudfox)
- 📒 [AWS CLI: Assume Role](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html)
- 📄 [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

---

**💡 Pro Tip:**  
Start every assumed-role scenario by checking what actions you can perform. Use `aws iam simulate-principal-policy`, `cloudfox iam`, or `enumerate-iam` to make your recon faster and more efficient!

