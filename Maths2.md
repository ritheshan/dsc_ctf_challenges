# CTF Write-up: Maths2 Challenge (addition.ctf.dscjssstuniv.in) 🎯📜🧮

## Challenge Overview 🎲🤔📌
The challenge requires solving a series of mathematical problems, such as:
- Least Common Multiple (LCM)
- Greatest Common Divisor (GCD)
- Greatest Prime Factor (GPF)

Each round presents a new problem, and we must compute and return the correct answer within a limited time. To automate this process, we used a Python script leveraging `pwntools`. 🚀🔢💻

---

## Solution Approach ⚙️🔍🛠
We created a script that:
1. **Connects** to the challenge server.
2. **Parses** the mathematical question.
3. **Computes** the required value.
4. **Submits** the answer back to the server.
5. **Repeats** this for multiple rounds until the flag is obtained. 🏆🎖🎯

---

## Exploit Script 🐍💡📜
```python
from pwn import remote
import math
import sympy

# Function to calculate GCD
def calculate_gcd(a, b):
    return math.gcd(a, b)

# Function to calculate the greatest prime factor
def calculate_greatest_prime_factor(n):
    return max(sympy.factorint(n).keys())

# Function to calculate LCM
def calculate_lcm(a, b):
    return abs(a * b) // math.gcd(a, b)

# Connect to the server and solve the challenge
def solve_challenge():
    host = "addition.ctf.dscjssstuniv.in"
    port = 9019

    conn = remote(host, port)  # Establish connection

    for _ in range(100):  # Loop through 100 rounds
        conn.recvuntil(b"Current round:")
        conn.recvline()  # Skip round number line
        
        # Receive the challenge prompt
        prompt = conn.recvuntil(b":").decode().strip()
        print("Prompt:", prompt)

        # Extract numbers and compute results
        if "greatest common divisor" in prompt:
            a, b = map(int, prompt.split(" of ")[1].split(" and ")[0].split())
            result = calculate_gcd(a, b)
        elif "greatest prime factor" in prompt:
            n = int(prompt.split(" of ")[1].split(":")[0])
            result = calculate_greatest_prime_factor(n)
        elif "least common multiple" in prompt:
            a, b = map(int, prompt.split(" of ")[1].split(" and ")[0].split())
            result = calculate_lcm(a, b)
        else:
            print("Unknown prompt:", prompt)
            break

        # Send the computed result
        conn.sendline(str(result).encode())
        print(f"Sent: {result}")

        # Receive and print the response
        response = conn.recvline().decode().strip()
        print(response)

    # Capture the final flag
    final_response = conn.recvall().decode().strip()
    print("Final response:", final_response)
    conn.close()

if __name__ == "__main__":
    solve_challenge()
```

---

## Explanation of the Script 📝🔢🤖
- **`remote(host, port)`**: Establishes a connection with the server.
- **Loops Through 100 Rounds**: Handles multiple challenges in succession.
- **Extracts Numbers from Prompt**: Parses the input string to retrieve numbers.
- **Calculates Required Values**:
  - `math.gcd(a, b)` for GCD.
  - `max(sympy.factorint(n).keys())` for GPF.
  - `abs(a * b) // math.gcd(a, b)` for LCM.
- **Sends Computed Answer**: Submits the solution to the server.
- **Receives and Prints Response**: Checks if the answer is correct.
- **Retrieves the Flag**: The flag is printed once all challenges are solved. 🏁📡✅

---

## Obtained Flag 🎉🏆🎯
```
dscctf{maths2_prolly_fun_lollz!!!}
```

---

## Lessons Learned 🎓📖🤹‍♂️
- **Automating calculations saves time** when dealing with repetitive mathematical challenges.
- **Using `pwntools` makes interaction with remote servers easier** in CTFs.
- **Efficient string parsing is crucial** for extracting problem statements correctly. 🔍⏳🧩

---

*Time to level up the game, huh!* 🎮📢🚀

