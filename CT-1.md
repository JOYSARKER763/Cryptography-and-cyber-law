Contents OverviewOverview and MotivationCore ObjectivesComputational SetupElementary Integer PropertiesPrimes and Prime DecompositionGCD and the Euclidean MethodArithmetic in Modular SystemsLinear Congruences and Multiplicative InversesFermat's and Euler's Classical TheoremsThe Chinese Remainder Theorem (CRT)Principles of Modern CryptographyThe RSA CryptosystemInteractive Python Lab ExperimentsStep-by-Step Solved ProblemsPractical Security ConsiderationsSummary of Acquired Skills1. Overview and MotivationNumber theory focuses on the structure and behavior of whole numbers. Topics like divisibility rules, prime distribution, common divisors, and remainder arithmetic form the theoretical backbone of this field. While historically viewed as pure mathematics, these concepts now power the security infrastructure of the modern internet.Cryptography applies mathematical hardness to protect digital data—ensuring privacy, data integrity, and sender verification. Unlike traditional secret-key methods, asymmetric (public-key) cryptography relies on a mathematically linked key pair: one key shared openly for encryption, and a matching secret key kept private for decryption.This report bridges theory and implementation by translating foundational number-theoretic theorems into clean Python scripts supported by verifiable numerical tests.Reference Material: Adapted from the Number Theory and Cryptography Workshop — mnp-club.Note: This documentation serves as an independent study compilation. Course-specific tasks should be cross-referenced with official class materials.2. Core ObjectivesUpon reviewing and executing the code modules in this study, one will be able to:Define integer divisibility, primality, unique factorization, and modular equivalence.Code both the standard and extended versions of the Euclidean Algorithm from scratch.Compute fast modular exponentiation and find modular multiplicative inverses.Apply Fermat's Little Theorem, Euler's Totient Theorem, and the Chinese Remainder Theorem to solve congruence systems.Contrast symmetric encryption with asymmetric public-key architectures.Execute a complete end-to-end RSA key generation, encryption, and decryption cycle in Python.3. Computational SetupAll scripts are written in standard Python 3 and require no external cryptographic packages. They can be executed locally via Jupyter or directly in the cloud using Google Colab.Local Environment Setup:python3 --version
pip install notebook matplotlib
jupyter notebook
4. Elementary Integer PropertiesAn integer $a$ divides another integer $b$ (written $a \mid b$) if there exists an integer $k$ such that $b = a \cdot k$. By the Division Algorithm, for any integer $a$ and positive integer $n$, there exist unique integers $q$ (quotient) and $r$ (remainder) such that:$$a = q \cdot n + r, \quad 0 \le r < n$$def division_algorithm(a: int, n: int):
    if n <= 0:
        raise ValueError("Divisor n must be positive.")
    q, r = divmod(a, n)
    return q, r

print(division_algorithm(47, 5))   # Output: (9, 2)
print(division_algorithm(-17, 5))  # Output: (-4, 3)
5. Primes and Prime DecompositionA prime number is an integer $p > 1$ whose only positive divisors are $1$ and $p$. According to the Fundamental Theorem of Arithmetic, every integer $n > 1$ can be uniquely represented as a product of prime powers:$$n = p_1^{e_1} p_2^{e_2} \cdots p_k^{e_k}$$def prime_factorization(n: int) -> dict:
    factors = {}
    d = 2
    while d * d <= n:
        while n % d == 0:
            factors[d] = factors.get(d, 0) + 1
            n //= d
        d += 1
    if n > 1:
        factors[n] = factors.get(n, 0) + 1
    return factors

print("Prime factors of 360:", prime_factorization(360))
# Output: {2: 3, 3: 2, 5: 1}
6. GCD and the Euclidean MethodThe Greatest Common Divisor (GCD) of two integers $a$ and $b$, denoted $\gcd(a, b)$, is the largest positive integer dividing both. If $\gcd(a, b) = 1$, they are coprime.The Extended Euclidean Algorithm also finds integers $x$ and $y$ satisfying Bézout's identity: $ax + by = \gcd(a, b)$.def extended_gcd(a: int, b: int):
    if b == 0:
        return a, 1, 0
    gcd, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return gcd, x, y

