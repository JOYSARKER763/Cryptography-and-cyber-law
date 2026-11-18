🔐 CT # 1: Practicing Number Theory & Public-Key CryptographySubmitted By: Joy Sarker  |  Student ID: IT23022Course: Cryptography and Cyber Law  |  Assignment: Class Test # 1An analytical and computational exploration of elementary integer properties, modular arithmetic, classical number-theoretic theorems, and end-to-end RSA public-key encryption implemented in Python.📑 Table of ContentsOverview and MotivationCore ObjectivesComputational SetupElementary Integer Properties & DivisibilityPrime Numbers & Prime FactorizationGCD & The Extended Euclidean AlgorithmArithmetic in Modular SystemsLinear Congruences & Modular Multiplicative InversesFermat's Little Theorem & Euler's Totient TheoremThe Chinese Remainder Theorem (CRT)Symmetric vs. Asymmetric Cryptography ArchitectureThe RSA Cryptosystem (Theory & Diagram)Complete End-to-End Python ImplementationStep-by-Step Solved Numerical ProblemsPractical Security ConsiderationsReferences & Summary1. Overview and MotivationNumber theory studies the intrinsic properties of integers—such as divisibility, prime distribution, common divisors, and modular congruences. Historically regarded as pure mathematics, number theory now serves as the cryptographic foundation of modern digital security, securing everything from HTTPS web traffic to digital signatures and blockchain networks.Primary Reference: Adapted and practiced from the Number Theory and Cryptography Workshop — mnp-club.2. Core ObjectivesEstablish formal definitions of divisibility, primality, unique factorization, and modular congruence.Implement the Euclidean and Extended Euclidean algorithms from scratch in Python.Compute Fast Modular Exponentiation ($a^b \bmod n$) in logarithmic time $O(\log b)$.Solve linear congruences and compute Modular Multiplicative Inverses.Apply Fermat's Little Theorem, Euler's Totient Theorem, and the Chinese Remainder Theorem (CRT).Build and verify a complete RSA Key Generation, Encryption, and Decryption pipeline.3. Computational SetupAll experiments were practiced and verified using Google Colab and local Jupyter Notebook environments using standard Python 3 libraries (math, random).# Verify Python environment and launch Jupyter
python3 --version
pip install notebook matplotlib
jupyter notebook
4. Elementary Integer Properties & DivisibilityMathematical FormulationAn integer $a$ divides an integer $b$ (denoted $a \mid b$) if there exists an integer $k$ such that $b = a \cdot k$. According to the Division Algorithm, for any integer $a$ and positive divisor $n > 0$, there exist unique integers $q$ (quotient) and $r$ (remainder) satisfying:$$a = q \cdot n + r, \quad \text{where } 0 \le r < n$$Python Implementationdef division_algorithm(a: int, n: int) -> tuple[int, int]:
    """Computes unique quotient q and remainder r such that a = q*n + r."""
    if n <= 0:
        raise ValueError("Divisor n must be a positive integer.")
    q, r = divmod(a, n)
    return q, r

print("47 divided by 5  ->", division_algorithm(47, 5))   # Output: (9, 2)
print("-17 divided by 5 ->", division_algorithm(-17, 5))  # Output: (-4, 3)
5. Prime Numbers & Prime FactorizationMathematical FormulationAn integer $p > 1$ is prime if its only positive divisors are $1$ and $p$. By the Fundamental Theorem of Arithmetic, every integer $n > 1$ admits a unique prime factorization:$$n = p_1^{e_1} \cdot p_2^{e_2} \cdots p_k^{e_k} = \prod_{i=1}^{k} p_i^{e_i}$$Python Implementation (Trial Division & Sieve of Eratosthenes)def prime_factorization(n: int) -> dict[int, int]:
    """Returns the prime factorization of n as {prime: exponent}."""
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

def sieve_of_eratosthenes(limit: int) -> list[int]:
    """Generates all prime numbers up to a given limit."""
    is_prime = [True] * (limit + 1)
    is_prime[0:2] = [False, False]
    for p in range(2, int(limit**0.5) + 1):
        if is_prime[p]:
            for multiple in range(p * p, limit + 1, p):
                is_prime[multiple] = False
    return [num for num, prime in enumerate(is_prime) if prime]

print("Prime factors of 360:", prime_factorization(360))
print("Primes up to 30:", sieve_of_eratosthenes(30))
6. GCD & The Extended Euclidean AlgorithmMathematical FormulationThe Greatest Common Divisor $\gcd(a, b)$ is the largest positive integer dividing both $a$ and $b$. Two numbers are coprime if $\gcd(a, b) = 1$.By Bézout's Identity, there exist integers $x$ and $y$ such that:$$ax + by = \gcd(a, b)$$Python Implementationdef gcd(a: int, b: int) -> int:
    """Standard Euclidean Algorithm."""
    while b != 0:
        a, b = b, a % b
    return abs(a)

def extended_gcd(a: int, b: int) -> tuple[int, int, int]:
    """Returns (g, x, y) such that a*x + b*y = g = gcd(a, b)."""
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return g, x, y

