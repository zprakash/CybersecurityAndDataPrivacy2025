# The Booking System – Phase 2: Password Cracking Report

This report documents the process of cracking passwords for users in the "The Booking System", along with answers to the security-related questions.

---

## Cracked Passwords

The initial list of database users and their corresponding password hashes was obtained (see pic below). The password hashes were determined to be **MD5** hashes (Hash Mode **0** in Hashcat).

<img src="./Lisst of db users.png" alt="My image" >

At least five user passwords were successfully cracked using **Hashcat** with a dictionary attack (`-a 0` or `-a 3` for a combined attack) and/or a mask attack, primarily utilizing the **rockyou.txt** wordlist.

### 1. `doh@springfieldpower.net`

* **Hash:** `d736fc82effd704296b5bbcff45f323e`
* **Cracked Password:** `donuts4life`

**Explanation:** The password was cracked using Hashcat with a dictionary attack (`-a 0`) against the **rockyou.txt** wordlist.

<img src="./Password crack using hashcat second email  doh@springfieldpower.net.png" alt="My image" >

---

### 2. `genius@starkindustries.com`

* **Hash:** `d50a4d03e42e17e9fa9ec29f89708`
* **Cracked Password:** `iamironman`

**Explanation:** The password was cracked using Hashcat with a dictionary attack (`-a 0`) against the **rockyou.txt** wordlist, suggesting it was a simple, common dictionary entry.

<img src="./cracked password of genius@starkindustries.com.png" alt="My image" >

---

### 3. `testuser1@gmail.com`

* **Hash:** `25d55ad283aa480a4f64c76d713c97ad`
* **Cracked Password:** `12345678`

**Explanation:** This password was cracked using a simple dictionary attack (`-a 0`) against the **rockyou.txt** wordlist. It is an extremely common, weak password.

<img src="./cracked password of testuser1@gmail.com.png" alt="My image" >

---

### 4. `whysoserious@gothamchaos.net`

* **Hash:** `f158d479ee181aac68b000a60e7a3d7a`
* **Cracked Password:** `chaos123!`

**Explanation:** The password was cracked using Hashcat with a combination attack (`-a 3`), where the base word was likely from the dictionary and numbers/symbols were appended based on a mask (`?d?d?d?s`).

<img src="./crackedd password of whysoserious@gothamchaos.net.png" alt="My image" >

---

### 5. `elementary@221bbaker.uk`

* **Hash:** `12c9ce6fb6b91c42b363b4cf02d8bb`
* **Cracked Password:** `deduction221B`

**Explanation:** The password was cracked using a Hashcat mask attack (`-a 3`) that likely combined a dictionary word (the first part of the password) with a mask representing numbers and letters. The screenshot shows Hashcat using the combination attack and the cracked password.

<img src="./hashcat using 3 for elementary@221bbaker.uk.png" alt="My image" >


---

### 6. Attack using Hydra

An attempt was also made to perform an **online dictionary attack** against a web login form using **Hydra**.

<img src="./attack trying using hydra.png" alt="My image" >

### Also used crunch to filter the total number of operations

<img src="./crunch demo.png" alt="My image" >

### Passsword guessing with hydra last two letters unknown'

<img src="./password guessing hyrda last tep letters.png" alt="My image" >

### Finally, password was cracked

<img src="./pass found.png" alt="My image" >

The target was `whatsupdoc@looneytunes.tv` against a locally hosted login page. This demonstrates an attack against a live system, which is much slower and subject to potential network/server-side defenses.

<img src="./cracked of whatsupdoc@looneytunes.tv.png" alt="My image" >

A successful online brute-force attempt was also documented against `doh@springfieldpower.net`, finding the last two character of password `fe` 

## Security Analysis Questions

### What is the main difference between Dictionary and Non-Dictionary attacks?

* **Dictionary Attacks:** Attempt to crack a password by comparing the target hash against a pre-compiled list of common passwords, words, phrases, and leaked credentials (the **dictionary**). They are fast but only effective if the password is on the list.
* **Non-Dictionary Attacks (e.g., Brute-Force, Mask, Hybrid):**
    * **Brute-Force:** Attempts every possible character combination within a defined length and character set. This is guaranteed to find the password eventually but is extremely time-consuming for long passwords.
    * **Mask Attack:** Uses a defined pattern (or **mask**) to limit the character space, such as targeting passwords known to contain a dictionary word followed by three numbers (e.g., `word?d?d?d`).
    * **Hybrid Attack:** Combines dictionary words with non-dictionary rules (e.g., appending numbers, capitalizing the first letter) to create variations.

**In essence, Dictionary attacks test *known* weak passwords, while Non-Dictionary attacks test *generated* permutations.**

### What advantage does an attacker gain by having access to the system’s database that reveals the users and the password hashes?

The primary advantage is that the attacker can perform the crack attempt **offline**. This provides several critical benefits:

1.  **Unlimited Attempts:** The attacker is not subject to any online defenses like account lockout policies, CAPTCHAs, or rate limiting. They can attempt billions of passwords without detection.
2.  **Scalability and Speed:** Cracking can be done using powerful, dedicated hardware (like GPUs) that is significantly faster than performing online login attempts, which are limited by network latency and server response time.
3.  **Targeted Cracking:** The attacker knows the **hash type** (MD5 in this case) and can choose the most efficient cracking method. Furthermore, having the usernames can facilitate targeted **social engineering** or informed guesswork (e.g., guessing a user's password contains their name, birthdate, or role).

### What concrete security benefits are achieved by using longer passwords instead of shorter ones?

The security benefits are achieved by exponentially increasing the **search space** for an attacker, making brute-force and mask attacks computationally infeasible.

* **Increased Entropy:** For every character added, the number of possible passwords increases exponentially, making the password have higher **entropy** (randomness).
    * *Example:* If a password uses 95 possible characters (upper/lower/numbers/symbols), a 6-character password has $95^6 \approx 7.35 \times 10^{11}$ possibilities. A 10-character password has $95^{10} \approx 5.98 \times 10^{19}$ possibilities.
* **Time Complexity:** The time required for an attacker to successfully brute-force the hash grows exponentially. A modern GPU rig might crack an 8-character password in hours or days, but a 16-character password (using the same character set) would take **trillions of years**.
* **Defense Against Dictionary/Mask Attacks:** Longer passwords are less likely to be fully covered by the simple permutations or masks typically used in common dictionary/hybrid attacks.

In short, longer passwords significantly increase the **time and cost** for an attacker to crack the password, often making the attack impractical.

---