g, x, y = extended_gcd(56, 15)
print(f"GCD: {g}, x: {x}, y: {y}")  # 56(-4) + 15(15) = 1
7. Arithmetic in Modular SystemsTwo integers $a$ and $b$ are congruent modulo $n$, written $a \equiv b \pmod n$, if $n \mid (a - b)$. Modular arithmetic preserves addition, subtraction, and multiplication, enabling Fast Modular Exponentiation ($a^b \bmod n$) in $O(\log b)$ time.def mod_exp(base: int, exp: int, mod: int) -> int:
    result = 1
    base %= mod
    while exp > 0:
        if exp % 2 == 1:
            result = (result * base) % mod
        exp //= 2
        base = (base * base) % mod
    return result

print("7^256 mod 13 =", mod_exp(7, 256, 13))  # Output: 9
8. Linear Congruences and Multiplicative InversesA linear congruence $ax \equiv b \pmod n$ has a solution if and only if $d = \gcd(a, n)$ divides $b$. When $\gcd(a, n) = 1$, $a$ has a unique modular multiplicative inverse $a^{-1} \bmod n$ such that $a \cdot a^{-1} \equiv 1 \pmod n$.def mod_inverse(a: int, n: int) -> int:
    g, x, _ = extended_gcd(a, n)
    if g != 1:
        raise ValueError(f"Inverse does not exist since gcd({a}, {n}) != 1")
    return x % n

inv = mod_inverse(3, 11)
print(f"Inverse of 3 mod 11 is {inv} (Check: {(3 * inv) % 11})")
9. Fermat's and Euler's Classical TheoremsFermat's Little Theorem: If $p$ is prime and $p \nmid a$, then $a^{p-1} \equiv 1 \pmod p$.Euler's Totient Function $\phi(n)$: Counts integers in $[1, n]$ coprime to $n$. For $n = p \cdot q$ (distinct primes), $\phi(n) = (p-1)(q-1)$.Euler's Theorem: If $\gcd(a, n) = 1$, then $a^{\phi(n)} \equiv 1 \pmod n$.def euler_totient(n: int) -> int:
    result = n
    p = 2
    temp = n
    while p * p <= temp:
        if temp % p == 0:
            while temp % p == 0:
                temp //= p
            result -= result // p
        p += 1
    if temp > 1:
        result -= result // temp
    return result

print("phi(35) =", euler_totient(35))  # Output: 24
10. The Chinese Remainder Theorem (CRT)Given pairwise coprime moduli $n_1, n_2, \dots, n_k$, the system of congruences $x \equiv a_i \pmod{n_i}$ has a unique solution modulo $N = n_1 n_2 \cdots n_k$:$$x = \sum_{i=1}^{k} a_i N_i y_i \pmod N, \quad \text{where } N_i = \frac{N}{n_i} \text{ and } N_i y_i \equiv 1 \pmod{n_i}$$def chinese_remainder_theorem(remainders: list, moduli: list) -> int:
    N = 1
    for m in moduli:
        N *= m
    total = 0
    for a_i, n_i in zip(remainders, moduli):
        N_i = N // n_i
        y_i = mod_inverse(N_i, n_i)
        total += a_i * N_i * y_i
    return total % N