g, x, y = extended_gcd(56, 15)
print(f"gcd(56, 15) = {g} | Bezout: 56({x}) + 15({y}) = {56*x + 15*y}")
7. Arithmetic in Modular SystemsConcept & Clock Arithmetic VisualizationTwo integers $a$ and $b$ are congruent modulo $n$, written $a \equiv b \pmod n$, if $n \mid (a - b)$. Modular arithmetic wraps integers around a cycle of size $n$:          0 (mod 5)
       4             1
         \         /
          [ MOD 5 ]
         /         \
       3             2
   Example: 17 ≡ 2 (mod 5)
Fast Modular Exponentiation (Binary Exponentiation)Computing $a^b \bmod n$ naively is infeasible for cryptographic exponents. Repeated squaring reduces time complexity to $O(\log b)$:def mod_exp(base: int, exp: int, mod: int) -> int:
    """Computes (base^exp) % mod efficiently in O(log exp) time."""
    result = 1
    base %= mod
    while exp > 0:
        if exp & 1:
            result = (result * base) % mod
        exp >>= 1
        base = (base * base) % mod
    return result

print("7^256 mod 13 =", mod_exp(7, 256, 13))  # Output: 9
8. Linear Congruences & Modular Multiplicative InversesMathematical FormulationA linear congruence $ax \equiv b \pmod n$ has solutions if and only if $d = \gcd(a, n)$ divides $b$. When $\gcd(a, n) = 1$, there exists a unique modular multiplicative inverse $a^{-1} \bmod n$ satisfying:$$a \cdot a^{-1} \equiv 1 \pmod n$$Python Implementationdef mod_inverse(a: int, n: int) -> int:
    """Computes the modular inverse of a modulo n using Extended GCD."""
    g, x, _ = extended_gcd(a, n)
    if g != 1:
        raise ValueError(f"Modular inverse does not exist because gcd({a}, {n}) = {g} != 1")
    return x % n

inv = mod_inverse(3, 11)
print(f"Inverse of 3 mod 11 = {inv} | Verification: (3 * {inv}) % 11 = {(3 * inv) % 11}")
9. Fermat's Little Theorem & Euler's Totient TheoremTheorems & EquationsFermat's Little Theorem: If $p$ is prime and $p \nmid a$, then:
$$a^{p-1} \equiv 1 \pmod p$$Euler's Totient Function $\phi(n)$: Counts positive integers $\le n$ coprime to $n$. For $n = p \cdot q$ (distinct primes):
$$\phi(n) = (p - 1)(q - 1)$$Euler's Theorem: If $\gcd(a, n) = 1$, then:
$$a^{\phi(n)} \equiv 1 \pmod n$$Python Implementationdef euler_totient(n: int) -> int:
    """Computes Euler's Totient Function phi(n)."""
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
print("Euler's Theorem Check (3^24 mod 35):", mod_exp(3, euler_totient(35), 35))  # Output: 1
10. The Chinese Remainder Theorem (CRT)Mathematical FormulationLet $n_1, n_2, \dots, n_k$ be pairwise coprime positive integers and $N = \prod_{i=1}^k n_i$. The system of simultaneous congruences:$$x \equiv a_i \pmod{n_i}, \quad i = 1, 2, \dots, k$$has a unique solution modulo $N$ given by:$$x \equiv \sum_{i=1}^{k} a_i N_i y_i \pmod N, \quad \text{where } N_i = \frac{N}{n_i} \text{ and } N_i y_i \equiv 1 \pmod{n_i}$$Python Implementationdef chinese_remainder_theorem(remainders: list[int], moduli: list[int]) -> int:
    """Solves a system of simultaneous linear congruences using CRT."""
    N = 1
    for m in moduli:
        N *= m
    
    total = 0
    for a_i, n_i in zip(remainders, moduli):
        N_i = N // n_i
        y_i = mod_inverse(N_i, n_i)
        total += a_i * N_i * y_i
    return total % N

# Solve: x ≡ 2 (mod 3), x ≡ 3 (mod 5), x ≡ 2 (mod 7)
x_sol = chinese_remainder_theorem([2, 3, 2], [3, 5, 7])
print("CRT Solution x =", x_sol)  # Output: 23
11. Symmetric vs. Asymmetric Cryptography ArchitectureArchitectural AspectSymmetric CryptographyAsymmetric (Public-Key) CryptographyKey StructureSingle shared secret key ($K$)Mathematically linked key pair ($K_{pub}, K_{priv}$)Computational SpeedVery fast (bit-level operations)Slower (large modular exponentiation)Key DistributionDifficult (requires secure channel)Easy (public key shared openly)Security BasisConfusion & Diffusion (SPN/Feistel)Hard math problems (Integer Factorization, DLP)ExamplesAES-256, ChaCha20, DESRSA, Diffie-Hellman, ECC12. The RSA Cryptosystem (Theory & Diagram)Cryptographic Workflow Diagram+-----------------------------------------------------------------------+
|                        RSA KEY GENERATION                             |
|  1. Select distinct large primes: p, q                                |
|  2. Compute modulus: n = p * q                                        |
|  3. Compute totient: phi(n) = (p - 1)(q - 1)                          |
|  4. Choose public exponent e: 1 < e < phi(n), gcd(e, phi(n)) = 1      |
|  5. Compute private exponent d: d ≡ e^(-1) (mod phi(n))               |
+-----------------------------------------------------------------------+
             |                                           |
             v                                           v
   Public Key: (e, n)                          Private Key: (d, n)
             |                                           |
             v                                           v
