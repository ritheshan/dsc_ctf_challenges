# Fav Number - CTF Write-up ✨⚡️✨

## Category: Misc (200 points) 

### Description 📝🎉🛠️
We are given a challenge where we need to guess the admin’s favorite number within the range [1, 10^100]. Upon connecting to the server, it provides hints whether our guess is too small or too large.

**Challenge Connection:**
```bash
nc favno.ctf.dscjssstuniv.in 9100
```

### Initial Thoughts 🤔⚙️📚
Since the number is randomly chosen within an enormous range, brute-forcing is impractical. Instead, we can utilize **Binary Search**, which drastically minimizes the number of attempts required to determine the correct number.

### Challenges ⏳💡🛠️
- **Time Constraint:** The connection times out if we take too long.
- **Large Search Space:** The number is within the range of `10^100`, exceeding standard integer limits in many programming languages.
- **Randomized Number:** Each new connection resets the number, preventing the reuse of previous guesses.

### Solution Approach 🛠️🔮🔄
We use **Binary Search** to systematically narrow the range based on the server’s feedback:
1. Initialize `low = 1` and `high = 10^100`.
2. Compute the middle value: `mid = (low + high) // 2`.
3. Send `mid` to the server and analyze the response:
   - **"Too small"** → Adjust `low = mid + 1`.
   - **"Too large"** → Adjust `high = mid - 1`.
   - **Success message** → Extract the flag and terminate.
4. Repeat until the correct number is found.

### Python Implementation 💻🌐🛠️
Using Python’s `socket` module, we automate the binary search process:

```python
import socket

def binary_search_ctf():
    host = "favno.ctf.dscjssstuniv.in"
    port = 9100
    
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((host, port))
    
    print(s.recv(1024).decode())  # Initial message
    
    low = 1
    high = 10**100  # Upper bound
    
    while low <= high:
        mid = (low + high) // 2
        s.send((str(mid) + "\n").encode())
        
        response = s.recv(1024).decode()
        print(f"Guess: {mid}, Response: {response}")
        
        if "too small" in response:
            low = mid + 1
        elif "too large" in response:
            high = mid - 1
        else:
            print("Found the number! Flag:", response)
            break
    
    s.close()

binary_search_ctf()
```

### Key Takeaways 🏆🎉🔧
- **Binary Search** reduced the number of guesses to approximately **333 attempts**, making it highly efficient.
- **Python’s arbitrary precision integers** (`int` type) allowed seamless handling of extremely large numbers.
- **Automating with sockets** ensured quick and precise interactions with the challenge server.

### Final Flag 🏳️‍🌟💪
After correctly identifying the number, we received the flag:
```
dscctf{y0u_4r3_4_sm4rt_arent_you}
```

---
This approach provides an optimal and efficient method to solve the challenge within the given constraints. ✨🔮🌟