# Solve x ≡ 2 (mod 3), x ≡ 3 (mod 5), x ≡ 2 (mod 7)
print("CRT Solution x =", chinese_remainder_theorem([2, 3, 2], [3, 5, 7]))  # Output: 23
11. Principles of Modern CryptographyFeatureSymmetric CryptographyAsymmetric (Public-Key) CryptographyKeys UsedSingle shared secret keyMathematically linked Public & Private keysSpeedVery fast (AES, ChaCha20)Slower due to large integer arithmetic (RSA, ECC)Key DistributionRequires secure key exchange channelPublic key can be distributed openlyPrimary Use CaseBulk data encryptionKey exchange, digital signatures, authentication12. The RSA CryptosystemRSA security relies on the computational difficulty of factoring the product of two large primes:Key Generation: Choose distinct primes $p, q$. Compute $n = p \cdot q$ and $\phi(n) = (p-1)(q-1)$. Select $e$ such that $1 < e < \phi(n)$ and $\gcd(e, \phi(n)) = 1$. Compute $d \equiv e^{-1} \pmod{\phi(n)}$.Public Key: $(e, n)$Private Key: $(d, n)$Encryption: $C \equiv M^e \pmod n$Decryption: $M \equiv C^d \pmod n$13. Interactive Python Lab ExperimentsBelow is a complete, end-to-end RSA implementation demonstrating string-to-integer encoding, encryption, and decryption:def rsa_demo():
    # Step 1: Choose two distinct primes
    p, q = 61, 53
    n = p * q                # 3233
    phi = (p - 1) * (q - 1)  # 3120
    e = 17                   # Public exponent coprime to 3120
    d = mod_inverse(e, phi)  # Private exponent: 2753

    print(f"Public Key  (e, n): ({e}, {n})")
    print(f"Private Key (d, n): ({d}, {n})")

    # Step 2: Encrypt a message character by character
    message = "IT23022"
    ciphertext = [mod_exp(ord(ch), e, n) for ch in message]
    print("Original Message :", message)
    print("Encrypted Blocks :", ciphertext)

    # Step 3: Decrypt back to plaintext
    decrypted = "".join(chr(mod_exp(c, d, n)) for c in ciphertext)
    print("Decrypted Message:", decrypted)

rsa_demo()
14. Step-by-Step Solved ProblemsProblem 1: Modular Inverse
Find $x$ such that $7x \equiv 1 \pmod{26}$.Apply Extended Euclidean Algorithm: $26 = 3(7) + 5$, $7 = 1(5) + 2$, $5 = 2(2) + 1$.Back-substitute: $1 = 5 - 2(2) = 5 - 2(7 - 5) = 3(5) - 2(7) = 3(26 - 3 \cdot 7) - 2(7) = 3(26) - 11(7)$.Taking modulo $26$: $-11 \equiv 15 \pmod{26}$. Thus, $x = 15$ (Check: $7 \times 15 = 105 = 4 \times 26 + 1$).Problem 2: Small RSA Hand Calculation
Let $p = 11, q = 13, e = 7$. Encrypt $M = 9$.$n = 11 \times 13 = 143$, and $\phi(n) = 10 \times 12 = 120$.Private key $d \equiv 7^{-1} \pmod{120} \implies d = 103$ since $7 \times 103 = 721 = 6(120) + 1$.Ciphertext: $C = 9^7 \bmod 143 = 48$.Decryption: $M = 48^{103} \bmod 143 = 9$.15. Practical Security ConsiderationsKey Size: Textbook small primes are trivially factored. Production RSA requires at least 2048-bit to 4096-bit moduli.Padding Schemes: Deterministic raw RSA is vulnerable to chosen-ciphertext attacks; real-world deployments must use probabilistic padding such as OAEP (Optimal Asymmetric Encryption Padding).Side-Channel Resistance: Implementations must use constant-time modular exponentiation to prevent timing and power-analysis attacks.16. Summary of Acquired SkillsTranslated formal algebraic properties (divisibility, Bézout's identity, modular congruences) into clean, modular Python functions.Implemented fast binary exponentiation and the Extended Euclidean Algorithm without relying on external libraries.Built and verified a functional asymmetric RSA encryption/decryption pipeline alongside Chinese Remainder Theorem solvers.