+-------------------------+                 +---------------------------+
|      ENCRYPTION         |   Ciphertext    |        DECRYPTION         |
|   C = M^e (mod n)       | --------------> |     M = C^d (mod n)       |
+-------------------------+                 +---------------------------+
Mathematical Proof of CorrectnessSince $e \cdot d \equiv 1 \pmod{\phi(n)}$, there exists an integer $k$ such that $ed = 1 + k\phi(n)$. By Euler's Theorem:$$C^d \equiv (M^e)^d \equiv M^{ed} \equiv M^{1 + k\phi(n)} \equiv M \cdot (M^{\phi(n)})^k \equiv M \cdot 1^k \equiv M \pmod n$$13. Complete End-to-End Python Implementationdef rsa_full_pipeline():
    # 1. Key Generation
    p, q = 61, 53
    n = p * q                 # n = 3233
    phi = (p - 1) * (q - 1)   # phi(n) = 3120
    e = 17                    # coprime to 3120
    d = mod_inverse(e, phi)   # d = 2753

    print("=== RSA KEY PAIR ===")
    print(f"Public Key  (e, n): ({e}, {n})")
    print(f"Private Key (d, n): ({d}, {n})\n")

    # 2. Encryption (Character-by-character ASCII encoding)
    plaintext = "Joy Sarker - IT23022"
    ciphertext = [mod_exp(ord(char), e, n) for char in plaintext]

    print("=== ENCRYPTION ===")
    print("Plaintext :", plaintext)
    print("Ciphertext:", ciphertext, "\n")

    # 3. Decryption
    decrypted_chars = [chr(mod_exp(c, d, n)) for c in ciphertext]
    decrypted_text = "".join(decrypted_chars)

    print("=== DECRYPTION ===")
    print("Recovered :", decrypted_text)
    assert plaintext == decrypted_text, "Decryption mismatch!"

if __name__ == "__main__":
    rsa_full_pipeline()
14. Step-by-Step Solved Numerical ProblemsProblem 1: Modular Multiplicative InverseFind $x$ such that $7x \equiv 1 \pmod{26}$.Apply Euclidean Algorithm:$26 = 3 \times 7 + 5$$7 = 1 \times 5 + 2$$5 = 2 \times 2 + 1$Back-substitute to express $1$ as a linear combination of $7$ and $26$:$1 = 5 - 2(2)$$1 = 5 - 2(7 - 5) = 3(5) - 2(7)$$1 = 3(26 - 3 \times 7) - 2(7) = 3(26) - 11(7)$Reduce modulo $26$:$-11 \equiv 15 \pmod{26} \implies \mathbf{x = 15}$.Verification: $7 \times 15 = 105 = 4(26) + 1 \equiv 1 \pmod{26}$.Problem 2: Hand-Worked RSA Encryption & DecryptionGiven $p = 11, q = 13, e = 7$, encrypt message $M = 9$ and decrypt it back.Compute $n$ and $\phi(n)$:
$$n = 11 \times 13 = 143, \quad \phi(143) = (11 - 1)(13 - 1) = 120$$Find Private Key $d$:
$$7d \equiv 1 \pmod{120} \implies d = 103 \quad (\text{since } 7 \times 103 = 721 = 6 \times 120 + 1)$$Encrypt $M = 9$:
$$C = 9^7 \bmod 143 = 4,782,969 \bmod 143 = \mathbf{48}$$Decrypt $C = 48$:
$$M = 48^{103} \bmod 143 = \mathbf{9}$$15. Practical Security ConsiderationsKey Length Requirements: Small primes used for educational demonstration can be factored instantaneously. Modern standards (NIST) mandate a minimum key size of 2048 bits (or 3072/4096 bits for long-term security).Probabilistic Padding (OAEP): Raw ("textbook") RSA is deterministic—encrypting the same message twice yields identical ciphertexts. Production RSA always applies OAEP (Optimal Asymmetric Encryption Padding) to prevent chosen-ciphertext attacks.Side-Channel Mitigations: Cryptographic libraries implement constant-time modular exponentiation (e.g., Montgomery Ladder) to prevent timing and power-analysis attacks.16. References & SummaryWorkshop Reference: mnp-club/Number_Theory_and_Cryptography_WorkshopTextbook Reference: William Stallings, Cryptography and Network Security: Principles and Practice.Summary: Through this lab assignment, foundational number-theoretic concepts—divisibility, Euclidean GCD, modular inverses, Euler's theorem, CRT, and RSA—were analytically derived and computationally verified in Python.
